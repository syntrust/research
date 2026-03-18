
# **Why `BLOCKHASH` Differs Between `eth_estimateGas` and Real Execution**

## **Background**

The EVM specification restricts access to block hashes:

> `BLOCKHASH` can only access the most recent 256 blocks, i.e.
> `[block.number - 256, block.number - 1]`, excluding the current block.

This implies:

```text
blockhash(block.number) = 0
```

---

## **Initial Observation**

While testing a contract, I observed that:

> The result of `BLOCKHASH` during `eth_estimateGas` differs from real transaction execution.

---

## **Minimal Reproduction Contract**

```solidity
event CurrentBlockHash(uint256 current, uint256 provided, bytes32 currentBlockHash);

function testBn(uint256 bn) external {
    require(bn != block.number, "Provided block number matches current block number");
    require(bn < block.number, "Provided block number is in the future");
    emit CurrentBlockHash(block.number, bn, blockhash(bn));
}
```

---

## **Experiment**

### Using default (`latest`)

```bash
BN=$(cast block-number -r $RPC)

DATA=$(cast calldata "testBn(uint256)" $BN)

cast rpc eth_estimateGas \
  "{\"from\":\"$FROM\",\"to\":\"$ADDR\",\"data\":\"$DATA\"}" \
  -r $RPC
```

The call **reverts**:

```text
Provided block number matches current block number
```

This shows:

```text
block.number = BN
```

So:

```text
BLOCKHASH(BN) → invalid → 0
```

---

### Real transaction execution

```bash
cast send $ADDR "testBn(uint256)" $BN --private-key $PK -r $RPC
```

The transaction **succeeds**, and emits:

```text
block.number = BN + 1
BLOCKHASH(BN) ≠ 0
```

---

### Using `pending`

```bash
BN=$(cast block-number -r $RPC)
BN=$((BN+1))

DATA=$(cast calldata "testBn(uint256)" $BN)

cast rpc eth_estimateGas \
  "{\"from\":\"$FROM\",\"to\":\"$ADDR\",\"data\":\"$DATA\"}" \
  pending -r $RPC
```

Observed behavior:

* With `bn = latest + 1` → succeeds
* Increment once more:

```bash
BN=$((BN+1))
```

* Now it reverts with:

```text
Provided block number matches current block number
```

---

## **What This Implies**

From the contract constraints:

```solidity
require(bn < block.number);
require(bn != block.number);
```

we can deduce:

* First call passes → `bn < block.number`
* Second call fails with equality → `bn == block.number`

Given the inputs:

* First: `bn = latest + 1`
* Second: `bn = latest + 2`

the only consistent conclusion is:

```text
block.number = latest + 1
```

---

## **Putting It All Together**

| Context                 | block.number | BLOCKHASH(N)  |
| ----------------------- | ------------ | ------------- |
| `estimateGas (latest)`  | N            | ❌ invalid (0) |
| `estimateGas (pending)` | N + 1        | ✅ valid       |
| Real execution          | N + k        | ✅ valid       |

---

## **Explanation**

The discrepancy comes from differences in execution context:

* `eth_estimateGas` with `latest` executes against the current block (`N`)
* `eth_estimateGas` with `pending` executes against a pending block (`N + 1`)
* Real execution happens in a future block (`N + k`, typically `N + 1`)

This directly affects block-dependent opcodes such as:

* `BLOCKHASH`
* `NUMBER`
* `TIMESTAMP`
* `PREVRANDAO`

---

## **Takeaway**

> `eth_estimateGas` is context-dependent, and its result may diverge from real execution when contracts rely on block metadata.

In particular:

* Using `latest` may produce results that cannot occur on-chain
* Using `pending` more closely approximates real execution
