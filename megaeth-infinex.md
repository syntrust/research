
<img src="_attachments/20260423143428.png" width="566" />


## 关于 Infinex

 Infinex 是一个跨链的“加密超级应用”，集钱包、投资组合管理、交易终端于一体。

功能特性 https://infinex.xyz/features
- Portfolio
- NFT
- Perps (powered by Hyperliquid)
- Swidge (Swap and bridge, across 20+ chain)
- Gas account (to pay for network costs everywhere)
## 注册 Infinex Account

<img src="_attachments/20260417181808.png" width="357" />
<img src="_attachments/20260417181841.png" width="357" />

<img src="_attachments/20260417181921.png" width="365" />

<img src="_attachments/20260417182107.png" width="369" />
<img src="_attachments/20260417182330.png" width="318" />
<img src="_attachments/20260417182439.png" width="318" />


### 关于 PassKey

Passkey 是一种**无密码登录**方式，用手机、电脑里的密钥配合指纹、人脸识别或设备 PIN 来完成身份验证，比传统密码更安全，也更不容易被钓鱼攻击。它的核心原理是公私钥加密：网站保存公钥，私钥留在你的设备上，登录时通过签名验证身份 。

两种类型：
- 同步型 passkey：比如 iCloud Keychain、Google 密码管理器这类方式，换手机后也更容易找回 。风险：云账号攻破。
- 设备绑定型 passkey：只存在于某一台设备或硬件安全密钥里，不会自动同步，安全性和隔离性更强，但迁移更麻烦。风险：设备丢失或被盗

优点：
- 方便：Passkey 不需要记忆，也不需要每次输入。通常只需要指纹、人脸或设备 PIN 就能完成登录 。
- 比密码安全：无法跨网站盗用，防钓鱼，不怕撞库，难以暴力破解。
#### Passkey 在加密和 web3 领域的应用
- 钱包创建与登录
- 交易签名
- DApp 登录
优点：不用助记词，抗钓鱼，生物识别（体验更像 Web2）

### 关于 Turnkey 

- Infinex 的安全保障：私钥如何安全保存 + 如何签名
- [Turnkey](https://www.turnkey.com/security-by-turnkey) 使用 TEE  (AWS Nitro Enclaves)，可隔离每个加密操作。

注册流程

```
Passkey（Google / Apple / WebAuthn）
        ↓
证明你是这个用户（authentication）
        ↓
Infinex backend
        ↓
调用 Turnkey
        ↓
在 Nitro Enclave 里生成/使用私钥（EOA）
```

签名流程
 ```
用户（Passkey）
   ↓ 验证身份
Infinex backend
   ↓ 授权请求
Turnkey（Nitro Enclave）
   ↓
私钥签名（永不出 enclave）
 ```
## 体验 Infinex
### infinex 官网 
https://app.infinex.xyz
	- DeFi 相关，Infinex wallet 没钱玩不了
	- 如果已经有钱，说明不是从 web2 来的新用户，谈不上改善用户体验

### megaETH 入口 
https://infinex.xyz/networks/megaeth

<img src="_attachments/20260423144759.png" width="278" />
<img src="_attachments/20260423144851.png" width="283" />

唯一可用：需要给游戏内钱包充值：

<img src="_attachments/20260423154742.png" width="390" />

### 从 rabbithole 进入
https://rabbithole.megaeth.com/featured-apps 
- 选择 Infinex 作为钱包
- 基本不在白名单

<img src="_attachments/20260423154654.png" width="284" />
<img src="_attachments/20260423161118.png" width="285" />

唯一可用：需要给游戏内钱包充值：
<img src="_attachments/20260423162126.png" width="423" />


### Swidge 交易

<img src="_attachments/20260423173253.png" width="420" />

在页面操作时 需要 Passkey 验证签名；在 chrome extension 没有
<img src="_attachments/20260423174004.png" width="399" />

https://megaeth.blockscout.com/tx/0xaea4b7b4d66c94b1cd5e66733a0811ffebb290566208e0df427f0e2c1bbf366c

- From 0xE2D4A7ff2b7bB9f92AD5d1eDd438224C1646733C （relayer/bot 支付 gas fee）
- Interacted with contract （ERC-4337）
- 收取一笔 0.000885 费用 给 0xd32c062c12C2D10BeC0187DD334cC15E0367f9AC
- 通过 Router / Forwarder 给 EisenDiamond  
- 找 0xae...d16 和 Liquidity Book Token  两个池子 分别兑换 0.29


## Gas Account 

- Infinex abstracts network costs ("Gas") for most onchain transactions undertaken within your account. 
- You can top up your Gas Account by purchasing credits at any time using any of the Infinex Supported Assets. 
- The token you use to purchase credits is converted at or about the prevailing USD spot price into credits, and added to your credit balance in your Gas Account.
- Purchased credits cannot be withdrawn or refunded
- 
<img src="_attachments/20260417185148.png" width="319" />


<img src="_attachments/20260422161713.png" width="319" />

### 给 Infinex 账户 充值 USDT0
（略）

### 给 Gas Account 充值 （用 megaETH 上的 USDT0）

注意：充值后不能撤出

<img src="_attachments/20260422171111.png" width="346" />
<img src="_attachments/20260422171328.png" width="344" />

尝试几次后成功

<img src="_attachments/20260422171656.png" width="319" />

给游戏账户充值：

<img src="_attachments/20260422171921.png" width="327" />

<img src="_attachments/20260423163126.png" width="347" />

 交易 费用从 Gas Account 中扣除
 
<img src="_attachments/20260422172113.png" width="324" />

<img src="_attachments/20260422173231.png" width="482" />


<img src="_attachments/20260422172458.png" width="489" />


https://megaeth.blockscout.com/address/0xE493838029fc44F973E83fb71e4025668BEdF572?tab=txs

每个操作都上链：

![](_attachments/20260422172658.png)

### 跨链

<img src="_attachments/20260422173527.png" width="390" />



<img src="_attachments/20260422173822.png" width="407" />


## 总结

### 实现
https://proposals.infinex.xyz/xips/xip-93
- 使用 Turnkey 管理私钥和签名。
- 使用[Zerodev](https://docs.zerodev.app/#zerodev-introduction)账户抽象钱包实现基于 ERC-4337 标准的 EVM 账户。

### Gas 支付的三种方式
https://infinex.xyz/legals/network-costs

1. 从交易使用的代币中收取， 从
2. Gas Account 中收取，或 
3. 直接使用当前链的原生代币支付。

### 费用来源
(https://proposals.infinex.xyz/xips/xip-94)
- 汇总收取所有 bridging 和 swap 费用，并定期转入 Infinex relayer 以支付 gas 费用。
- "Not worrying about gas" 是平台垫付，通过交易税的方式收取



