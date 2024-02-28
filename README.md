# jti2

JTI可信认证v2

## 概念 Concept

JTI = J Trusted Identity 可信身份认证。打造一个在Jouleverse链上提供可信身份认证的体系，为日后涌现出其他更好、更严谨、更可靠的可信身份认证体系做一个示范。

## 动机 Motivation

区块链的特点之一是匿名化，更准确的说，是中本聪隐私模型，即将身份隐私信息置于链下不上链，与链上行为地址相分离，从而杜绝互联网传统隐私模型的大面积隐私泄露风险。具体可参考中本聪撰写的比特币白皮书中“隐私”一节。

但是，这也为区块链应用于非货币场景带来了新的难题。首当其冲的，便是众所周知的“女巫攻击”（sybil attack）问题。该问题说的是，由于区块链地址可以由无限自制的随机私钥无限创建，一个链下的真实主体便可以创建无限多的链上地址，实现“分身”。

比如，在一个限定每人一票的链上治理机制中，一个人可以轻而易举地创造压倒性比例的“分身”来实现压倒性的票数，从而控制和操纵投票结果，进而篡夺整个治理系统。

同时，可信身份也有助于消除区块链被应用于非法活动的风险。结合区块链本身的可溯源性，可以有效追溯非法活动的源头和走向，并在执法机构需要时配合打击犯罪。

JTI，就是对于在去中心化的链上实现一套可以抗女巫攻击的可信身份系统的尝试。

在这个解决方案中，所采用的方法是社交认证的方法。即，通过链下的人际社会关系来尽最大努力识别和判断区块链地址的所有者是一个真实、可靠的人，然后通过赋予NFT (Non-Fungible Token)的方式来对地址进行标记，同时保持隐私信息不上链，以此实现中本聪隐私模型和抗女巫攻击的结合。

## 误解 Myth

有人认为，可信身份认证是违背区块链匿名性、隐私性以及抗审查性的“原教旨”特性的。这是对区块链的深刻误解。

区块链从来都不是完全匿名的。它只是采用了中本聪隐私模型，把隐私数据与链上地址相分离，以达到更好保护隐私的目的。至于担心链下部分的隐私信息泄露问题，那就是如何加强链下数据安全保管的技术问题。

另外，区块链的所谓抗审查性，指的是下层节点不对上层应用的交易数据进行价值判断和过滤，而绝非指对抗现实世界的法律秩序和道德规范。越是应用链，越是远离货币（如BTC）这一包容性最强的应用场景，就越是要注意符合现实世界的秩序和规范，并尽可能在上层应用层主动担当起合规义务。

## 设计思路 Design

把整个JTI v1的三个设计要素进行分离，解耦合为三个部分：

功能 functionality | 实现 implementation | 元数据 metadata | 赋予权 assigner | 条件 condition | 撤销权 revoker
-|-|-|-|-|-
分组标记 | group NFT | group, assigner | 责任组长 | 1. 互加微信，进分组 2. 填登记表 | 审计人
可信身份 | identity NFT | id, timestamp, assigner | 第二组长 | 1. 互加微信 2. 二次确认登记表已填写无误 | 审计人
可信级别 | identity NFT | 星级(scale 1-10) | Oracle | TBD | 审计人

## 配置合约 JTI Config

name | JTIConfig
-|-
owner | 平稳运行后移交给JNSDAO多签；可以增删groupAdmin、groupAuditor、identityAuditor、starOracle
groupAdmin | ```mapping(address=>uint)``` 组号 1-10 未来可以扩展；可以多个地址负责一个组；赋组时组长只能赋本组号；可以改本组成员的组号，改成其他组号（转组）
groupAuditor | ```mapping(address=>bool)``` <del>可以revoke group NFT</del>；可以任意改组号（不能赋组号）；
identityAuditor | ```mapping(address=>bool)``` 可以revoke identity NFT；可以任意编辑星级
starOracle | ```mapping(address=>uint)``` 可以在指定星级以下加星、减星

## 分组标记 Group NFT

name | JTIGroup
-|-
type | ERC-721 non-transferrable
owner | 平稳运行后移交给JNSDAO多签；可以配置JTIConfig合约地址
group | 组号；0 = 无分组
groupAddressList | 反向索引，用于枚举某个组下所有的JTI
assigner | 赋组组长的地址（记录用）
func assign(mint) | 仅groupAdmin
func revoke(burn) | 取消此功能。group NFT不需要revoke
func changeGroup | 仅本组admin或auditor
func tokenURI | 返回NFT json

## 可信身份 Identity NFT



