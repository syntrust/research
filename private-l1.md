# Private L1: Privacy as a First-Class Citizen in the Strawmap

## 1. Introduction

In Ethereum’s long-term roadmap (the **Strawmap**), **privacy** appears only once as an explicit goal:

> **Privacy as a first-class citizen, via L1 shielded transfers.**

Despite the brief wording, this statement represents one of the **five “north stars”** of the roadmap. These north stars are highlighted with black boxes in the Strawmap diagram and represent the most important long-term design goals for Ethereum.

Among the five highlighted goals:

- **Three focus on performance and scalability**
- **One focuses on post-quantum security**
- **One focuses on privacy**

The presence of privacy in this group indicates that **privacy is considered a protocol-level priority**, not merely an application-layer feature.

However, the planned timeline places this feature **very far in the future (2030+)**, suggesting that major technical prerequisites must be solved before it can be implemented safely.

---

# 2. What “Privacy as a First-Class Citizen” Means

The phrase **“privacy as a first-class citizen”** implies that privacy is **native to the protocol itself**, rather than implemented through optional tools or smart contracts.

This distinction is important.

Today most Ethereum privacy solutions operate at the **application layer**, meaning:

- They rely on **external protocols**
- They often require **wrapped assets** such as WETH
- Privacy guarantees depend on **specific contracts or services**

A first-class privacy model would instead provide **native shielded transfers at the L1 level**.

At minimum, this implies:

- **Native ETH transfers can be shielded**
- Privacy does **not depend on external tools**
- Privacy is enforced **by the protocol itself**

This model is closer to privacy-oriented blockchains such as Zcash or Monero.

---

# 3. Why Privacy Matters: Evidence from the PSE Survey

Research from the **Ethereum Privacy and Scaling Explorations (PSE)** team suggests that privacy demand is not the problem.

The problem is **usability**.

Key findings from the survey include:

- **Privacy is non-negotiable** for many users
- Current solutions **fail in real-world user experience**
- Privacy tools are **frequently tried but rarely become habitual**
- Users strongly prefer **privacy by default**

In other words, the adoption gap is driven **not by lack of interest**, but by **poor product experience and technical limitations**.

This reinforces the argument that privacy must be **built into the base protocol**, rather than added as optional tooling.

---

# 4. Technical Prerequisites in the Strawmap

The Strawmap indicates that **Private L1 depends on several major protocol upgrades**.

The dependency chain looks roughly like this:

```

Native Account Abstraction (AA)
↓
NTT Precompile
↓
Post-Quantum Transactions
↓
Private L1 (Shielded Transfers)

```

Another related infrastructure upgrade is:

```

Sharded Mempool → Post-Quantum Transactions

```

These dependencies explain why privacy appears **late in the roadmap**.

---

# 5. Why Private L1 Depends on Post-Quantum Transactions

The dependency on **post-quantum security** is particularly important.

Privacy systems store encrypted information **on-chain**, including data such as:

- Who transferred funds
- Who received them
- The transfer amount

If the cryptography protecting these transactions is later broken, the previously hidden information could become **permanently public**.

Unlike ordinary transactions, this privacy loss would be **irreversible**, because the encrypted data would already be stored on-chain.

This is why Ethereum must ensure that **the zero-knowledge proof systems used for shielded transfers are post-quantum secure**.

Otherwise, private transactions could eventually be decrypted by quantum computers—similar to concerns raised about **Zcash shielded transactions**, which rely on cryptographic assumptions vulnerable to future quantum attacks.

Therefore, Ethereum cannot safely implement **“privacy as protocol”** until its transaction system supports **post-quantum cryptography**.

---

# 6. NTT Precompile and Native Account Abstraction

Two other dependencies are required to enable post-quantum transactions.

### Native Account Abstraction (EIP-8141)

Native account abstraction restructures transactions as a sequence of **execution frames**, allowing Ethereum to redefine key transaction components.

This design enables:

- Alternative signature schemes
- Flexible fee payment mechanisms
- Custom transaction validation logic

In practice, this flexibility allows Ethereum to **replace ECDSA signatures** with **post-quantum alternatives**.

---

### NTT Precompile

The **NTT (Number Theoretic Transform) precompile** provides a highly optimized primitive used in modern proof systems.

Its role includes:

- Accelerating **STARK verification**
- Supporting **post-quantum cryptographic primitives**
- Improving performance of large polynomial operations used in ZK proofs

Together, these upgrades provide the cryptographic infrastructure required for **post-quantum secure privacy systems**.

---

# 7. Sharded Mempools

Another supporting upgrade is the **sharded mempool design**.

In this model:

- Validators only download **a subset of blob transactions**
- Each node stores **part of the blob data in its mempool**

This reduces bandwidth and memory requirements for block builders.

Sharded mempools were proposed in research such as:

https://ethresear.ch/t/a-new-design-for-das-and-sharded-blob-mempools/22537

This infrastructure helps scale transaction throughput while maintaining decentralized participation.

---

# 8. Privacy at the Transaction Ingress Layer

Vitalik Buterin has emphasized that privacy must also be considered at the **network layer**, not just in transaction content.

Even if a transaction is cryptographically private, it can still be exposed during **transaction propagation**.

When a transaction is visible while entering the network, attackers can perform:

- **Sandwich attacks**
- **Transaction griefing**
- **De-anonymization through RPC endpoints or public mempools**

Possible solutions include **network-level privacy mechanisms**, such as:

- Tor-style routing
- Ethereum mixnets
- Low-latency privacy networks like Flashbots’ **Flashnet**

These technologies may eventually integrate with privacy initiatives such as **Kohaku**.

---

# 9. Client-Side ZK Proof Generation

Another challenge for private transactions is **proof generation performance**.

Privacy systems typically require users to generate **zero-knowledge proofs locally**.

However, today’s proof generation is often **too slow for everyday usage**.

Research from PSE suggests that **client-side GPU acceleration** could solve this problem.

Key idea:

- ZK proofs are generated **directly on users’ devices**
- GPU acceleration makes proof generation **fast enough for mainstream adoption**
- The approach also aligns with **post-quantum cryptographic directions**

Without major improvements in proof performance, privacy systems cannot scale to everyday Ethereum usage.

---

# 10. Possible Future Directions

Several experimental projects may contribute ideas toward Ethereum’s long-term privacy architecture.

These should be viewed as **research directions rather than finalized designs**.

---

## PlasmaFold

**PlasmaFold** is a prototype combining **Plasma-based L2 designs** with **proof folding schemes**.

Folding techniques (such as **Nova** or **Sangria**) allow many proofs to be recursively compressed into a single proof.

When combined with **Proof-Carrying Data (PCD)**, folding can drastically reduce:

- Memory requirements
- Proof generation costs

This makes it possible for privacy proofs to run even on **low-end hardware**.

---

## Intmax

**Intmax** is an Ethereum Layer 2 protocol focused on:

- **Extreme scalability**
- **Native privacy**
- **Stateless architecture**

It can be viewed as an evolution of the original Plasma concept.

In Intmax:

- Most transaction data is **not stored on-chain**
- Users maintain local state
- The network verifies correctness using cryptographic proofs

This approach significantly reduces data availability costs.

---

## Kohaku

**Kohaku** is envisioned as a collection of **privacy-first Ethereum tooling**, including:

- Privacy wallets
- Privacy pools (such as **Railgun**)
- Post-quantum accounts
- Network-level privacy infrastructure

It may serve as an experimental platform for integrating multiple privacy technologies across the Ethereum stack.

---

## Stealth Addresses (ERC-5564)

Another important building block is **stealth addresses**.

Stealth addresses create **one-time receiving addresses** derived from a recipient’s public key.

This allows:

- The recipient to receive funds
- Observers to **not link the payment to the recipient’s main address**

Stealth addresses provide an additional anonymity layer for on-chain transfers.

---

# 11. How Other Privacy L1s Work

Several blockchains already treat privacy as a core feature.

These systems provide useful reference models.

---

## Zcash

Zcash introduced the **shielded pool** concept using **zk-SNARKs**.

The transaction model works as follows:

### Commitment

A user creates a **note (UTXO)** containing:

- Amount
- Secret key

The note is hashed to form a **commitment**, which is inserted into a **Merkle tree**.

---

### Spend Proof

When spending a note, the user generates a **zk-SNARK proof** showing:

- The note exists in the Merkle tree
- The user owns the note
- The note has not been spent

Importantly, the proof reveals **none of the following**:

- Transaction amount
- Sender address
- Recipient address

---

### Nullifier

To prevent double-spending:

- Each spend reveals a **nullifier**
- The nullifier is added to a global **nullifier set**

The recipient receives an **encrypted note plaintext** representing the new shielded UTXO.

---

## Monero

Monero enforces privacy **by default** using:

- **Ring signatures**
- **Stealth addresses**
- **Confidential transactions**

Every transaction is private, making it the closest example of **mandatory privacy digital cash**.

---

## Iron Fish

Iron Fish generalizes Zcash’s Sapling protocol to support **multiple assets**, not just a native token.

The design focuses heavily on **user experience** while maintaining shielded transfers.

---

## Aleo

Aleo extends privacy beyond transfers by enabling **private smart contract execution**.

It uses the **ZEXE model**, where state transitions are computed **off-chain** and verified with zero-knowledge proofs.

---

## Namada

Namada introduces a **Multi-Asset Shielded Pool (MASP)**.

Assets from multiple chains share a **single anonymity set**, increasing the effectiveness of privacy protections.

---

# 12. Conclusion

Ethereum’s Strawmap includes only one explicit privacy goal:

> **Privacy as a first-class citizen via L1 shielded transfers.**

Despite its short description, the goal carries significant implications:

- Privacy must be **native to the protocol**
- Shielded transfers must support **native ETH**
- The system must be **post-quantum secure**
- Privacy must extend beyond cryptography to **network-layer protections**

Because these requirements depend on major upgrades—such as **native account abstraction**, **NTT precompiles**, and **post-quantum transactions**—the feature appears late in the roadmap.

However, its position among the roadmap’s **five north stars** shows that Ethereum considers privacy not a niche feature, but a **foundational property of the future protocol**.

## References

- https://strawmap.org/
- https://pse.dev/blog/px-user-survey-2025
- https://pse.dev/blog/pse-roadmap-2025#1-private-transfers
- https://pse.dev/blog/client-side-gpu-everyday-ef-privacy
- https://x.com/VitalikButerin/status/2028524112868708616
- https://www.reddit.com/r/ethereum/comments/1rera79/comment/o7ixy4x/?context=3
- https://vitalik.eth.limo/general/2023/01/20/stealth.html
