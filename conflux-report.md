# Conflux (CFX) 估值分析研究报告

**更新日期：2026年4月17日**  
**研究目标**：验证 Conflux 当前估值是否显著领先于链上真实使用与生态活跃度，是否存在“资产有价格、生态无密度”的结构性脱节。  
**核心结论**：**当前（2026年4月）Conflux 被明显高估**。市值主要由“中国唯一合规公链 + AxCNH + 一带一路”叙事驱动，而非链上生态密度。若 AxCNH 未能快速转化为真实 DeFi/机构结算流量，估值存在下行风险。

### 研究方法

本报告的横向对比对象限定为 **Conflux、Kaia、Gnosis** 三条同市值区间的 EVM L1，核心目的不是比较叙事强弱，而是比较“相近估值下，谁拥有更高的真实使用密度”。

- **可比对象选择**：先用 CoinGecko 筛选与 Conflux 市值接近、且同属 EVM 生态的 L1，最终选取 Kaia 与 Gnosis 作为对照链，尽量避免因架构差异带来的口径失真。  
- **统一时间窗口**：链上活跃度指标统一采用 `2026-04-15 UTC` 完整日，避免把滚动 24 小时、月度快照和历史累计值混在一起；市场价格、市值和交易量则单独标注其对应快照时间。  
- **统一指标框架**：三条链都尽量落到同一组核心指标上比较，包括 `日交易数`、`活跃地址数`、`DeFi TVL`、`DEX Volume (24h)`；其中链上行为优先使用官方浏览器或官方 API，TVL 与 DEX 成交统一使用 DefiLlama 作为补充口径，再结合 AxCNH 的链上分布和使用情况，判断 Conflux 的估值是否由真实生态密度支撑。  
- **AxCNH 专项拆解**：除链级数据外，报告单独追踪 AxCNH 的 `总供应`、`持币地址数`、`持仓集中度`、`完整日转账笔数` 与 `参与地址数`，用来判断这一核心叙事资产究竟是已形成真实使用，还是仍停留在少数账户主导的结算型余额。  
- **事件与价格联动追踪**：对于 AxCNH、BRI、RWA、减半与 RWA 资产上线等关键事件，报告采用“`事件核心 - 价格反应 - 关联强度`”框架逐条梳理，区分哪些催化对应的是短期情绪交易，哪些更可能反映中长期基本面变化。  

**附：对比对象简介**

- **Conflux**：目前中国唯一合规的开放式公链，采用独创的树图（Tree-Graph）结构实现高度并行处理，支持 3,000+ TPS。PoW+PoS 混合共识机制使 Conflux 能够在不牺牲去中心化的前提下实现高性能。Conflux 网络由 Core 和 eSpace 两个独立的空间组成，其中 eSpace 完全兼容 EVM。Conflux 重点打造政企级合作与合规跨境贸易生态，深度绑定“一带一路”（BRI）与离岸人民币生态，2025 年推出全球首个受监管的离岸人民币稳定币 **AxCNH**。  
- **Kaia**：是 2024 年由亚洲两大社交巨头 Kakao（韩国）和 LINE（日本/东南亚）旗下的区块链项目合并而成的 Layer 1 公链，支持 4,000+ TPS，区块时间 1 秒，且具备即时确认性，Gas 费用约为以太坊的 1/10，支持社交小程序访问及 Gas 代付，重点打造稳定币结算层，支持原生 USDT，集成 RWA，旨在打造亚洲最大的 Web3 生态系统，重点覆盖韩国、日本、台湾、泰国及印尼市场。  
- **Gnosis**：是由以太坊早期核心团队建立的 Layer 1 社区公链，核心架构与以太坊高度对等（EVM-equivalent），支持 2,000+ TPS，区块时间约 5 秒。它以去中心化程度极高著称（拥有超过 20 万个验证者），原生支持 xDAI（稳定币）作为支付代币，费用极低且稳定。Gnosis 重点打造链上支付与去中心化治理基础设施，旗下拥有行业标准级的 Safe 多签钱包和 Gnosis Pay 自托管借记卡。  

### 市场估值概览

**同市值 EVM L1 对比（2026-04-15 UTC）**

| 链 | 代币 | 价格 | 7天涨幅 | 市值 | 24h 交易量 | CoinGecko 排名 |
|---|---|---:|---:|---:|---:|---:|
| **Conflux** | `CFX` | **$0.05981** | **+19.25%** | **$310.42M** | **$45.71M** | **#139** |
| **Kaia** | `KAIA` | **$0.04749** | **+0.94%** | **$278.17M** | **$5.49M** | **#144** |
| **Gnosis** | `GNO` | **$118.75** | **-3.52%** | **$313.47M** | **$2.95M** | **#138** |

**解读**：
- 在市值接近的 EVM L1 中，Conflux 过去 7 天涨幅明显领先于 Kaia 和 Gnosis，且 24h 成交量远高于两者，说明其在当前时间窗口内的二级市场交易热度和最近叙事（减半、XAUt0等）关注度更强。
- 但这一表现具有明显的阶段性和偶然性，只能反映近期价格行为，不代表未来会长期持续强于同类链。

### 链上采用与使用密度

**同市值 EVM L1 活跃度对比（2026-04-15 UTC）**

| 链 | 日交易数 | 倍数 | 活跃地址数 | 倍数 | DeFi TVL | 倍数 | 市值/TVL | DEX Volume (24h) | 倍数 | 数据来源 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| **Conflux** | **26,479**（[Core](https://confluxscan.org/pow-charts/tx) 10,332 + [eSpace](https://evm.confluxscan.org/charts/tx) 16,147）| **1.0x** | **4,193**（[Core](https://confluxscan.org/pow-charts/active-accounts) 1,362 + [eSpace](https://evm.confluxscan.org/charts/active-accounts) 2,831）| **1.0x** | **$7.66M** | **1.0x** | **40.5x** | **$50,439** | **1.0x** | ConfluxScan charts + DefiLlama |
| **Kaia** | **159,628** | **6.0x** | **29,179** | **7.0x** | **$12.40M** | **1.6x** | **22.4x** | **$141,252** | **2.8x** | Kaiascan charts + DefiLlama |
| **Gnosis** | **375,816** | **14.2x** | **4,294** | **1.0x** | **$91.96M** | **12.0x** | **3.4x** | **$5.71M** | **113.2x** | GnosisScan chart + DefiLlama |

**解读**：
- **总体判断**：按这组更新后的链上数据看，Conflux 在同市值 EVM L1 中处于相对弱势。  
- Kaia 的日交易数约为 Conflux 的 **6.0x**，活跃地址数约为 **7.0x**，基础 TVL 和 DEX 成交也分别约为 **1.6x** 和 **2.8x**；同时其 **市值/TVL 仅为 22.4x**，明显低于 Conflux 的 **40.5x**，说明其用户活跃面和资金沉淀都更厚。  
- Gnosis 的活跃地址数与 Conflux 基本接近，但日交易数约为 **14.2x**、基础 TVL 约为 **12.0x**、DEX 成交约为 **113.2x**；其 **市值/TVL 仅为 3.4x**，更像是资本、协议和高频交易深度明显更强的成熟链。  
- **归纳**：Conflux 当前不仅弱于 Gnosis 这种“资金和流动性密度高”的链，也弱于 Kaia 这种“用户活跃面更宽”的可比链；而其 **40.5x** 的市值/TVL 也显著高于 Kaia 和 Gnosis，因此“估值领先于使用密度”的判断被这组横向对比进一步强化。

### AxCNH 稳定币专项分析

**通过 ConfluxScan API 统计的 AxCNH 链上数据（2026-04-15 UTC）**

| 维度 | 结论 |
|---|---|
| 总量 | **3,812.84 万枚** |
| 市值 | **约 $5.59M** |
| 发行空间 | **100% 在 eSpace** 流通 |
| 合约地址 | `0x70bfd7f7eadf9b9827541272589a6b2bb760ae2e` |
| 持币地址数 | 截至 `2026-04-16`，持币地址共 **1,036 个**。 |
| 持仓集中度 | 前 **2** 个地址合计约占 **98.0%**，前 **5** 个地址约占 **99.1%**，整体流通高度集中。 |
| 完整日转账活跃度 | 记录 **253** 笔转账、**26** 个 unique participants。 |
| 流动性与 DeFi 集成 | 当前主要流动性仍集中在 Swappi DEX（Conflux 原生 DEX），但整体 DeFi 集成度较低 |

**解读**：AxCNH 确为 Conflux eSpace 上的真实资产，但当前更像**少数机构 / 发行账户主导的结算型余额**，而非已广泛渗透到 DeFi 与日常链上交互的稳定币。这也意味着，AxCNH 虽能支撑政策与合规叙事，却尚未有效转化为更高的生态密度。

### AxCNH、BRI 与 RWA 叙事的事件索引

| 时间 | 事件 | 链接 |
|---|---|---|
| 2025年6月 | 哈萨克斯坦 AFSA 向 AnchorX.KZ 发出首张法币稳定币发行牌照，为 AxCNH 后续在哈萨克斯坦的合规试点提供监管基础。 | [AIFC/AFSA：AFSA Grants First Stablecoin Issuer License](https://aifc.kz/news/afsa-grants-first-stablecoin-issuer-license/) |
| 2025年7月 | AnchorX 与相关合作方围绕 AxCNH、跨境支付、RWA 和“一带一路”场景展开战略合作；Conflux 3.0 升级方案同期进入公开披露阶段。 | [港交所公告：Goldstream 与 AnchorX 战略合作](https://www1.hkexnews.hk/listedco/listconews/sehk/2025/0707/2025070700485.pdf)；[Conflux Docs：v3.0 升级文档](https://doc.confluxnetwork.org/docs/general/hardforks/v3.0/) |
| 2025年7月 | Conflux、AnchorX 与 TokenPocket 公布合作，提出在中亚、东南亚等重点地区推进稳定币支付试点；同期 AxCNH 与 Conflux 3.0 一起进入公开叙事阶段。 | [Cointelegraph：Conflux launching offshore yuan-backed stablecoin](https://cointelegraph.com/news/conflux-launches-yuan-stablecoin-and-blockchain-upgrade) |
| 2025年9月 | AxCNH 在香港第十届 Belt and Road Summit 正式发布，并披露其已在哈萨克斯坦取得 AFSA 牌照、与 ATAIX Eurasia 对接交易、与中联重科测试 Conflux 链上跨境支付。 | [EZ Newswire：AxCNH debuts at the 10th Belt and Road Summit](https://www.eznewswire.com/newsroom/anchorx-axcnh-first-licensed-offshore-yuan-pegged-stablecoin-belt-and-road-summit)；[Kursiv/Reuters 转述：China launches stablecoin in Kazakhstan](https://kz.kursiv.media/en/2025-09-30/engk-yeri-china-launches-stablecoin-in-kazakhstan-in-challenge-to-dollar/amp/) |
| 2025年10月-11月 | AxCNH 在 Conflux 生态内的落地开始从“叙事”转向“工具与入口”，官方整理了 AxCNH 的使用入口、DEX 与借贷场景。 | [Conflux Forum：AxCNH landing page is now live](https://forum.conflux.fun/t/axcnh-landing-page-is-now-live/22974)；[Conflux Forum：AxCNH 存款激励活动持续进行中](https://forum.conflux.fun/t/axcnh/23036) |
| 2026年1月 | Conflux 官方年终回顾明确写到：AxCNH 在 2025 年已完成新加坡、马来西亚早期试点，并以此为基础向哈萨克斯坦、印尼及更广泛的中亚 / 东南亚区域扩展。 | [Conflux Medium：EOY Recap and Highlights 2025](https://medium.com/conflux-network/conflux-community-eoy-recap-and-highlights-2025-015a260124a8) |
| 2026年4月 | XAUt0（Tether Gold 的 omnichain 版本）正式上线 Conflux，RWA 资产侧的基础设施叙事进一步扩展。 | [USDT0 官方博客：XAUt0 Is Live on Conflux](https://blog.usdt0.to/xaut0-is-live-on-conflux) |

**解读**：
- 从叙事源头看，AxCNH、BRI 与 RWA 并不是完全停留在概念层面的空叙事，确实存在合作、升级、产品入口与资产上线等连续事件；
- 但当前可验证的事件来源仍主要来自官方公告、论坛帖子与互联网媒体传播，尚不足以证明其已经形成大规模、可感知的真实世界使用体验。

### 估值判断：是否被高估？
- **支持高估的核心证据**（数据驱动）：  
  - 估值/使用脱节严重： 高市值/TVL、每日数万美元 DEX 量和极低费用，属于典型“叙事溢价链”。  
  - AxCNH 虽已在链上落地，但整体仍呈现高度集中、低活跃、低渗透特征，尚未有效转化为 DeFi 密度或 TVL 增长。  

- **反驳点（叙事潜力）**：  
  AxCNH+BRI+RWA 是真实政策红利及潜在市场机会，落地持续推进，若 2026-2027 年规模化落地（流通量从百万美元级升至数十亿），TVL 有望显著重估。  

### 结论
按 **2026-04-15 UTC** 的同口径对比，Conflux 在与 **Kaia**、**Gnosis** 这类同市值 EVM L1 的横向比较中，二级市场热度更强，但链上使用密度明显偏弱：全网日交易数、活跃地址、DeFi TVL 与 DEX 成交均不占优。与此同时，**AxCNH + BRI + RWA** 叙事并非空洞概念，确有监管、合作与区域扩展落地，能够解释 Conflux 为何获得持续估值溢价；但 AxCNH 当前仍呈现高集中、低活跃、低 DeFi 渗透特征，尚未把叙事充分转化为生态深度。更符合证据的判断是：**Conflux 并非“没有基本面”，而是“叙事真实、采用滞后”，当前估值仍阶段性领先于真实使用密度。**
