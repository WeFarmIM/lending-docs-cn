# 协议简介

WeFarm 储蓄协议将从贷款人到智能合约的加密存款聚集起来，供用户以自己存入的抵押品资产为抵押借款。如果合同中有自由资本，协议将自动将资金部署到 Compound、AAVE 等协议。

![](broken-reference)

## 资本准备金率和复合比率

Compound 协议为所有可用数字资产(Ether、USD Coin、Augur、Dai、Sai、Wrapped BTC、Ox、Basic Attention Token)提供 cToken，使 WeFarm 能够向 Compound 提供/提取资产，以提高 WeFarm 的利用率。

当资本准备金率(R)达到一定水平时，WeFarm 自动向 “Compound/AAVE 网络” 提供贷款货币，当资本准备金率(R)降到 0 \~ 10 时，自动从 “Compound/AAVE 网络” 收回贷款货币。以下是资本利用率(U)、Compound 存款资本比率(C)、资本准备金率(R)的定义。

1. 资本利用率 (U) = 未偿贷款总额/市场存款总额.
2. Compound 存款资本比率 (C) = Compound 存款资本/总市场存款.
3. 资本准备金率 (R) = 1 - U - C.

WeFarm 总是将 R 保持在 10 到 20 之间。当 R > 20 时，它应该发出储蓄池智能合约将存款存入 Compound，这将增加 C 的价值，并将剩余的准备金减少到总存款的 15%。当 R < 10 时，应该发出储蓄池智能合约退出货币市场的信号，这将降低 C 的价值，并将剩余的准备金增加到总存款的 15%。(这个准备金率范围是全局可配置的。)
