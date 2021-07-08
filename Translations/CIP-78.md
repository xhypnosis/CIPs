https://github.com/Conflux-Chain/CIPs/blob/master/CIPs/cip-78.md
# Simple Summary
修复交易收据中的错误字段。
# Abstract
目前，当交易执行失败的时候，交易收据将显示此次交易未被赞助，即使赞助商实际上已经代付了gas fee。此外，若赞助商没有足够的余额来支付存储抵押时，将由交易发送者来承担该笔交易的存储抵押。然而，交易收据上的记录将显示此次交易是被赞助的。
1. 交易记录表明因未被赞助交易费而交易执行失败。实际上，赞助商支付了gas费用。
2. 当赞助人没有足够的余额支付存储抵押品时，发送方将支付存储抵押品。 但是，交易记录显示该交易是被赞助的。
# Motivation
在任何情况下，交易收据中的“is_sponsed”字段都应始终与赞助商是否代付的实际情况保持一致。
无论发生什么，交易收据中的“is_sponsed”字段都应始终与赞助商是否付款保持一致。
# Specification
此次CIP之前。
| |**Not in whitelist**|**In whitelist but sponsor cannot afford**|**Sponsored**|
|:---|:---|:---|:---|
|**Success**|Gas: false / Storage: false|Gas: false / **Storage: true**|Gas: true / Storage: true|
|**Reverted / Fail**|Gas: false / Storage: false|Gas: false / Storage: false|**Gas: false / Storage: false**|

此次CIP之后。
| |**Not in whitelist**|**In whitelist but sponsor cannot afford**|**Sponsored**|
|:---|:---|:---|:---|
|**Success**|Gas: false / Storage: false|Gas: false / **Storage: false**|Gas: true / Storage: true|
|**Reverted / Fail**|Gas: false / Storage: false|Gas: false / Storage: false|**Gas: true / Storage: true**|
