---
id: coursework_summary
aliases: []
tags: []
---

# HumanResources Coursework — Full Conceptual Overview

## 1. EOAs, Wallets, and Smart Contracts

### EOAs
Externally Owned Accounts controlled by private keys. Can initiate transactions, pay gas, hold tokens, and call contracts.

### Smart Contracts
Programs deployed at addresses with code+storage. Cannot initiate transactions; execute only when called by EOAs or other contracts.

## 2. Deployment & HR Manager

Constructor sets immutable HR manager. Your deployment uses your wallet; graders deploy fresh instances in their tests with their own HR manager addresses.

## 3. Access Control

`onlyHR` verifies `msg.sender == _hrManager`. Employees recognized via `employees[msg.sender].exists`.

## 4. External Contracts

USDC, WETH, Uniswap Router, Chainlink Feed are real contracts on Optimism. Balances are kept in each token contract’s storage keyed by address.

## 5. Interactions

Your contract coordinates external calls: USDC transfers, Chainlink price reads, Uniswap swaps, WETH unwrap, ETH transfers.

## 6. Funding

HR manager must send USDC to the contract manually. In tests, funding is simulated with cheatcodes.

## 7. Salary Accrual

Track USD amounts in 1e18. Accrue linearly between timestamps using weekly salary / seconds per week.

## 8. salaryAvailable Output

Return amounts scaled to currency decimals:
- USDC: 6 decimals (usd1e18 / 1e12)
- ETH: 18 decimals via Chainlink conversion.

## 9. Chainlink

Provides ETH/USD price with 8 decimals, used for conversion and slippage checks.

## 10. Uniswap

Used for USDC->WETH swaps. Fee tier selects the pool. Enforce slippage with `amountOutMinimum`.

## 11. Fork Testing

Local simulation of Optimism state. Allows testing real interactions without spending gas.

## 12. Two Deployments

Your real deployment for submission; graders re-deploy your code for automated tests.

## 13. Withdraw Flow

1. `_updateAccrued`
2. USDC mode: convert USD->USDC, `USDC.transfer`
3. ETH mode: convert USD->ETH (oracle), swap USDC->WETH, unwrap, send ETH.

## 14. What the Contract Holds

Internal state only. Token balances tracked inside external token contracts. ETH balance tracked natively.

## 15. What Graders Test

Access control, accrual math, conversion correctness, slippage rules, errors, events, correct addresses, safe ETH transfer, and deployment info.
