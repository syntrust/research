# Robinhood Chain：一条“券商原生”金融 L2 的快速冷启动逻辑

> 更新日期：2026-07-20  
> 数据说明：TVL、稳定币和交易量均为动态指标，本文中的链上数据只代表更新时点的快照，不应视为长期水平。

Robinhood Chain 于 2026 年 7 月 1 日开放公共主网。它最值得关注的地方，并不是发明了新的共识机制或虚拟机，而是把 Robinhood 已有的券商用户、股票资产、法币入口和产品分发能力，与 Arbitrum、Uniswap、Morpho、Paxos、Chainlink 等成熟的链上基础设施组合到了一起。

换句话说，Robinhood 不是先造一条链，再寻找用户和场景；它是先有用户、资产和业务，再为这些业务建设一条专用链。这也是理解其快速冷启动的起点。

## 核心判断

Robinhood Chain 可以被理解为一条“券商原生”的以太坊 L2。它采用成熟的 Arbitrum 技术，把主要工程资源放在金融产品、账户体验、流动性组织和合规边界上，而不是重新开发底层共识。

它目前形成的基本闭环是：

```text
Robinhood App / Wallet
        ↓
用户、法币与既有账户关系
        ↓
USDG（链上现金）＋ Stock Tokens（证券经济敞口）
        ↓
Uniswap / Morpho / Earn / Lighter（钱包集成）
        ↓
交易、抵押、借贷、收益与衍生品
        ↓
用户账户 / Wallet
```

这套闭环解释了 Robinhood Chain 为什么能在主网上线后迅速获得资金和交易量。但需要区分“当前热度”和“长期价值”：现阶段链上活动主要由稳定币、Morpho、Uniswap、Meme 币和新链投机推动；Stock Tokens 是最具差异化的长期资产，却还不是 TVL 和交易量的主要来源。

用一句话概括三条常被放在一起比较的链：

> **Base 靠分发与通用生态取胜，MegaETH 靠实时执行能力取胜，Robinhood Chain 则靠“自带用户、金融资产和交易场景”取胜。**

## Robinhood 的公司背景：为什么是它来做这条链

Robinhood 成立于 2013 年，最早以移动端零佣金美股交易闻名。它把开户、股票、期权和加密资产交易做成了面向普通用户的消费级 App，也由此推动美国零售券商全面进入零佣金时代。两位创始人在创业前曾为银行和对冲基金开发交易基础设施，这家公司从一开始就带有“把专业交易能力软件化，再交给零售用户”的产品基因。

截至 2026 年 5 月，Robinhood 披露约有 **2,770 万入金客户**和约 **3,770 亿美元平台资产**。这两个数字比 TPS 更能解释 Robinhood Chain 的战略价值：即使只有一小部分既有客户进入链上，也足以构成多数新链难以获得的真实分发入口。

Robinhood 的盈利模式主要来自三类收入：一是订单流支付、期权、加密资产和预测市场等交易相关收入；二是客户现金、保证金贷款和证券借贷带来的利息收入；三是 Robinhood Gold、信用卡、财富管理等订阅和金融服务收入。因此，Robinhood Chain 的商业意义不只是赚取 Gas 费，而是把资产发行、交易、结算、借贷和账户关系进一步纳入 Robinhood 的产品体系。

## 技术架构：创新不在重新发明共识

### 成熟的 Arbitrum L2 底座

Robinhood Chain 基于 Arbitrum Dedicated Blockchains / Nitro 构建，结算到以太坊，使用 Ethereum blobs 作为数据可用性层，并以 ETH 支付 Gas。它保持 EVM 兼容，Solidity、Foundry、Hardhat、Viem、Wagmi 等常见工具可以直接使用。

网络给用户的软确认约为 100 毫秒，但这不等于交易已获得以太坊最终性。交易先由 sequencer 接收、排序并返回回执，随后在数分钟内批量发布到以太坊；发布后再经过约 13 分钟，才获得完整的以太坊最终性。通过官方 canonical bridge 提回以太坊时，还要另外面对约 7 天的欺诈证明挑战期。

这套架构的优点是上线快、工具成熟、迁移成本低，并能把链的性能和运行策略按金融业务需求进行定制。它的代价则是用户在软确认阶段仍然依赖 sequencer，不能把“界面上立即成功”与“以太坊最终结算”混为一谈。

### 更可预测的交易排序，同时保留合规控制

Robinhood Chain 采用先到先得的交易排序方式，顺序由交易抵达 sequencer 的时间决定。提高优先费并不能越过已经进入队列的交易，这比单纯依靠 Gas 竞价更适合需要可预测执行顺序的金融市场。

不过，先到先得并不意味着完全消除了 MEV。网络连接质量、交易提交路径和 sequencer 的控制权仍可能带来优势。Robinhood Chain 还在 sequencer 层进行制裁地址筛查：读取链上状态仍然开放，但与受制裁地址相关的交易可能不会被纳入区块。因此，它是“任何人可以部署和读取”的开放网络，却不是一条完全不受运营方影响的抗审查网络。

### 账户抽象与所谓的“AI-native”

Robinhood Chain 为 ERC-4337 提供了完整的基础设施支持，也支持 EIP-7702，可以实现 Gas 代付、批量交易、Session Key、支出限额和自动化策略。这些能力能把授权、兑换、抵押和借贷等多步操作包装成更接近传统金融 App 的体验，也方便 AI Agent 在限定资产、限定额度和限定时间内执行任务。这里的“Gasless”并不是 Gas 消失了，而是由 Paymaster 或产品方代付。

因此，Robinhood 所说的“AI-native”主要发生在账户和产品层，而不是共识层：已上线的基础是智能账户与权限控制；Robinhood 还公布了 Trading MCP，并准备逐步推出 Agentic Accounts。这些都不是某种“AI 共识”。这个区别很重要，因为前者是可以落地的用户体验改进，后者容易变成营销术语。

### 机构联盟式治理

协议由八席 Security Council 管理，其中 Robinhood 占两席，BitGo、Chainlink Labs、Fireblocks、Offchain Labs、Paxos 和 Talos 各占一席。常规操作需要 6/8 签名并经过七天 timelock，紧急操作需要 7/8 签名。BoLD 欺诈证明目前由 Offchain Labs 和 Alchemy 两个许可验证者负责。

这种结构的优势是责任主体、升级流程和机构级密钥管理相对明确；不足则是治理和验证仍然高度许可化。Robinhood Chain 更像由一组金融与基础设施机构共同维护的网络，而不是由 Token DAO 驱动的公链。L2Beat 在本次更新时将其列入 “Others”，直接原因是外部挑战者少于五个；其 Stage 0 核查项同时指出，可用的节点软件仍在审查中。协议缺少用户退出窗口且可被即时升级，则是另一项需要单独考虑的风险。

### Stock Tokens 才是真正的差异化资产

截至 2026 年 7 月 20 日，Robinhood 官方链上注册表列出 **96 个处于 ACTIVE 状态的 Stock Token 和代币化 ETF**，包括 NVIDIA、Apple、Google 和 QQQ 等标的。它们采用 ERC-20 形式，可以进入自托管钱包，并在合规允许的地区参与交易、抵押和借贷。这个数量不能与 Robinhood Europe 上一代 Classic Stock Tokens 的历史产品数量混为一谈。

但 Stock Token **不是底层股票本身**。它是 Robinhood Assets (Jersey) Limited 发行的代币化债务证券，为持有人提供底层证券的经济敞口，却不赋予对底层公司的所有权、投票权或直接法律权利。发行方称流通代币由托管机构持有的底层股票充分支持；现金股息则通过再投资提高 `multiplier`，而不是直接向钱包派发现金。

这套结构提高了链上可组合性，也同时引入发行人、托管、赎回和司法管辖风险。产品并非全球无条件开放，美国、加拿大、英国、瑞士等地区存在明确限制。因此，“24/7 股票上链”更准确的说法是：符合条件的用户可以 24/7 交易一种与股票挂钩的链上证券产品，而不是全球用户都获得了原股票的完整股东权利。

## 核心生态版图

Robinhood Chain 的生态并不是数百个彼此独立的应用自然生长出来的，而是一套围绕交易、借贷和资产发行搭建的“预装式”金融栈，目前仍高度集中于少数核心协议。下表同时包含 Robinhood 明确公布的首日合作方、官方文档列示的第三方项目和后续出现的无许可应用；被官方页面列出并不必然构成法律意义上的背书或合作关系，也不表示所有项目都已原生部署在 Robinhood Chain。

| 层级 | 代表项目或产品 | 在生态中的作用 | 当前观察 |
|---|---|---|---|
| 用户与分发 | Robinhood App、Robinhood Wallet | 承接既有券商用户、钱包用户和法币资金 | 最大优势是现成入口，但券商用户是否会转向自托管仍待验证 |
| 现实资产 | Robinhood Stock Tokens | 将股票和 ETF 的经济敞口做成可组合 ERC-20 | 长期差异最大，当前资产规模仍小于稳定币和 DeFi |
| 链上现金 | USDG（Paxos） | 交易报价、借贷本金、抵押品和收益产品的核心现金资产 | 大量稳定币提前进入，解决了新链最难的流动性冷启动 |
| 现货与流动性 | Uniswap V2/V3/V4、Pleiades、Rialto、1inch | 提供公开 AMM、专业做市和聚合交易 | 交易量几乎全部集中在 Uniswap，流动性效率高但集中度也高 |
| 借贷与收益 | Morpho、Robinhood Earn；Steakhouse、Spark、Ethena、Maple 等策略或资产支持方 | 提供 USDG 存借、收益金库及未来的股票抵押融资 | Morpho 是当前最主要的 TVL 承接者；Earn 公告中的约 7% APY 是浮动估算，不是固定收益，支持方也不等于都在本链独立部署协议 |
| 衍生品与交易集成 | Lighter、Arcus | 通过链上协议或 Robinhood Wallet 集成提供永续合约等交易入口 | Lighter 的公开信息主要是 Wallet 集成，不应据此推断其协议原生部署在本链；该板块目前也不是链上爆发的主引擎 |
| Meme 与发行平台 | NOXA Fun、Flap.sh 等 | 低门槛发币、传播和高频换手 | 是短期获客与交易量的重要来源，也会放大刷量和投机成分 |
| 跨链 | LayerZero、Across、Relay、Arbitrum canonical bridge | 将 ETH、稳定币和其他资产带入网络 | 快桥改善体验，canonical withdrawal 仍有约 7 天挑战期 |
| 数据与基础设施 | Chainlink、Alchemy、BitGo、Fireblocks、Allium | 提供预言机、RPC、AA、托管、分析和机构运维 | 让网络从第一天就具备较完整的金融基础设施 |

### 当前链上快照

按 DefiLlama 在 2026 年 7 月 20 日的同口径数据，Robinhood Chain 的 DeFi TVL 约为 **2.58 亿美元**，稳定币约 **3.93 亿美元**，24 小时 DEX 交易量约 **4.38 亿美元**，七天 DEX 交易量约 **45.6 亿美元**。

其中，Morpho Blue 的 TVL 约为 **1.61 亿美元**，占全链 DeFi TVL 的六成以上。Uniswap V2、V3 和 V4 则贡献了几乎全部现货交易量。链上确有可观察的资金存量和成交记录，但这些活动的质量与可持续性仍需验证，生态也高度依赖少数协议。

更关键的是，Stock Tokens 的活跃规模仍只是数千万美元量级，而且不同数据页面近期存在索引和分类差异。相较之下，稳定币存量和日 DEX 交易量约高 8—10 倍。因而当前最准确的描述不是“股票交易已经主导链上活动”，而是：

> **USDG 提供现金，Morpho 承接资金，Uniswap 和 Meme 币制造换手，Stock Tokens 提供长期差异化叙事。**

## 重要成功因素

### 1. 反向冷启动：先有市场，再有链

普通新链的典型路径是“造链—招开发者—找应用—找资产—找用户”。Robinhood 的路径恰好相反：先在 Arbitrum One 上推出第一代 Classic Stock Tokens，验证需求、托管、合规和钱包体验，再把新一代产品迁移到可定制的专用链。

这种“launch and migrate”模式跳过了最危险的空链阶段。Robinhood Chain 上线时承接的是已经存在的业务，而不是一份等待未来实现的白皮书。

### 2. 难以复制的资产供给能力

任何 EVM 链都能发行 ERC-20，但 Base、MegaETH 或普通创业团队无法凭空获得苹果、英伟达和 QQQ 的合规经济敞口。Robinhood 真正的护城河来自券商基础设施、证券与托管关系、KYC 体系、公司行动处理和产品分发，而不只是 Stock Token 合约代码。

因此，竞争者可以复制它的链，却很难同时复制它的资产、牌照关系、托管网络和用户信任。

### 3. 自带用户、资金和分发入口

近 2,800 万平台级入金客户、Robinhood App、Robinhood Wallet 和既有法币关系，共同构成了强大的分发基础。这个数字不等于 2,800 万人都能购买 Stock Tokens：股票代币受到地区资格限制，美国等市场的 Robinhood 用户目前并非其直接可触达对象，Earn、Wallet 和 Stock Token 面向的用户范围也不完全相同。即便如此，多数公链需要靠空投让用户跨桥，再说服钱包和交易所支持；Robinhood 仍可以把合资格的链上产品嵌入用户熟悉的金融界面中，让区块链尽量退到后台。

这与 Base 借助 Coinbase 分发的逻辑相似，但用户资产结构不同：Coinbase 的强项是加密用户和 USDC，Robinhood 的强项是券商用户、股票关系与现金管理。

### 4. “预装生态”让资金立即形成闭环

Robinhood 没有等待原生 DEX、借贷和预言机慢慢成长，而是在主网上线时就引入或集成 Uniswap、Morpho、Paxos、Chainlink、Alchemy、BitGo、Lighter 等成熟项目。用户进入网络后，立即可以持有 USDG、交易、存款、借贷或通过钱包集成进入衍生品市场。

这套做法牺牲了一部分生态多样性，却显著降低了冷启动风险。对早期金融网络而言，“先形成可用闭环”往往比“先追求应用数量”更重要。

### 5. 成熟底层让团队把资源用在金融产品上

Robinhood 没有为了技术叙事重新开发 L1，而是采用经过长期生产验证的 Arbitrum Nitro。这样既能快速获得 EVM 工具、以太坊结算和标准桥接能力，也能把更多精力投入到资产发行、账户体验、风控、合规和分发。

100 毫秒体验有价值，但不是决定胜负的护城河。真正重要的是技术“足够快、足够稳、足够容易集成”，从而让业务层的优势发挥出来。

### 6. 长期金融叙事与短期投机流量并行

Stock Tokens、RWA、借贷和 Agentic Trading 构成长期故事；Meme 币、DEX 高频换手和收益机会则负责短期获客。前者帮助 Robinhood Chain 建立与其他 L2 不同的身份，后者在几天内制造地址、交易量和社交传播。

这套双引擎在上线初期形成了明显效果，却并非没有风险。Meme 用户能否最终转化为 Stock Token、借贷和长期资产用户，仍然是 Robinhood Chain 尚未证明的关键一步。

## Robinhood Chain vs Base vs MegaETH

三条链都是以太坊 L2，但它们解决的问题不同。Base 想成为开放的链上经济平台，MegaETH 想把 EVM 变成接近实时服务器的执行环境，Robinhood Chain 则想把券商用户、股票和美元资产放进同一个可编程金融市场。

| 维度 | Base | MegaETH | Robinhood Chain |
|---|---|---|---|
| 发起方 | Coinbase | MegaLabs | Robinhood |
| 本质定位 | 通用链上经济与应用平台 | 实时、高性能 EVM | 股票、稳定币和财富管理导向的垂直金融链 |
| 冷启动资源 | Coinbase 用户、USDC、钱包、开发者生态 | 实时执行技术、MEGA Token、性能原生应用 | 券商用户、平台资产、Stock Tokens、USDG、App 与 Wallet |
| 技术路径 | 起步于 OP Stack；2026 年开始迁移到 Base 自营的统一开放技术栈 | 节点角色专业化、约 10ms Mini-block、约 1s EVM block | Arbitrum Dedicated Blockchain / Nitro，约 100ms 区块与软确认 |
| 用户感知速度 | 约 200ms Flashblock，约 2 秒形成完整 L2 区块 | 约 10ms 可读取执行结果 | 约 100ms 获得 sequencer 回执 |
| 数据与结算 | 数据发布到以太坊，结算到以太坊 | 交易数据使用 EigenDA，状态承诺与最终结算锚定以太坊 | 使用 Ethereum blobs，结算到以太坊 |
| EVM 兼容性 | 高度兼容标准 EVM 和 Ethereum JSON-RPC | 合约工具兼容，但有双维 Gas、资源上限和 Realtime API 等定制语义 | EVM 兼容，但需注意 Arbitrum 的区块号、Gas 和地址别名差异 |
| 原生 Gas 资产 | ETH | ETH；$MEGA 已上线，但目前不是主网 Gas Token | ETH |
| 交易排序 | Flashblock 内主要按费用排序，同时受交易到达时间影响 | 由高性能 sequencer 实时执行 | 先到先得，不存在普通优先 Gas 竞价 |
| 生态组织方式 | 大量开发者和应用自由生长 | 重点孵化必须依赖低延迟的原生应用 | 主网上线时直接配置成熟金融协议 |
| 代表性资产与应用 | USDC、Aerodrome、Aave、Morpho、消费与支付应用 | USDm、实时交易、游戏、预测市场等 | USDG、Stock Tokens、Morpho、Uniswap、Robinhood Earn |
| 主要优势 | 分发广、流动性深、开发者与应用多 | 执行性能和实时状态更新最突出 | 差异化金融资产、券商用户和完整产品闭环 |
| 核心风险 | 目标过于通用，差异化可能被稀释；单一 active sequencer 和自营单一客户端也带来运营集中风险 | 性能供给领先，但必须证明足够多应用真的需要 10ms；单 sequencer 和外部 DA 也带来不同信任假设 | 监管与发行人风险、治理中心化、生态与交易量高度集中 |

### 同口径数据快照

| 指标（2026-07-20） | Base | MegaETH | Robinhood Chain |
|---|---:|---:|---:|
| DeFi TVL | 约 $4.54B | 约 $44.4M | 约 $257.7M |
| 稳定币市值（Stablecoins Mcap） | 约 $4.81B | 约 $29.9M | 约 $393.5M |
| 24h DEX 交易量 | 约 $588.9M | 约 $0.59M | 约 $438.4M |

这组数据最值得注意的不是 Robinhood Chain 已经接近 Base 的生态规模——它的 TVL 仍只有 Base 的一小部分——而是它用远小于 Base 的资金存量，制造出了相当高的 DEX 换手。高周转说明冷启动速度很快，也意味着机器人、Meme 和相同资金反复交易的影响很大，不能把 DEX Volume 直接解释成新增用户、净流入或股票投资需求。

Base 的技术栈变化也值得单独说明：它虽然以 OP Stack 起步，但 2026 年已经开始转向 Base 自己维护的统一栈，Azul 和 Beryl 升级也已落地。这提高了迭代自主性，同时把更多实现和升级风险集中到 Base 自身。2026 年 6 月 25 日和 26 日，Base 曾因 sequencer block builder 的软件问题发生两次出块中断，分别持续约 116 分钟和 20 分钟；资金安全没有受影响，但事件说明“生态成熟”并不等于运营风险消失。

MegaETH 则说明另一个问题：链能够处理大量交易，不代表已经形成同等规模的经济活动。它的 10ms Mini-block 和官方宣称的 100,000+ TPS 是执行能力或容量主张，而不是自然需求。10ms 还是交易到达 sequencer 后的预确认与状态可见性口径，并非全球用户从点击到以太坊不可逆结算只需 10ms。MegaETH 后续需要证明实时订单簿、游戏世界、预测市场或高频 Agent 等应用，能够创造普通 L2 无法承载的商业价值。

从竞争逻辑看，三条链代表三种路线：

> **Base 是链上互联网，MegaETH 是链上实时计算机，Robinhood Chain 是链上券商与金融交易所。**

Robinhood Chain 能够快速完成冷启动，并不是因为 100ms 比 Base 的 200ms 快一倍，也不是因为它比 MegaETH 更快。决定性差异在于，它不是拿技术寻找市场，而是让一个已经存在的金融市场选择技术。

## 当前繁荣应如何解读

观察 Robinhood Chain 时，最容易犯的错误是只看交易量。TVL、稳定币、交易量、费用、协议收入和链收入分别回答不同问题，不能相互替代。

- **稳定币规模**说明链上有多少可用现金，但不代表这些资金正在参与 DeFi。
- **TVL**说明资金进入了协议，却可能高度集中在一个借贷市场。
- **DEX Volume**反映换手强度，同一笔资金可以被机器人反复交易很多次。
- **用户支付的费用**不等于协议收入，更不等于 Robinhood Chain 的收入；相当一部分费用流向 LP、做市商、创建者和第三方协议。
- **RWA 市值和持有人数**更接近 Stock Tokens 的实际采用程度，但近期数据仍受索引口径影响。

因此，当前热度可以拆成两层：短期是 USDG、Morpho 收益、Uniswap 流动性和 Meme 交易形成的资金飞轮；长期则取决于 Stock Tokens 能否真正进入交易、抵押、借贷和财富管理场景。


## 结论

Robinhood Chain 的技术水平足以支撑消费级金融体验，但技术不是它脱颖而出的决定性因素。它真正难以复制的资源是：

```text
真实用户
＋ 真实金融资产
＋ 券商、托管与合规关系
＋ App 和 Wallet 分发入口
＋ 上线即用的流动性与协议
```

短期看，它已经成功建立由 USDG、Morpho、Uniswap 和 Meme 交易驱动的资金飞轮；中期看，关键是这些流量能否转化为 Stock Token、借贷和长期资产用户；长期看，最大变量仍是监管是否允许这种代币化债务证券在更多地区和 DeFi 场景中规模化流通。

因此，现阶段最准确的判断是：

> **Robinhood Chain 已经完成了一次强劲的链上冷启动，但还没有证明 Stock Tokens 已经成为这条链最大的真实业务。它的长期价值，将由股票资产与真实用户的迁移决定，而不是由短期交易量决定。**

后续最值得跟踪的三个指标是：

1. Stock Tokens 的活跃市值、持有人数和 DeFi 使用率能否持续增长；
2. DEX 交易是否逐步从 Meme 高频换手转向股票、稳定币和真实金融需求；
3. Robinhood App 中的普通券商用户是否真正进入自托管钱包和链上借贷产品。

## 参考资料

### Robinhood Chain 与 Stock Tokens

- [Robinhood Chain 主网上线公告](https://robinhood.com/us/en/newsroom/robinhood-accelerates-global-expansion-robinhood-chain-mainnet-stock-tokens-agentic-trading/)
- [Robinhood Chain 官方文档](https://docs.robinhood.com/chain/)
- [连接信息：Chain ID、RPC、Ethereum blobs 与 ETH Gas](https://docs.robinhood.com/chain/connecting/)
- [与 Ethereum 的差异：排序、筛查与 EVM 行为](https://docs.robinhood.com/chain/differences-from-ethereum/)
- [交易最终性](https://docs.robinhood.com/chain/transaction-finality/)
- [Account Abstraction](https://docs.robinhood.com/chain/account-abstraction/)
- [治理与验证者](https://docs.robinhood.com/chain/governance/)
- [Stock Tokens 产品与法律结构](https://robinhood.com/rhj/stocktokens/)
- [官方 Token Contracts](https://docs.robinhood.com/chain/contracts/)
- [Robinhood 2026 年 5 月运营数据](https://investors.robinhood.com/news-releases/news-release-details/robinhood-markets-inc-reports-may-2026-operating-data)
- [Arbitrum：Robinhood Chain mainnet](https://blog.arbitrum.io/robinhood-chain-mainnet/)
- [L2Beat：Robinhood Chain 风险与成熟度](https://l2beat.com/scaling/projects/robinhood)

### Base 与 MegaETH

- [Base：向统一自营技术栈迁移](https://blog.base.dev/next-chapter-for-base-chain-1)
- [Base Azul 升级](https://docs.base.org/base-chain/specs/upgrades/azul/overview)
- [Base Beryl 升级](https://blog.base.dev/introducing-base-beryl)
- [Base Protocol Overview](https://docs.base.org/base-chain/specs/protocol/overview)
- [Base Flashblocks 与交易最终性](https://docs.base.org/base-chain/network-information/transaction-finality)
- [Base 2026 年 6 月出块中断复盘](https://blog.base.dev/postmortem-june-25th-block-production-outage)
- [MegaETH 官方网站](https://www.megaeth.com/)
- [MegaETH Mini-blocks](https://docs.megaeth.com/miniblocks)
- [MegaETH Realtime API](https://docs.megaeth.com/developer-docs/overview-2/realtime-api)
- [MegaETH Architecture](https://docs.megaeth.com/architecture)
- [MegaEVM 与标准 EVM 的差异](https://docs.megaeth.com/megaevm)
- [MegaETH 与 EigenDA](https://www.megaeth.com/blog-news/endgame-how-we-break-performance-limits-with-eigenda)
- [MegaETH MiCA 风险披露](https://static.megaeth.com/MEGA%20MiCA%20Whitepaper.pdf)

### 动态数据

- [DefiLlama：Robinhood Chain](https://defillama.com/chain/robinhood-chain)
- [DefiLlama：Base](https://defillama.com/chain/Base)
- [DefiLlama：MegaETH](https://defillama.com/chain/MegaETH)
- [Robinhood Chain Blockscout](https://robinhoodchain.blockscout.com/)
