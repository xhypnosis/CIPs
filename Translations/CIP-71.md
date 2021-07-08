https://github.com/Conflux-Chain/CIPs/blob/master/CIPs/cip-71.md
# Simple Summary
允许合约开发人员关闭合约的抗重入(anti-reentrancy)机制。
# Abstract
当前，Conflux 智能合约虚拟机拥有抗重入机制。当单个合约在调用栈内出现两次时，Conflux 将禁止在后续执行诸如 `SSTORE` 、`LOG0` 至 `LOG4` 以及余额非零的 `CALL`  等写入操作。此 CIP 旨在令此功能可配置。我们将引入一个新的内部合约，此合约将允许合约自身或其管理者开启或关闭抗重入功能。
# Motivation
Conflux 引入抗重入机制来避免某些来自 DEFI 应用的攻击。然而此机制也会禁用一些合法操作。举例来说，如果某借方合约调用了一个闪电贷合约，然后此闪电贷合约又调用了借方合约的“callback”函数，那么借方合约就会在调用栈内出现两次，接下来的所有写入操作都将被禁止。尽管开发人员可以通过改变合约间的交互逻辑来避免出现这类问题，但是这会给合约迁移带来额外的任务量。
# Specification
## Internal contract
Conflux将引入一个新的内部合约。
```
pragma solidity >=0.4.15;

contract AntiReentrancy {
    /**
     * @dev the contract turns on/off the anti-reentrancy. 
     * @param allow A boolean variable to indicate if the reentrancy is allowed in this contract.
     */
    function allowReentrancy(bool allow) external {}

    /**
     * @dev a contract admin turns on/off the anti-reentrancy for the contract. 
     * @param contractAddr The address of the contract. 
     * @param allow A boolean variable to indicate if the reentrancy is allowed in this contract.
     */
    function allowReentrancyByAdmin(address contractAddr, bool allow) external {}


    /**
     * @dev get whether the reentrancy is allowed for a contract. vote power staking balance at given blockNumber
     * @param contractAddr The address of the contract. 
     */
    function isReentrancyAllowed(address contractAddr) external returns (bool) {}
}
```
## Change for the anti-reentrancy mechanism
只有在一个不允许重入(抗重入功能开启)的合约地址在调用栈内出现两次时，Conflux才会禁止写入操作(触发了抗重入机制)。
## Activation plan
参数: `BLOCK_NUMBER_CIP71A`，` BLOCK_NUMBER_CIP71B`。

a. 启用新的内部合约，对于Conflux上允许重入的合约，关闭其抗重入功能。此内部合约将从`block_numer >= BLOCK_NUMBER_CIP71A`开始激活。

b. 对于当前抗重入机制的实现，将从`block_number >= BLOCK_NUMBER_CIP71B`开始修复其错误行为。
