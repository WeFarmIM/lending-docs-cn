# 利率模型

## **利率计算公式**

### **参数定义**

* **u (utilization rate)** 某个代币的资产利用率
* **c (Rate Curve Constant)**:
* **s (Compound Supply Rate)**: Compound 市场中实时的供给率
* **b (Compound Borrow Rate)**: Compound 市场中湿湿的借出率
* **sw (Compound Supply Rate Weight)**: Compound 供给率权重
* **bw (Compound Borrow Rate Weight)**: Compound 借出率权重
* **r (Compound Supply Ratio)**: 提供到 Compound 市场中资产比例

### 借款利率模型

$$借款 APR= sw \times s + bw \times b + c \div (1-u)$$

当 $$u$$ > 0.999 时, $$c \div (1-u) = c \div (1-0.999)= c \times 1000$$

对于 Compound 或其他市场均不支持的资产类型, Compound 供给率权重=0, Compound 借出率=0.

综上所述，有两个因素决定了 `借款 APR`，即市场上可用的现行市场利率和 WeFarm 协议中的资本利用率。而且，这是一个非线性模型。如果资金池的利用率接近较高水平，借款利率可以迅速自动调整。

根据不同的参数集，我们有三种不同的策略: 保守模式、适度模式和激进模式。

| 参数                | 保守模式 | 适度模式 | 激进模式 |
| ----------------- | :--: | :--: | :--: |
| Compound 供给率权重    |  0.1 |  0.3 |  0.9 |
| Compound 借出率权重    |  0.9 |  0.7 |  0.1 |
| RateCurveConstant |   4  |   8  |  12  |

以下是基于三种策略的借款利率曲线在不同资本利用水平下的变化情况:

![Interest Model](broken-reference)

### 存款利率模型

$$存款利率 = r \times s + b \times u$$

对于在复合或其他货币市场上无法获得的资产，复合供给利率权重=0，复合借贷利率权重=0

对于 Compound 或其他市场均不支持的资产类型, Compound 供给率权重=0, Compound 借出率=0.

## 利率计算模型

### **参数定义**

* **Deposit Principle**: 用户存款代币
* **Deposit Interest**: 存款利率
* **Deposit Storage Interest:** 已提取利息
* **Deposit Accrual Interest:** 未提取利息
* **Deposit Interest Rate Per Block:** 每个区块利息收益率
* **Deposit Interest Per Block:** 每个区块利息收益
* **BlocksPerYear:** 一年中预计区块数

### **计算公式**

$$Deposit Interest Rate Per Block = 借款 APR \div BlocksPerYear$$

$$Deposit Interest Per Block = (Deposit Principle + Deposit Storage Interest) \times Deposit Interest Rate Per Block$$

$$Deposit Interest(block_t) = Deposit Interest(block_t-_1)+ Deposit Interest Per Block$$

**借款 APR** 在每次该代币用户与合约产生交互时更新.

如果用户与合约产生交互，则该用户将获得在上一个区块和当前区块之间利息。应计利息将加到 **已提取利息** 中.
