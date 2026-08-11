---
id: Lecture6-DeFi
aliases: []
tags: []
---


### Web 3

"Ecosystem of decentralised, smart-contract based applications that can replace centralised intermediaries to allow users to interact in a permisionless and trustless way" 

Decentralised financial applications are a key focus of Web3 applications.

DeFi needs the following properties:
	1. Non-custodial : i.e no banks or centralised holdings of funds. Every user has immediate access to their funds.
	2. Permisionless : Anyone can make financial plays without censorship or control of a third party.
	3.  Openly-Auditable: Anyone can inspect the global state of any transaction.
	4. Composable: Multiple financial operations should be able to be combined to make larger more complex financial instruments/ processes.

DeFi applications need some protocols/ features to ensure the above properties:
	1. Blockchain primitives. This is smart contracts etc. that ensure that transactions can be verified to be completed in order and accurately before being chained to another.
	2. Keepers: Off-chain agents to trigger on-chain actions.
	3. Governance: A management of these applications (multiple)
	4. Oracles: A means of transporting off chain information reliably to be used on-chain. This includes verification of the off-chain information.

#### Oracles

- Most simple oracle would be some means of knowing how much ETH is worth in terms of USD which can be found from aggregating multiple price feeds from markets off-chain. This price would have to be accurate and honest to be used by on-chain nodes for transactions/ smart contract calls.
- There are multiple oracle architectures: centralised, multi-sig (which sounds good but there is opportunity for collusion) and finally data-aggregator oracle networks. These networks also use some PoS and verification scheme between nodes to ensure honest prices. 

#### Protocols for Loanable Funds (PLFs)

- A critical application needed for a DeFi. The basic concepts of pooling loanable funds from savers and distributing them to borrowers at an interest rate. All managed by smart contracts.
- Key facilitator is **Collateral***. All loans are overcollatoralized i.e the borrower must pledge an amount *greater* than what they are borrowing. This is what ensures the appropriate payback of loans, as not doing so would result in loss of collateral worth potentially more than what they borrowed.
- Despite this "drawback" of overcollateralization there are benefits:
	1. Some borrowable assets may have temporary perks like voting rights.
	2. Long and short trading opportunities.
- The collateral itself can potentially equal 0 due to how volatile the currency of the collateral may be. There are defence mechanisms in place to protect the lender in these situations, as "liquidators" can step in and pay the loan back on behalf of the borrower and recieve the collateral posted at a hefty discount.
  The borrower keeps all of the money that they borrowed but obviously lose the collateral that they posted. The liquidator pays the loan but recieves the liquidation bonus which is the cost of the loan + 10%. This amount is subtracted from the original collateral posted from the borrower. 

#### Flash loans

- Very interesting application of undercollateralized loans. The basic principle is that a user must borrow and repay the loan within the same transaction. This can be guarenteed due to the atomicity of transactions, so if this final condition of the repayment is not met, then the whole transaction will revert.
- A common use case for this could be a collateral swap operation as indicated on slide 37.
- Flash loans give access to large amounts of capital which is a good thing to improve overall market efficiency & liquidity however it can be used nefariously to drive up indirect prices using this access to large capital.

#### Stablecoins

- "*A crypto-currency with an additional economic structure to stabilise purchasing power and price*"
- 3 main types of stable-coin:
	1. Custodial: Reintroduction of trust of third party to manage the collateral associated with the coin. Custodian will control real asset X and issue on-chain coins pegged against the value of asset X.
	2. Central Bank Digital Currencies: Same thing but government issued coins. Equivalent to <mark style="background: #ADCCFFA6;">fiat currency???</mark>.
	3. Non-custodian stablecoin....
	
