# IoTeX 交易结构复核报告

Generated at: 2026-04-15

## 目的

针对 IoTeX 在 24 小时内的链上交易数据，复核其主要来源是来自真实用户行为，还是主要来自协议系统动作与自动化任务网络。

## 结论

- IoTeX 在 `2026-04-12` 当天的交易活跃度，主要不是自然用户行为，而是协议系统动作与自动化任务网络。
  - 其中 `87.23%` 是零费 `GrantReward` 型系统交易
  - 剔除系统交易后，剩余付费交易的 `78.18%` 又高度集中在两个疑似自动化的合约方法。
  - 与同一时间窗对齐的官方链上日志里，只抓到 `4` 条 DEX `Swap` 事件、`4` 笔唯一 `swap tx`，不足以支撑“真实交易需求驱动的活跃”。
- 因此，IoTeX 这组 `tx/day` 和 `tx/DAU` 数据不适合直接与 Base、Monad 等做同口径生态活跃度比较。

## 方法

1. 以 Dune 的 `2026-04-12` 作为日度口径参考，用 IoTeX 官方 RPC 与区块浏览器/API 复核当日完整交易，并与 Dune 的数据进行对比。
2. 按交易类型拆分系统动作与普通交易，识别出零费用 `GrantReward` 型系统交易；同时对照区块数，验证其与出块节奏高度绑定。
3. 对剩余付费交易按目标地址、调用方法、发送节奏和合约业务做聚类归因。
4. 在同一时间窗与同一块区间内，直接扫描官方 RPC 的 `UniswapV2` 与 `UniswapV3` `Swap` 事件日志。

## 样本范围

| 指标 | 数值 |
| --- | --- |
| Dune 日期 | 2026-04-12 |
| UTC 窗口 | 2026-04-12T00:00:00.000Z 至 2026-04-13T00:00:00.000Z |
| 区块范围 | 47,015,995 至 47,050,552 |
| 区块数 | 34,558 |
| 交易数 | 39,646 |
| 唯一发送地址 | 131 |
| Dune tx_24h | 39,670 |
| Dune dau | 131 |
| Dune 对齐率 | 99.94% |

## 第一层：系统交易

### 系统交易画像

| 指标 | 数值 |
| --- | --- |
| 系统交易数 | 34,582 |
| 总交易占比 | 87.23% |
| 发送方数量 | 36 |
| 共同目标地址 | 0xa576c141e5659137ddda4223d209d4744b2106be |
| gas | 0 |
| gasPrice | 0 |
| gasUsed | 0 |
| Type | GrantReward |

###  结构判断

- 从区块分布看，这类 `GrantReward / zero-gas` 系统交易在样本窗口内的每个区块都能见到，指向同一个系统合约地址 `0xa576c141e5659137ddda4223d209d4744b2106be`；这说明它更像伴随出块过程持续发生的协议级记账行为，而不是零散出现的普通用户交易。
- 官方文档 [IoTeX gRPC API](https://docs.iotex.io/builders/reference-docs/native-iotex-development/iotex-grpc-api) 的示例显示，`grantReward` 这类 `gasPrice=0` 的系统 action 由区块生产者地址发出。
- 官方文档 [Voters and Delegates](https://docs.iotex.io/blockchain/learn-iotex/core-concepts/voters-and-delegates) 写明，前 `36` 名 `Consensus Delegates` 负责交易验证和出块；
- 这些发送方使用同一组 `GrantReward` 类型交易发送到统一合约地址，不携带普通转账金额字段，更像在链上更新奖励状态或将奖励记入奖励池/奖励账户。

## 第二层：非系统交易

### 非系统交易概览

| 指标 | 数值 |
| --- | --- |
| 非系统交易数 | 5,064 |
| 总交易占比 | 12.77% |
| 合约调用数 | 4,661 |
| 普通转账数 | 403 |
| 非系统平均手续费 | 0.18786984 IOTX |
| 非系统中自动化疑似占比 | 64.67% |

### 结构判断

- 非系统交易没有像系统交易那样“每块一笔”的强协议节奏，但依然非常集中。
- 前两大方法 `depositSettleAndClaim(address[],uint256[],address[])` 和 `depositToStake(uint64,uint256,uint8[])` 合计 `3,959` 笔，占非系统交易的 `78.18%`。
- 这说明把系统交易剔掉之后，IoTeX 的剩余活跃也不是“分散的自然用户使用”，而是仍以少数自动化簇为主。

### 同窗口 DEX Swap 复核

- 在与本报告完全一致的时间窗 `2026-04-12 00:00:00 UTC` 至 `2026-04-13 00:00:00 UTC`、以及同一块区间 `47,015,995` 至 `47,050,552` 内，用 IoTeX 官方 RPC 直接扫描 `Swap` 事件日志后，只抓到 `4` 条 DEX `Swap` 事件、对应 `4` 笔唯一交易。
- 其中 `UniswapV2` 风格 `Swap` 事件 `3` 条，`UniswapV3` 风格 `Swap` 事件 `1` 条；合计只占总交易数的约 `0.01%`，占非系统交易的约 `0.08%`。
- 这组结果和前文“DEX 成交几乎为零”的判断一致，也进一步说明 IoTeX 当日的高链上活跃并不是由真实交易需求驱动。

| Swap 类型 | 事件数 | 唯一交易数 | 主要池子 |
| --- | --- | --- | --- |
| UniswapV2 风格 `Swap` | 3 | 3 | `DEPINS/WIOTX`、`GFT/WIOTX` |
| UniswapV3 风格 `Swap` | 1 | 1 | `XPIN/WIOTX` |
| 合计 | 4 | 4 | 3 个池子 |

| 时间（UTC） | 交易哈希 | 池子 |
| --- | --- | --- |
| 2026-04-12 08:04:15 | `0xc3404a0939af8e7df0acf6b55707b8b375c3be849de0609c9cc4028676fe29f4` | `XPIN/WIOTX` v3 pool (`0.3%`) |
| 2026-04-12 14:29:55 | `0x217cb4cf8deb1959d700fe3dae7962d94b8d67cb514618d80af39ea1b504dcbf` | `GFT/WIOTX` v2 pair |
| 2026-04-12 22:25:57 | `0x6800485dba5c7821eda54b59f7b224f4fa647d82ae76467dd6cf9c4fc6ba3620` | `DEPINS/WIOTX` v2 pair |
| 2026-04-12 22:26:17 | `0x424ac734e4d7be080dc7526b0be0e7b197b5b4358dfb1952ecf7b10a6d5bf46b` | `DEPINS/WIOTX` v2 pair |

## 高占比方法解码

| 方法/调用类型 | 交易数 | 占全部交易 | 占非系统交易 | 发送方数 | 主要目标地址 | 目标类别 | 综合判断 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `GrantReward` 协议奖励记账调用 | 34,582 | 87.23% | 不适用 | 36 | `0xa576c141e5659137ddda4223d209d4744b2106be` | 协议奖励虚拟目标（推断） | 浏览器类型显示为 `GrantReward`；EVM 侧 selector 为 `0xd72778dc`，未命中公开 ABI。几乎全部打向同一奖励虚拟目标，且 `36` 个发送方与 IoTeX `36` 个 `Consensus Delegates` 的共识结构高度一致，更像协议内部奖励结算方法。 |
| `depositSettleAndClaim(address[],uint256[],address[])` | 2,875 | 7.25% | 56.77% | 1 | `0x236cbf52125e68db8fa88b893ccafb2ee542f2d9` | ioSwarm AgentRewardPool v2 | 官方 IIP-58 将其列为 `AgentRewardPool v2` 原子结算方法；样本 calldata 与该签名精确匹配。单地址、单目标、单方法，中位间隔 `30s`，极像协调器定期做链上奖励结算并代部分代理领取奖励。 |
| `depositToStake(uint64,uint256,uint8[])` | 1,084 | 2.73% | 21.41% | 11 | `0x04c22afae6a03438b8fed74cb1cf441168df3f12` | Native Staking Virtual Contract | 官方 `Native Staking Virtual Contract` 的加仓方法；`cast sig` 可直接算出其 selector 正是 `0x34e8e145`。多地址协同打同一目标，更像批量质押/加仓类操作，而非普通 dApp 业务逻辑。 |
| `native-transfer` | 403 | 1.02% | 7.96% | 51 | 分散 | 普通转账流入地址为主 | 普通转账，是相对更接近自然用户行为的一层，但规模仍然很小。 |
| `route(address,uint256,bytes32[],bytes)` | 179 | 0.45% | 3.53% | 1 | `0xebf9ab649f9952f9b6e85e59fac9fed43594e3e0` | 路由/执行合约 | 对应路由执行方法。 |
| 未公开私有方法 | 145 | 0.37% | 2.86% | 1 | `0xd323f014c598f033c40ae2e02ed971c472a5bbe4` | 私有任务合约（推断） | EVM selector 为 `0x581b7575`，未命中公开签名库；高频单目标私有方法。 |
| `setDirectPrice(address[],uint256[])` | 124 | 0.31% | 2.45% | 2 | `0xe15b879a66be885c0d6e877228720d29c28a3aa6` | 价格更新合约 | 对应价格更新方法。 |
| `claim(uint256,uint8[])` | 84 | 0.21% | 1.66% | 3 | 分散 | 领取类调用 | 对应领取方法。 |
| `distributeRewards(bytes32,uint256,address[],uint256[])` | 74 | 0.19% | 1.46% | 1 | `0x4839f8718709d4f76c6f23625908cfdc4330ef82` | 奖励分发合约 | 对应奖励分发方法。 |
| `pushToken()` | 24 | 0.06% | 0.47% | 1 | `0xd8d1fa9edb209bcfb8dcae0af7dbf29867fab8ca` | 代币推送/运维合约 | 对应代币推送/运维方法。 |

## 结构判断

- 如果把系统交易单独拿出来看，IoTeX 这一天的大部分链上活动其实是“协议奖励相关动作”，而不是普通用户转账。
- 如果把系统交易剔掉再看，剩余交易里也有非常大的比例来自少数重复任务簇，特别是 `ioSwarm` 奖励池结算相关的 `depositSettleAndClaim(address[],uint256[],address[])` 和 `Native Staking` 相关的 `depositToStake(uint64,uint256,uint8[])` 两组。
- 真正相对更像自然用户活动的是 `native-transfer` 那一小层，但规模只有 `403` 笔，只占总交易的 `1.02%`。
- 再结合同日 DEX 成交几乎为零，这个链上活跃画像更接近“系统奖励 + 自动化任务网络”，而不是“高频真实用户生态”。

## 说明

- 本报告把 Dune 的 `2026-04-12` 解释为 UTC 日；这是与 Dune 日表对齐的合理假设。
- Dune 在本报告中的作用，仅是提供日期口径和日度汇总指标参考，用来与官方链上数据做交叉核实；具体交易结构、方法调用和地址行为分析，全部以 IoTeX 官方 RPC 与区块浏览器/API 数据为准。
- `0x236cbf52125e68db8fa88b893ccafb2ee542f2d9` 的业务身份来自 IoTeX 官方 ioSwarm 文档与 IIP-58，其中将它列为主网 `AgentRewardPool v2` 合约；样本调用的 calldata 结构与 `depositSettleAndClaim(address[],uint256[],address[])` 精确一致。
- `0x04c22afae6a03438b8fed74cb1cf441168df3f12` 的业务身份来自 IoTeX 官方 IIP-12 接口文档，它是 `Native Staking Virtual Contract`；`cast sig 'depositToStake(uint64,uint256,uint8[])'` 可直接得到 `0x34e8e145`。
