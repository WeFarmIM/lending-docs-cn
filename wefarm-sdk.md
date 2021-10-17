# WeFarm SDK

## 简介

目前，我们在主网上使用 `web3.js` 与我们的合同进行交互。如果我们的合约有变化，我们需要在不同的开发人员开发的不同的项目中处处改变这个变化，这会造成很多时间浪费。目前，我们的 web 应用、移动应用、liquidator bot，甚至在测试中都采用了这种方法。如果我们构建一个 SDK，我们将节省大量的时间，可以在所有这些文件中使用，它可以更多地解耦组件，缩短时间，以协调升级我们的智能合约。

例子:

1. Compound: [https://github.com/compound-finance/compound-js](https://github.com/compound-finance/compound-js)
2. DyDx: [https://github.com/dydxprotocol/solo](https://github.com/dydxprotocol/solo)

## 目标

### 例子

我们只需要通过提供 provider 信息, 就可以与合约进行交互。用法示例如下:

1. 安装依赖

* 使用 npm 安装依赖

```bash
npm i -s @WeFarm/wefarm-protocol
```

* 使用 yarn 安装依赖

```bash
yarn add @WeFarm/wefarm-protocol
```

2\. 初始化 WeFarm 实例

```js
import WeFarm from "@WeFarm/wefarm-protocol";

var wefarm = new WeFarm(window.ethereum); // web 浏览器

var wefarm = new WeFarm("http://127.0.0.1:8545"); // HTTP provider

var wefarm = new WeFarm(); // Uses Ethers.js fallback mainnet (for testing only)

var wefarm = new WeFarm("ropsten"); // Uses Ethers.js fallback (for testing only)

// 使用 private key 初始化 (服务器端使用)
var wefarm = new WeFarm("https://mainnet.infura.io/v3/_your_project_id_", {
  privateKey: "0x_your_private_key_", // 最好使用环境变量传入
});

// 使用助记词初始化 (服务器端使用)
var wefarm = new WeFarm("mainnet", {
  mnemonic: "cyber punk game...", // 最好使用环境变量传入
});
```

3\. 使用连接实例与合约交互

```js
console.log(WeFarm.address.DAI, WeFarm.address.cETH); // 获取当前使用的 Compound 账户

await wefarm.borrow(WeFarm.addresses.DAI, new BN(10).pow(new BN(18))); // 从 WeFarm 协议借 DAI
```

4\. 使用实例与服务端 API 交互

```js
// 获取某账户的 LTV
const accLtv = await wefarm.API.ltv(address);
```

## 模块

WeFarm

这是 SDK 的主要对象。用户应该提供一个 provider 来创建 WeFarm 对象，然后使用这个实例与智能合约进行交互。在设置 provider 之后，我们可以在 provider 中指定一个可用的帐户来与合约交互。这应该是一个单例。默认情况下，它将使用 provider 中的第一个帐户来进行所有交易。

### ContractInstance 合约实例

通过指定合约的地址和 ABI，返回相应的合约实例。

### ContractInstanceStore 合约实例管理器

我们可以使用这个对象来获取所有不同的合约实例。我们确保每个合约在同一应用程序中只有一个实例，以确保性能。

### AccountsInstance

这是 Accounts 合约的包装类，我们可以使用它与链上的 Accounts 合约进行交互。

### SavAccInstance

这是 SavingAccount 合约的包装类，我们可以使用它与链上的 SavingAccount 合约进行交互。

### BankInstance

这是 Bank 合约的包装类，我们可以使用它与链上的 Bank 合约进行交互。

### Constants 常量

该文件包含 SDK 将使用的所有常量。比如我们的协议在不同的测试网和主网的地址，以及每个不同合同的最新 ABIs。

### Utils 工具

一些公用工具，可以帮助从第三方来源获得一些必要的数据，如实时 gas 气价格。

## API

### 智能合约相关

公有 API

* wefarm.userHasAnyDeposits(user: string, target: string):
  * Description: 返回 目标账号是否存款.
  * Parameters:
    * user: The address of the user who tries to call this function.
    * target: The address of the target that the user wants to query.
* wefarm.getDepositPrincipal(tokenName: string, user: string, target: string):
  * Description: 返回 账号指定币种的存款金额.
  * Parameters:
    * tokenName: The token's name that you want to query, please check what token names are supported in the **Token Names** section.
    * user: The address of the user who tries to call this function.
    * target: The address of the target that the user wants to query.
* wefarm.getBorrowPrincipal(tokenName: string, user: string, target: string):
  * Description: 返回 账号指定币种的借款金额.
  * Parameters:
    * tokenName: See **Token Names** section.
    * user: The address of the user who tries to call this function.
    * target: The target address that the user wants to query.
* wefarm.getLastDepositBlock(tokenName: string, user: string, target: string):
  * Description: 返回 账号指定币种的存款区块.
  * Parameters:
    * tokenName: See **Token Names** section.
    * user: The address of the user who tries to call this function.
    * target: The target address that the user wants to query.
* wefarm.getLastBorrowBlock(tokenName: string, user: string, target: string):
  * Description: 返回 账号指定币种的借款区块.
  * Parameters:
    * tokenName: See **Token Names** section.
    * user: The address of the user who tries to call this function.
    * target: The target address that the user wants to query.
* wefarm.getDepositInterest(tokenName: string, user: string, target: string):
  * Description: 返回 存款利息.
    * Parameters:
      * tokenName: See **Token Names** section.
      * user: The address of the user who tries to call this function.
      * target: The target address that the user wants to query.
* wefarm.getBorrowInterest(tokenName: string, user: string, target: string):
  * Description: 返回 借款利息.
    * Parameters:
      * tokenName: See **Token Names** section.
      * user: The address of the user who tries to call this function.
      * target: The target address that the user wants to query.
* wefarm.getDepositBalanceCurrent(tokenName: string, user: string, target: string):
  * Description: 返回 用户某币种的总存款金额.
    * Parameters:
      * tokenName: See **Token Names** section.
      * user: The address of the user who tries to call this function.
      * target: The target address that the user wants to query.
* wefarm.getBorrowBalanceCurrent(tokenName: string, user: string, target: string):
  * Description: 返回 用户某币种的总借款金额.
    * Parameters:
      * tokenName: See **Token Names** section.
      * user: The address of the user who tries to call this function.
      * target: The target address that the user wants to query.
* wefarm.getBorrowPower(user: string, target: string):
  * Description: 返回 用户偿债能力(wei).
  * Parameters:
    * user: The address of the user who tries to call this function.
    * target: The target address that the user wants to query.
* wefarm.getDepositETH(user: string, target: string):
  * Description: 返回 用户 ETH 存款金额(wei).
  * Parameters:
    * user: The address of the user who tries to call this function.
    * target: The target address that the user wants to query.
* wefarm.getBorrowETH(user: string, target: string):
  * Description: 返回 用户 ETH 借款金额(wei).
  * Parameters:
    * user: The address of the user who tries to call this function.
    * target: The target address that the user wants to query.
* wefarm.isAccountLiquidatable(user: string, target: string):
  * Description: 返回 用户账号是否处于可清算状态.
  * Parameters:
    * user: The address of the user who tries to call this function.
    * target: The target address that the user wants to query.

私有 API (默认账号为 provider 中第一个账号)

* wefarm.deposit(token: Token, amount: any):
  * Description: 存款
  * Parameters:
    * token: See **Token Names** section.
    * amount: The amount defined in BigNumber type.
* wefarm.borrow(token: Token, amount: any): Borrow an amount of token from WeFarm.
  * Description: 借款
    * Parameters:
      * token: See **Token Names** section.
      * amount: The amount defined in BigNumber type.
* wefarm.repay(token: Token, amount: any): Repay an amount of token to WeFarm.
* Description: 还贷款
  * Parameters:
    * token: See **Token Names** section.
    * amount: The amount defined in BigNumber type.
* wefarm.withdraw(token: Token, amount: any): Withdraw an amount from WeFarm.
  * Description: 提币
    * Parameters:
      * token: See **Token Names** section.
      * amount: The amount defined in BigNumber type.
* wefarm.withdrawAll(token: Token):
  * Description: 提所有币
    * Parameters:
      * token: see **Token Names** section.
      * amount: The amount defined in BigNumber type.
* wefarm.getTotalDepositStore(tokenName:string):
  * Description: 代币总存款
  * Parameters:
    * tokenName: See **Token Names** section.
* wefarm.getBorrowRatePerBlock(tokenName: string):
  * Description: 代币借款率
  * Parameters:
    * tokenName: See **Token Names** section.
* wefarm.getDepositRatePerBlock(tokenName:string):
  * Description: 代币存款率
  * Parameters:
    * tokenName: See **Token Names** section.
* wefarm.getCapitalUtlizationRatio(tokenName: string):
  * Description: 代币 U 值
  * Parameters:
    * tokenName: See **Token Names** section.
* wefarm.getCapitalCompoundRatio(tokenName: string):
  * Description: 代币 C 值
  * Parameters:
    * tokenName: See **Token Names** section.
* wefarm.getTokenState(tokenName: string):
  * Description: 代币状态
  * Parameters:
    * tokenName: See **Token Names** section.
* wefarm.getPoolAmount(tokenName: string):
  * Description: 合约中代币资产数
    * Parameters:
      * tokenName: See **Token Names** section.

### 代币名称

* wefarm-js 支持的代币:
  * BAT
  * DAI
  * ETH
  * REP
  * USDC
  * USDT
  * WBTC
  * ZRX
  * MKR
  * FIN
  * LPToken: The WeFarm liquidity provider token related to the uniswap.
  * TUSD
* 我们可以使用上面的名称作为字符串作为参数传递，或者当它需要一个 Token 类型参数时，我们可以使用 enum Token 类型。
* enum Token 类型定义为:

```js
export enum Token {
    BAT = 1,
    DAI,
    ETH,
    REP,
    USDC,
    USDT,
    WBTC,
    ZRX,
    MKR,
    FIN,
    LPToken,
    TUSD
}
```

### 状态相关

详细信息请参考: https://app.gitbook.com/@wefarm/s/front-end/api-info-about-stat.wefarm.cn

* wefarm.API.statusAssets(): Call `/api_v2/address/status_assets`
* wefarm.API.balances(): Call `/api_v2/address/balances`
* wefarm.API.ltv(): Call `/api_v2/address/ltv`
* wefarm.API.balanceLog(): Call `/api_v2/address/balance_log`
* wefarm.API.getSavingsOrder(): Call `/api_v2/address/get_savings_order`
* wefarm.API.tokenStatus(): Call `/api_v2/market/token_status`
* wefarm.API.tokenStatistical(): Call `/api_v2/market/token_statistical`
* wefarm.API.tokenPrice(): Call `/api_v2/market/token_prices`
* wefarm.API.totalAssets(): Call `/api_v2/market/total_assets`
* wefarm.API.addressList(): Call `/api_v2/market/address_list`
