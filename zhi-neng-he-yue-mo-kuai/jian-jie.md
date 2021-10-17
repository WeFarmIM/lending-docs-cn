---
description: 核心智能合约简介
---

# 简介

## 智能合约

协议中有 5 个核心合约, `SavingAccount`, `Accounts`, `Bank`, `TokenRegistry` 和 `GlobalConfig`.

前三个合约是可升级的，它们包含协议的主要业务逻辑。协议的结构是整体的，这意味着 `SavingAccount` 、 `Accounts` 和 `Bank` 合约需要相互依赖，紧密合作。出于代码大小限制的原因，我们将其解耦为三个合约。这里的命名可能有点违反直觉。通常，当我们想存钱到银行时，我们先去银行开一个储蓄账户，然后我们把钱存入这个账户。在这里， `SavingAccount` 合约的行为类似于传统银行，它是你从 WeFarm 存款或借款的入口。Bank 合约记录 WeFarm 协议池的状态，如总存款、总借款和当前利率。Accounts 合约记录每个特定用户的存款和借款余额。

WeFarm 事务的工作流程如下图所示。用户通过调用`SavingAccount`合约上的函数来发送交易，然后 `SavingAccount` 合约将调用 `Bank` 合约来创建一个利率指数检查点，该检查点将为所有用户计算累积利息。然后， `Bank` 合约将调用 `Accounts` 合约中的函数来计算用户帐户中的余额变化。

![交易流程](broken-reference)

`TokenRegistry` 和 `GlobalConfig` 合约用于存储数据。其他三个合约将使用其中的数据。为此在其他三个合约中，需要在合约的内存空间中保留全局配置的地址。

## 储蓄账户合约 SavingAccount

This contract is the interface that our users will mainly interact with. Users will send transactions to this contract to `deposit`, `withdraw`, `borrow`, `repay`, `withdrawAll`, and `liquidate` functions to interact with our protocol. For each operation, this contract is the contract that receives and sends out the tokens. Whenever users send a transaction to the `SavingAccount` contract, it will emit an event to log this transaction.

用户将主要与这个合约交互。用户将发送交易到本合约的 `存款`、`取款`、`借款`、`偿还`、`取款墙` 和 `清算`功能，与我们的协议进行交互。对于每个操作，这个合约是接收和发送代币的合约。每当用户向 `SavingAccount` 合约发送事务时，它将发出一个事件来记录该事务。

### 偿贷能力 Borrowing Power

用户的借贷能力, 也称为偿贷能力, 与用户在系统中存入的抵押品有关。合约从 ChainLink 的预言机获得价格。如果用户的 LTV 太大，那么它就有被清算的风险。目前，我们将清算门槛设定在 0.8 左右。

## 银行合约 Bank

这个合约将在与我们的 `SavingAccount` 合约发生交互时创建利率指数。我们使用利率指数来计算借款和存款利息。给定不同的索引，我们在这里计算用户余额。当合约创建索引时，该合约将发出一个事件。

### Rate Index

i 和 j 代表两个不同区块

$$RateIndex_i = RateIndex_j \times (1 + RatePerBlock_{ij} \times (i - j))$$

### Balance

i 和 j 代表两个不同区块

$$Balance_i = Balance_j \times RateIndex_j \div RateIndex_i$$

借款余额和存款余额都使用这种方法来计算。

## 账户合约 Accounts

该合约将记录不同代币的每个用户账户的余额。该合约还包含一个位图，该位图将快速显示用户是否对每个代币有存款/借款，同时 gas 成本更低。

## 全局配置合约 GlobalConfig

这个合约存储了所有合约的地址，所以所有其他合约只能保留 GlobalConfig 的地址来调用其他合约的函数。它还用于配置协议的准备金率和社区基金率。

## 代币注册合约 TokenRegistry

这个合约记录关于每个合约支持的代币的元数据。我们可以使用此合约添加新的受支持的代币。

### 当前支持的代币

1. DAI
   1. Token Address: 0x6b175474e89094c44da98b954eedeac495271d0f
   2. cToken Address: 0x5d3a536e4d6dbd6114cc1ead35777bab948e3643
2. USDC
   1. Token Address: 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48
   2. cToken Address: 0x39aa39c021dfbae8fac545936693ac917d5e7563
3. USDT
   1. Token Address: 0xdac17f958d2ee523a2206206994597c13d831ec7
   2. cToken Address: 0xf650c3d88d12db855b8bf7d11be6c55a4e07dcc9
4. TUSD
   1. Token Address: 0x0000000000085d4780B73119b644AE5ecd22b376
   2. cToken Address: Not Compound supported.

## 可升级支持

对于 `SavingAccount`、`Accounts` 和 `Bank` 合约，我们使用 `OpenZeppelin` 代理来执行升级过程。所以除了这些合约，我们还为这三个中心合约每个都有一个 `OpenZeppelin` 代理合约。我们永远不能改变代理合约地址，但是底层实现的合约是可以改变的。

![代理合约工作流程](broken-reference)

对于 `GlobalConfig` 和 `TokenRegistry` 合约，我们可以部署一个新合约，并在其他三个合约中调用 `setter` 函数来指向新合约，因为它们不保存任何事务数据。
