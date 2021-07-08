https://github.com/Conflux-Chain/CIPs/blob/master/CIPs/cip-64.md
#Simple Summary
当前，Conflux 上的智能合约无法在执行时获得当前的 epoch 数。为了维护 EVM 的兼容性，此 CIP 引入了一个新的内部合约，使 epoch 信息能被 Conflux 上的合约访问。
#Abstract
在 dapp 开发中，一个常用的技术是将事件所对应的区块数存储在合约中，然后使用此区块数信息来查询合约。举例而言，某人也许希望从合约的创建开始来查询日志。然而 Conflux 的查询 APIs 只支持查询 epoch 数，不支持查询区块数，这使上述通过区块数查询的技术无法被应用在 Conflux 上。此次 CIP 引入了一个新的内部合约，使其他合约可以访问执行(指定交易的) epoch 数。
#Motivation
在智能合约中，有两种使用区块数(`block.number`)的常见用例。
第一种用例是计时。因为区块数的增长速率相对稳定且很难被影响，所以适合时间锁等应用。
第二种用例是查询。例如，一个合约可以保存其自身被创建时的区块数，用户可以通过读取此信息来确认需要从哪个区块开始查询合约事件。
第二种用例在Conflux不可用，因为我们的查询 APIs(RPC) 仅支持通过  epoch数来查询，而不支持通过区块数查询。
#Specification
从epoch XXX开始，一个新的内部合约将被部署在地址`0x0888000000000000000000000000000000000003`(`cfx:type.builtin:aaejuaaaaaaaaaaaaaaaaaaaaaaaaaaaapx8thaezf`)。此合约具有如下接口。
```solidity
pragma solidity >=0.4.15;

contract Context {
    /*** Query Functions ***/
    /**
     * @dev get the current epoch number
     */
    function epochNumber() public view returns (uint64) {}
}
```
当交易执行触发对 `epochNumber()` 的调用时，内部合约对于此函数的实现，必须根据交易执行的上下文，返回其当前所处的 epoch 数。
与`NUMBER`操作码类似，调用此内部合约的gas成本应为`2`。 CIP-64
