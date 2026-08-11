---
id: Lecture5-Ethereum_Scaling
aliases: []
tags: []
---

  ## Ethereum scaling ##

Ethereum has a scalability trilemma:
  - Security.
  - Decentralisation.
  - Scalability.

  It is very difficult to optimise all 3 of these in a single layer.

L0 is the hardware layer. L1 is the base blockchain layer, i.e base consensus and block-chain itself so Ethereum or BTC. L2 is where we find our optimisations now....

### L2 ###

- Execute transactions of chain and use the L1 layer as an anchor of truth.
- The basic idea:

  - Nodes submit transactions to an L2 RPC which are handled by a "sequencer".
  - sequencer completes transactions using a dummy EVM.
  - Sequencer bundles a bunch of these txs into batches and sends them to the L1 layer in regular Ethereum transactions. 
    This transaction submitted to the L1 Ethereum layer is a call to a "Rollup Contract". The key thing to understand is that the Rollup Contract is written and maintained by the team that handles the L2 layer. They wrote the code for the block and published it onto the global Ethereum chain.

  - The transaction to the Rollup Contract contains a summary of all the Txs that the batch included and a pre and post state root. 

  There are 2 ways of how things can proceed from now:
  

## 1. Big Idea
Ethereum L1 is secure but slow.  
**Rollups (L2s)** move execution off-chain and use Ethereum only for **data + verification**, giving scalability *without* losing security.

---

## 2. Rollup Basics
- **Sequencer:** orders & executes txs off-chain.  
- **Rollup contract (on L1):** records batches, proofs, deposits, withdrawals.  
- **Ethereum validators:** run that contract’s code → enforce rules.  
- **Proofs:** ensure off-chain execution was correct.

Result: fast cheap txs on L2, Ethereum-level trust on L1.

---

## 3. Keeping It Permissionless
- Users can **bypass the sequencer** by sending txs directly to the L1 rollup contract.  
- Contract forces sequencer to include these or its batch becomes invalid.  
- Data for every batch is stored on L1 → anyone can verify or rebuild.  
✅ No one can permanently censor users.

---

## 4. Fallback & Recovery
All L2 data lives on Ethereum:  
1. Anyone can **re-run** posted batches → rebuild full L2 state.  
2. Users can **prove balances or exits** directly to the rollup contract.  
3. Ethereum verifies and releases funds.  
Even if the L2 dies, assets and state remain safe on L1.

---

## 5. Trust & Security Across Layers
| Guarantee | Mechanism |
|------------|-----------|
| Correctness | Fraud / validity proofs checked by Ethereum |
| Data integrity | All tx data posted on L1 |
| Economic honesty | Sequencer bond slashed for fraud |
| User safety | Direct L1 exit path via the bridge |
| Final enforcement | Ethereum consensus |

---

## 6. Optimism Example
- **Type:** Optimistic Rollup (uses fraud proofs + 7-day challenge).  
- **Execution:** OpEVM (EVM-equivalent).  
- **Sequencer:** single operator (for now).  
- **Contract:** `OptimismPortal` on Ethereum handles proofs & bridges.  
→ Cheap, fast L2 secured entirely by Ethereum.

---

## 7. Canonical Bridge
Official L1↔L2 gateway built into the rollup contract.  
- **Deposit:** lock tokens on L1 → mint on L2.  
- **Withdraw:** burn on L2 → prove → unlock on L1.  
- Implements the fallback exit path.  
Funds never leave Ethereum custody → always recoverable.

---

## 8. How L2s Stay Secure
1. Ethereum stores all data (availability).  
2. Proofs verify computation (correctness).  
3. Bonds deter fraud (incentives).  
4. L1 fallback lets users withdraw anytime (safety).  
5. Consensus enforces contract logic (final trust).

---

## 9. The Rollup-Centric Future
Ethereum = **settlement + data layer**  
Rollups = **execution layers**  
“Do work off-chain, prove it on-chain.”  
Massive throughput, low fees, Ethereum-grade security.

---
