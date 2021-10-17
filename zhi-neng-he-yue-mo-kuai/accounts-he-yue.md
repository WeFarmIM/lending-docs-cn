---
description: Accounts 合约
---

# Accounts 合约

`Accounts` 合约跟踪账户的本金、利息和其他与余额相关的状态。

## getBorrowBalanceCurrent

获取一个账户的代币的当前借款余额。

`function getBorrowBalanceCurrent (address _token, address _accountAddr) public view returns (uint256 borrowBalance)`

* parameters
  * \_token: 代币地址
  * \_accountAddr: 账户地址

## getDepositBalanceCurrent

获取一个账户的代币的当前存款余额。

`function getDepositBalanceCurrent (address _token, address _accountAddr) public view returns (uint256 depositBalance)`

* parameters
  * \_token: 代币地址
  * \_accountAddr: 账户地址

## getBorrowETH

以 ETH 计算账户所有借款价值

`function getBorrowETH(address _accountAddr) public view returns (uint256 borrowETH)`

* parameters:
  * \_accountAddr: 账户地址

## getDepositETH

以 ETH 计算账户所有存款价值

`function getDepositETH(address _accountAddr) public view returns (uint256 depositETH)`

* parameters:
  * \_accountAddr: 账户地址

## getBorrowPrincipal

获取账户的借款本金

`function getBorrowPrincipal(address _accountAddr, address _token) public view returns(uint256)`

* parameters:
  * \_accountAddr: 账户地址
  * \_token: 代币地址

## getDepositPrincipal

获取账户的存款本金

`function getDepositPrincipal(address _accountAddr, address _token) public view returns(uint256)`

* parameters:
  * \_accountAddr: 账户地址
  * \_token: 代币地址

## getBorrowInterest

获取账户借款利息

`function getBorrowInterest(address _accountAddr, address _token) public view returns(uint256)`

* parameters:
  * \_accountAddr: 账户地址
  * \_token: 代币地址

## getDepositInterest

获取账户存款利息

`function getDepositInterest(address _account, address _token) public view returns(uint256)`

* parameters:
  * \_account: 账户地址
  * \_token: 代币地址

## getBorrowPower

获取账户偿贷能力. 偿贷能力是抵押物代币的价值之和，由其借款 LTC 贴现。

`function getBorrowPower(address _borrower) public view returns (uint256 power)`

* parameters:
  * \_borrower: 从 WeFarm 平台借款的地址

## getLastBorrowBlock

获取账户最近一次借某代币的区块 ID。

`function getLastBorrowBlock(address _accountAddr, address _token) public view returns(uint256)`

* parameters:
  * \_accountAddr: 账户地址
  * \_token: 代币地址

## getLastDepositBlock

获取一个账户最近一次存入代币的区块 ID。

`function getLastDepositBlock(address _accountAddr, address _token) public view returns(uint256)`

* parameters:
  * \_accountAddr: 账户地址
  * \_token: 代币地址

## isUserHasAnyDeposits

如果帐户有任何代币的正存款则返回 true。否则, 返回 false。

`function isUserHasAnyDeposits(address _account) public view returns (bool)`

* parameters:
  * \_account: 账户地址

## isUserHasBorrows

如果一个账户有一个大于 0 的代币借贷余额，返回 true。否则,返回 false。

`function isUserHasBorrows(address _account, uint8 _index) public view returns (bool)`

* parameters:
  * \_account: 账户地址
  * \_index: The index of the token that you saved in the TokenRegistry.

## isUserHasDeposits

如果一个账户有一个大于 0 的代币存款余额，返回 true。否则,返回 false。

`function isUserHasDeposits(address _account, uint8 _index) public view returns (bool)`

* parameters:
  * \_account: 账户地址
  * \_index: The index of the token that you saved in the TokenRegistry.
