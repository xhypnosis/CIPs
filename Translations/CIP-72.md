https://github.com/Conflux-Chain/CIPs/blob/master/CIPs/cip-72.md
# Simple Summary
让Conflux接受来自以太坊钱包 (如Metamask) 签名产生的交易。
# Abstract
Conflux 的交易格式与以太坊不同，所以无法接受由 Metamask 钱包产生的交易签名。此 CIP 计划让 Conflux 将目标 epoch (`target epoch`) 为`u64::MAX` 的交易视同以太坊类型的交易，并根据以太坊的规则验证交易签名。
# Motivation
此CIP将允许用户使用诸如Metamask的以太坊钱包与Conflux链上的智能合约交互。我们相信此举能吸引更多用户来体验Conflux。
# Specification
若交易的目标 epoch 为`u64::MAX`，Conflux将视其为以太坊类型的交易。应遵循以下步骤来验证以太坊类型交易。

- 交易的存储限制为`u64::MAX`
- 交易签名是为`rlp(nonce, price, gas_limit, recipient, chain_id, ε, ε)`的哈希值而产生的。此处`ε`表示Conflux规范中定义的 "empty string"。

对于以太坊类型的交易，Conflux不要求发送者有足够余额来支付存储抵押。
RPC在收到一个以太坊类型的交易时，如果发送者的地址不是以`0x1`开头，则拒绝此类交易。

## Activation plan
参数：`EPOCH_HEIGHT_CIP72A`，`BLOCK_NUMBER_CIP72B`。

a. 从`epoch_height >= EPOCH_HEIGHT_CIP72A`开始，以太坊类型交易可以被打包进区块。在此之前，由于以太坊类型交易的目标epoch数值太大，将其打包进区块的行为将被视为非法。b. 从`block_number >= BLOCK_NUMBER_CIP72B`开始，虚拟机可以执行以太坊类型交易。在此之前，虚拟机将忽略此类交易。
