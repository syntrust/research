---
title: "Stock Tokens：你买到的不是股票，而是一张链上债权"
created: 2026-07-21
updated: 2026-07-24
status: "active"
tags:
  - "research"
  - "rwa"
  - "robinhood"
---

# Stock Tokens：你买到的不是股票，而是一张链上债权

以 Robinhood Chain 上的 NVDA 为例，Stock Token 解决的是“如何把美股的经济敞口搬到链上”，而不是“如何把英伟达股票本身变成 ERC-20”。

最重要的区别只有一句话：

> **持有 NVDA Token，不等于持有 NVIDIA 股票。你持有的是 Robinhood Assets (Jersey) Limited（RHJ）发行的一张代币化债务证券。**

它会尽量跟踪 NVDA 的价格和分红，但不赋予 NVIDIA 股东身份、投票权或对 NVIDIA 的直接请求权。Robinhood 的[产品说明](https://docs.robinhood.com/rhj/product/)和 [NVDA 最终条款](https://cdn.robinhood.com/assets/robinhood/legal/rhj_final_terms_for_tokenised_debt_securities_linked_to_nvidia.pdf#page=14)都明确写明了这一点。

## 一张表看懂你买到了什么

| 问题 | Stock Token 的实际安排 |
|---|---|
| 法律性质 | RHJ 发行的代币化债务证券 |
| 经济敞口 | 跟踪一只股票或 ETF 的价格，并按条款反映分红等公司行动 |
| 股东权利 | 没有底层公司的所有权、投票权、参会权或优先认购权 |
| 抵押安排 | 产品按条款进行足额抵押，抵押物主要是底层股票，也可能包括现金、合格金融工具和结算中的请求权 |
| 分红 | 不直接派发现金，而是再投资后提高每枚 Token 对应的股票数量 |
| 日常买卖 | 普通用户主要通过 RFQ、AMM、订单簿或其他二级市场交易 |
| 直接赎回 | 通常经授权参与者（AP）处理；满足特定条件并完成 KYC/AML 后，投资者也可向发行人申请 |
| 赎回所得 | 现金，不是真实股票 |

这里最容易产生误解的是“1:1 背书”。Robinhood 的面向用户页面称每枚 Token 都有对应股票支持，但法律文件的表述更精确：产品是**足额抵押、有限追索**的债务证券，抵押物“主要”由底层股票构成；[NVDA 最终条款第 5 页](https://cdn.robinhood.com/assets/robinhood/legal/rhj_final_terms_for_tokenised_debt_securities_linked_to_nvidia.pdf#page=5)还允许发行人出借底层股票，并以等值现金或其他合格金融工具作为抵押。因此，它并不意味着“每一枚 Token 背后始终静态锁着一股 NVDA”。

## 分红和拆股如何反映到链上

Stock Token 是 18 位小数的 ERC-20，同时实现了仍处于 Draft 状态的 [ERC-8056](https://eips.ethereum.org/EIPS/eip-8056)。这个扩展引入了 `uiMultiplier`，用来表示“一枚 Token 对应多少股底层股票”。

初始状态通常是：

```text
1 NVDA Token = 1 股 NVDA
uiMultiplier = 1.0
```

如果 NVIDIA 派发现金股息，RHJ 不会把现金直接打到持币人的钱包，而是按条款将净股息再投资。你的原始 Token 余额和 `totalSupply` 不变，但 `uiMultiplier` 会提高。例如：

```text
钱包原始余额：100 NVDA Token
uiMultiplier：1.02
对应股票数量：100 × 1.02 = 102 股
```

因此，链上价格预言机返回的是：

```text
Token 价格 = 底层股票价格 × uiMultiplier
```

Robinhood 的[开发者文档](https://docs.robinhood.com/chain/building-with-stock-tokens/)说明，Chainlink 返回的价格已经包含乘数，应用不应再次相乘。拆股、分红等公司行动处理期间，Token 交易和价格源还可能短暂停止更新，具体机制见[公司行动说明](https://docs.robinhood.com/rhj/corporate-actions/)和[预言机文档](https://docs.robinhood.com/chain/oracles-and-price-feeds/)。

## 为什么还需要这种产品

如果它不是真股票，用户为什么仍可能需要它？核心价值主要有三类。

### 1. 把交易时间和持有方式搬到链上

符合资格的用户可以在自托管钱包中持有和转移 Stock Token，二级市场也可以在传统交易所闭市时继续运行。Robinhood 称产品已通过 Robinhood Wallet 面向 120 多个国家开放，但实际可用性仍取决于司法辖区和用户资格，不能把“钱包可访问”理解成“全球任何人都可合法购买”。相关范围见 Robinhood 的[主网上线公告](https://robinhood.com/us/en/newsroom/robinhood-accelerates-global-expansion-robinhood-chain-mainnet-stock-tokens-agentic-trading/)和[受限地区清单](https://docs.robinhood.com/rhj/restricted-jurisdictions/)。

### 2. 降低跨境和碎股门槛

Token 可以像普通 ERC-20 一样拆分到很小的单位，也能在兼容钱包之间转移。对难以开设美股账户、跨境汇款成本较高或只想投入很小金额的用户，这种形式可能比传统券商账户更容易接触。

但它降低的是产品和结算摩擦，不会自动免除当地证券法、税务申报、外汇管理和合规义务。

### 3. 获得链上可组合性

Stock Token 可以被钱包、交易协议、借贷市场或自动化策略调用。Robinhood 的[集成文档](https://docs.robinhood.com/chain/building-with-stock-tokens/)列出了 RFQ、AMM、借贷、指数篮子和衍生品等场景。

可组合性同时意味着风险叠加：把 Stock Token 存入借贷协议后，用户承受的不再只是 NVDA 价格波动，还包括发行人、抵押物、预言机、智能合约和清算机制的风险。

## 一级市场：Token 是如何被铸造和赎回的

普通用户不能直接要求 RHJ 铸币。一级市场由授权参与者（Authorised Participant，AP）连接发行人和二级市场。截至 NVDA 最终条款签署日，唯一公开列出的 AP 是 Bitstamp Global；其他主要服务商包括 Alpaca Securities、JPMorgan London 和 Security Agent Services AG，详见 [RHJ 服务商清单](https://docs.robinhood.com/rhj/service-providers/)。

### 铸造：AP 申购，RHJ 买入抵押物并发行 Token

根据[基础招股书第 97—98 页](https://cdn.robinhood.com/assets/robinhood/legal/rhj_base_prospectus.pdf#page=97)，流程可以压缩为：

```text
AP 向 RHJ 提交申购订单
        ↓
RHJ 指示 Alpaca 买入相应抵押物
        ↓
券商确认订单后，RHJ 创建 Token 并转给 AP
        ↓
最迟下一个结算工作日：
AP 支付款项，抵押物进入 RHJ 的托管账户
```

这里有两个重要细节：

- 这是现金申购，不是 AP 直接把一篮子股票交给发行人。
- 铸币、付款和股票正式进入托管账户并非同一时刻完成；结算期间的抵押物可能表现为 RHJ 对券商的交付请求权。

因此，更准确的说法是“发行与抵押物结算相互配套”，而不是“每次 Mint 之前都已有一股股票静态锁在链下账户中”。

### 赎回：销毁 Token，卖出抵押物，收到现金

普通投资者日常退出通常是在二级市场卖出。直接赎回则需要经 AP 处理，或满足招股书规定的直接赎回条件，并完成发行人的 KYC/AML 审查。

发行人接受赎回申请后，会指示券商卖出对应抵押物；卖单被接受后，Token 被转入指定钱包并销毁，出售所得通常在下一个结算工作日用于支付赎回款。投资者无权要求交付真实 NVDA 股票。完整步骤见[基础招股书第 100—101 页](https://cdn.robinhood.com/assets/robinhood/legal/rhj_base_prospectus.pdf#page=100)。

## 二级市场：普通用户实际在哪里交易

Robinhood 文档列出四类流动性来源：

- **RFQ**：0x RFQ、1inch Fusion、LiFi 等聚合器从做市商获取带签名、带期限的报价；
- **AMM**：例如 Uniswap 的公开流动性池；
- **专有 AMM**：例如由专业做市资金支持的 Rialto；
- **订单簿或衍生品场所**：例如 Lighter。

其中，RFQ 可以理解为“先询价，再原子结算”。做市商的报价通常会参考以下因素：

```text
参考报价
≈ 底层股票价格 × uiMultiplier
+ 库存与对冲成本
+ 闭市和波动风险溢价
+ 做市商点差
```

这只是对做市逻辑的概括，并不是发行文件规定的固定公式。不同场所的实际报价、费用和可成交深度可能明显不同。

## Token 价格为什么能跟住真实股票

锚定机制与 ETF 的申购赎回有相似之处：当 Token 相对理论价值出现明显溢价或折价时，有一级市场权限的参与者可以通过铸造或赎回扩大套利空间，从而推动价格回归。

假设：

```text
NVDA 股价：$200
uiMultiplier：1.02
Token 理论价值：$204
```

如果 Token 在二级市场卖到 $210，AP 或做市商可以建立 NVDA 对冲头寸、申购新 Token，再把 Token 卖入溢价；新增供给会给 Token 价格带来向下压力。

如果 Token 只卖 $198，套利者可以在二级市场买入便宜的 Token，申请赎回并同时管理股票端敞口；买盘和销毁会给 Token 价格带来向上压力。

不过，这条套利链路比“实物申赎 ETF”更长。它依赖 AP、券商、发行人、托管、KYC/AML 和结算工作日，而且股票闭市时无法同步完成底层买卖。RHJ 也明确承认，流动性、Mint/Burn 定价和市场环境都可能造成偏离；其[价格偏离披露页](https://docs.robinhood.com/rhj/price-deviations/)会在链上价格相对参考价格连续七个交易日偏离 5% 或以上时进行披露。

## 终端用户最需要注意的六类风险

### 1. 你承担的是“股价风险 + 发行结构风险”

NVDA Token 是 RHJ 的债务，不是 NVIDIA 的股票。它属于**有担保、有限追索**债务：担保代理可以代表投资者执行特定系列的抵押物，但清算税费、执行费用和部分服务商费用的受偿顺序可能排在投资者之前；如果剩余资产不足，投资者不能向 RHJ 的其他资产或其他产品系列继续追索。

相关权利、执行顺序和限制见[基础招股书第 108—111 页](https://cdn.robinhood.com/assets/robinhood/legal/rhj_base_prospectus.pdf#page=108)以及 [NVDA 最终条款第 14—16 页](https://cdn.robinhood.com/assets/robinhood/legal/rhj_final_terms_for_tokenised_debt_securities_linked_to_nvidia.pdf#page=14)。

### 2. “足额抵押”不等于“没有交易对手风险”

NVDA 条款允许底层股票被出借。出借期间，抵押物可能变成现金或其他合格金融工具，投资者因此会额外暴露于借券、替代抵押物、托管和执行风险。

即使不发生借券，发行和赎回的结算期间也存在券商请求权和资金在途状态。足额抵押降低了发行人风险，但没有消除法律执行和操作风险。

### 3. Token 交易 24/7，股票和一级市场却不是

链上池子周末仍可交易，但美国股票市场、券商和一级申赎流程有营业时间。闭市期间，做市商更难实时对冲，点差、滑点和价格偏离通常会扩大。

基础招股书把“有限交易时间”列为高风险因素；NVDA 最终条款也明确表示，二级市场流动性没有保证，见[招股书第 59 页](https://cdn.robinhood.com/assets/robinhood/legal/rhj_base_prospectus.pdf#page=59)和 [NVDA 最终条款第 16 页](https://cdn.robinhood.com/assets/robinhood/legal/rhj_final_terms_for_tokenised_debt_securities_linked_to_nvidia.pdf#page=16)。

### 4. 一级市场参与者集中

截至 NVDA 最终条款签署日，唯一列名的 AP 是 Bitstamp Global。AP 负责连接发行、赎回和市场流动性；如果它暂停服务、受监管限制或退出，而替代参与者尚未接入，Token 的价格锚定和退出效率都会受到影响。

这不等于“AP 一停，所有 Token 立即失效”，但意味着一级市场目前存在明显的集中度风险。

### 5. DeFi 会放大闭市、预言机和清算风险

Robinhood 的 Chainlink Stock Token 价格源按 **24/5** 更新，而 Token 本身可以 **24/7** 转移和交易。公司行动期间，价格源还可能暂停。借贷协议如何处理周末价格、过期报价、sequencer 中断和公司行动，直接决定用户是否会遭遇错误定价或意外清算。

因此，在把 Stock Token 用作抵押品之前，应核对具体市场的 heartbeat、staleness check、sequencer uptime check、清算阈值和暂停机制，而不能仅凭“有 Chainlink 喂价”判断安全。技术注意事项见 Robinhood 的[预言机文档](https://docs.robinhood.com/chain/oracles-and-price-feeds/)。

### 6. 可用性和监管边界会变化

这些产品依据 Regulation S 排除美国和美国人士，并在加拿大、英国、瑞士等地区受到限制。美国 SEC 工作人员在 2026 年 1 月的[代币化证券声明](https://www.sec.gov/newsroom/speeches-statements/corp-fin-statement-tokenized-securities-012826-statement-tokenized-securities)中指出，第三方发行的代币化产品可能不赋予底层证券权利，并会引入真实股东通常不会承担的第三方破产风险。

这份 SEC 文件是工作人员声明，不是新规则，也没有直接裁定 Robinhood 产品是否合法。更稳妥的结论是：产品资格、销售范围和监管分类仍可能随司法辖区和规则变化，用户不能把今天能够访问某个前端等同于长期、无条件的可交易权。

## 结论：它卖的是可编程敞口，不是链上股东身份

Stock Token 的价值很清楚：它把美股价格敞口做成了可自托管、可拆分、可转移和可组合的链上资产。

它增加的风险也同样清楚：

```text
底层股票价格
+ RHJ 发行人和有限追索结构
+ AP、券商、托管与替代抵押物
+ 二级市场流动性和闭市价差
+ 预言机、智能合约与自托管
+ 各司法辖区的销售限制
```

对无法便利使用传统美股券商、又理解这些额外风险的合资格用户，它可能提供有价值的新入口。对已经拥有低成本、受本地监管保护的美股账户，而且只需要长期持有股票的人，直接持股通常结构更简单。

评估任何一只 Stock Token，可以依次问六个问题：

1. 发行人是谁，我对底层公司到底拥有什么权利？
2. 抵押物是什么，是否允许出借或替换？
3. AP、券商、托管人和担保代理分别是谁？
4. 我能否直接赎回，最终拿到现金还是股票？
5. 闭市、价格源暂停或 sequencer 故障时，协议如何定价和清算？
6. 我所在地区是否允许购买、持有和转让？

以上为机制梳理，不构成投资、法律或税务建议。条款和地区限制变化较快，应以 [RHJ 披露库](https://robinhood.com/eu/en/legal/rhj/)中的最新基础招股书、补充文件和对应产品最终条款为准。
