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

## 认证流程 Procedure

认证参与角色有三个人：球长（负责人，赋予星球标识），另一个球长（见证人，赋予JTI认证标识），审计人（负责审计登记信息）

一、用户和球长取得联系

分两种情况：

- 用户主动：通过[行星列表](https://github.com/Jouleverse/planetary-system/blob/main/README.md)中球长的微信号，联系到球长
- 球长主动：对于任何人拉进相关微信群中的新用户，凡没有认证的，主动联系

二、球长帮助用户上星（入驻星球）

步骤：
1. 球长确认正确添加了用户微信，把用户拉进自己管理的星球微信群，并指导用户正确添加Jouleverse网络并创建自己的Jouleverse链地址
2. 球长使用[合约工具](https://jscan.jnsdao.com/tools/contractx/#contract-tab-Planet)给用户的链地址赋予(assign)本星标识
3. 球长请用户填写[《JTI可信身份认证信息登记表》](https://docs.qq.com/form/page/DTFBXQmtLRktRT1ZN#/fill)
4. 球长给用户介绍另外一个球长作为见证人，引导用户添加见证人微信，告知链地址和昵称，请见证人对其进行JTI认证

三、见证人验证用户真实性，进行JTI认证

步骤：
1. 见证人一定要确保自己直接添加了用户微信，通过和用户的交流，判断对方是一个真人，且未重复认证，并直接从用户那里获得其待认证的链地址（不可以不直接联系用户而从球长手里拿到地址就认证）
2. 见证人（另一球长）使用[合约工具](https://jscan.jnsdao.com/tools/contractx/#contract-tab-JTI2)给用户地址赋予(assign) JTI认证标识
3. 见证人在【Jouleverse球长交流群】中，报告完成新的JTI认证，说明被认证用户的昵称和JTI编号，并 @ 提醒审计人（目前是Koant或教链）进行处理

四、审计人后向审核用户登记信息无误

步骤：
1. 审计人查看[《JTI可信身份认证信息登记表》](https://docs.qq.com/sheet/DTGtOTVdjbmxoc1lD)审核新认证用户的各项资料，检查其链地址上的星球标识和JTI标识无误，重点是核查是否是同一身份重复认证
2. 经审计无误后，审计人把该名新用户的JTI编号和认证时间（日期）补充到《登记表》中（确保与链上信息一致），备案留查
3. 审计人在【Jouleverse球长交流群】中，报告审计无误，说明被审计用户昵称和JTI编号，并 @ 该用户所在星球的球长，提醒他了解该用户已完成JTI认证

（全流程完）

## 设计思路 Design

把整个JTI v1的三个设计要素进行分离，解耦合为三个部分：

功能 functionality | 实现 implementation | 元数据 metadata | 赋予权 assigner | 条件 condition | 修改/撤销权 modifier
-|-|-|-|-|-
星球标记 | planet NFT | planet, assigner | 星球管理人(原分组组长) | 1. 互加微信，进星球群 2. 填登记表 | 审计人
可信身份 | identity NFT | id, timestamp, assigner | 其他星球管理人 | 1. 互加微信 2. 二次确认登记表已填写无误 | 审计人
<del>可信级别（删除）</del> | <del>identity NFT</del> | <del>星级(scale 1-10)</del> | <del>Oracle</del> | <del>TBD</del> | <del>审计人</del>

## 配置合约 JTI Config

name | JTIConfig
-|-
owner | 平稳运行后移交给JNSDAO多签（？）；可以配置planetName, planetColor, planetAdmin, planetAuditor, identityAuditor <del>、starOracle</del>
planetAdmin | ```mapping(uint=>address)``` 星球可自由申请创立(需符合条件，星系管理委员会负责审核)；<del>可以多个地址负责一个组</del> 一个地址可兼管多个星球（尽量避免）；加入星球时组长只能赋本星球标记；每个人都可以自由更改自己所在星球（移民）；审计人可以更改任何人所在的星球
planetAuditor | ```mapping(address=>bool)``` <del>可以revoke planet NFT</del>；可以任意改组号（不能赋组号）；
identityAuditor | ```mapping(address=>bool)``` 可以revoke identity NFT <del>；可以任意编辑星级</del>
<del>starOracle</del> | <del>```mapping(address=>uint)``` 可以在指定星级以下加星、减星</del>（暂先删除标星设计）

组长兼任的情况，如果允许同一个组长使用两个地址，就会出现自己set planet然后自己assign identity的问题。所以要记录assigner address并做排斥判断。

一个星球只能有一个admin。多人共管的情况，那就上面套一层多签。

## 星球 Planet NFT

name | Planet
-|-
type | ERC-721 non-transferrable
owner | 平稳运行后移交给core多签（？）；可以配置JTIConfig合约地址
planetOfToken | 星球id（0 = 地球）结合Config可以得到 name（名称），color（色彩）
planetAddresses | 反向索引，用于枚举某个星球上所有的居民（成员）
assignerOfToken | 谁赋予的星球标记（用于排斥判断）
func assign(mint) | 仅planetAdmin
<del>func revoke(burn)</del> | 取消此功能。planet NFT不需要revoke
func changePlanet | 仅本人或auditor
func tokenURI | 返回NFT json

## 可信身份 Identity NFT

name | Identity
-|-
type | ERC-721 non-transferrable
owner | 平稳运行后移交给core多签（？）；可以配置JTIConfig合约地址、Planet合约地址；可以fixSince
ICON_COLOR | 视觉色彩，初始默认 string"c647ba", can be changed by owner
transfer | 允许持有者transfer，相当于更改地址
verifier | 验证者地址。谁赋予JTI标记（用于审计）
verifiedByPlanet | 验证者管理的星球id
sinceBlock | 获得认证时的区块高度, can be fixed by owner 
sinceTimestamp | 获得认证时的timestamp, can be fixed by owner
func assign | mint. 仅planetAdmin (& identity auditor) 且 非Planet assigner
func revoke | burn. 仅identityAuditor 
func reassign | mint with older tokenId.
func tokenURI | 返回NFT json



