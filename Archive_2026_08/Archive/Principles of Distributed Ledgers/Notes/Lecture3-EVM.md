

# 🧠 Ethereum Virtual Machine (EVM) — Summary Notes

---

## 1. Ethereum Overview
- Ethereum is a **global state machine** where blocks represent **state transitions** caused by transactions.
- Each node runs the **EVM** to verify these transitions deterministically.

---

## 2. Accounts
| Type | Controlled By | Has Code? | Can Send TXs? |
|------|----------------|------------|----------------|
| **EOA** | Private key | No | Yes |
| **Contract** | On-chain bytecode | Yes | No |

Each account stores: `nonce`, `balance`, `storageRoot`, `codeHash`.

---

## 3. The EVM
- A **stack-based virtual machine** that executes contract bytecode.
- Components:
  - **Stack:** computation
  - **Memory:** temporary data
  - **Storage:** persistent contract data
  - **Calldata:** input arguments
  - **Gas:** execution cost tracker
- Execution is deterministic and identical on all nodes.

---

## 4. Transaction Flow
1. EOA sends a signed transaction.
2. Validators include it in a block.
3. EVM loads target contract’s bytecode from world state.
4. Reads calldata → first 4 bytes = **function selector**.
5. Jumps to the matching function and executes.
6. Updates balances/storage → new global state.

---

## 5. Function Selectors & ABI
- ABI: Application Binary Interface.
- Selector = first 4 bytes of `keccak256("functionName(types)")`.
- Transaction data = `[selector | encoded arguments]`.
- Contract’s **dispatcher** matches selector → jumps to correct code.
- If no match → `fallback()` or `receive()` runs.
- Works identically for contract-to-contract calls.

---

## 6. Function & Contract Calls
| Call Type | Uses Selector? | Description |
|------------|----------------|-------------|
| **Internal** | No | Jump within same bytecode |
| **External (CALL)** | Yes | Calls another contract |
| **Delegatecall** | Yes | Runs foreign code in caller’s storage |
| **Staticcall** | Yes | Read-only call |

Each call has its own gas, memory, and context.

---

## 7. Gas
- Each opcode has a cost; gas prevents infinite loops.
- Unused gas refunded; if it runs out → revert.
- **Base fee** burned, **tip** goes to validator.

---

## 8. Message Calls & Atomicity
- Contract-to-contract execution (via `CALL`).
- All calls form a single atomic chain.
- If any fail → full revert (no partial state).

---

## 9. Data Regions
| Area | Persistent | Purpose |
|-------|-------------|----------|
| **Storage** | ✅ | Contract state |
| **Memory** | ❌ | Temporary data |
| **Calldata** | ❌ | Function input (read-only) |

---

## 10. Deployment vs Execution
| Stage | Description |
|--------|--------------|
| **Deployment** | Uploads compiled bytecode → new address |
| **Execution** | Loads existing bytecode and runs it |

---

## 11. Consensus Layer
- **Proof-of-Stake** decides who proposes/attests blocks.
- **EVM execution** defines valid state updates.
- All validators re-run transactions → identical result → consensus.

---

## 12. Core Properties
- **Deterministic:** same input = same result.
- **Atomic:** full success or revert.
- **Isolated:** no external data.
- **Transparent:** code/state are public.
- **Composable:** contracts can interact freely.
- **Gas-limited:** bounded computation.

---

## 13. Quick Flow Diagram
