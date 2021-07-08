https://github.com/Conflux-Chain/CIPs/blob/master/CIPs/cip-43.md
# Simple Summary
此次 CIP 提议将交易最终性 (finality) 引入 Conflux，最终性由质押 CFX 持有人来投票产生。这一 CIP 将避免 PoW 潜在的51%算力攻击，并提高未来发生在 Conflux 上高价值交易的确认安全性。
# Abstract
我们提议引入一条独立的 PoS 链，用于监控 PoW 链(树图结构)的过程，对根据 PoW 确认规则已确认的主链区块达成共识。一旦 PoS 链认为一个主链区块已被确认，所有 PoW 链的矿工应该跟随其后产生新的区块。此举将有效保护 Conflux 链免受来自 PoW 机制的51%远程攻击。
CFX 持有人可以将持有的 CFX 存入特定的合约，以此来成为一个 PoS 节点。PoS 节点通过一个可验证随机函数(VRF)来产生参与 PoS 链共识的委员会。委员身份是临时的，将以六小时为周期进行轮转。委员会的选举类似美国参议院，其成员的任期是交错的，每小时将选举大约六分之一的委员会席位。
# Motivation
在一条公链生态发展早期，算力总量相对不足。这导致可以低成本发起51%攻击并通过双花(double-spending)来窃取链上资产。以太经典，grin 和 verge 在去年相继遭到51%攻击。为了减轻来自攻击者的威胁，Conflux 引入 PoS finality，通过质押 CFX 产生一个委员会来推荐已确认的主链区块，所有 PoW 链节点应跟随此区块来打包产生新的区块。
