---
id: tutorial3
aliases: []
tags: []
---


# OverCollateralisation


A protocol MakerCow allows users to borrow units of the stablecoin Zoro (pegged to the USD) provided users
overcollateralise the borrowed amount. In particular, the protocol enforces that for every loan of Zoro tokens,
there is at least 150% of the value in Ether (the liquidation ratio). If there is less than 150% of the value of
Zoro tokens in Ether (ETH), then the deposited ETH collateral is seized and auctioned off, with buyers paying
for the collateral with Zoro tokens.

1. Alice has 4 ETH, and when she takes out the loan, the market price of ETH is 1 806 USD. What is the
maximum number of Zoro tokens Alice can mint?

4 / 1.5 = 2.6666
2.6666 * $1806 = 4203 Zoro Tokens.

2. What are the risks associated with her minting the maximum number of Zoro tokens with the 4 ETH?

The protocol enforces the 150% OverCollateralisation ratio, so by pledging all her ETH, if the price of Eth was so fluctuate and drop below this threshold, she is opening herself up to having all her ETH seized. Eth is generally quite volatile, so this is not a good strategy. 

3. After minting Zoro stablecoin tokens, Alice trades them elsewhere for ETH and uses this new ETH to
top up her vault and mint more Zoro tokens. She does this the maximum number of times to increase
her exposure to ETH. What is her maximum theoretical exposure in ETH? How many Zoro tokens would
have been minted?



4. Instead, Alice opts for a less risky position, borrowing 3600 Zoro against her 4 ETH of collateral. What
is the liquidation price for the vault now, assuming the liquidation ratio remains 150%

Price of ETH = 1806 USD/Zoro. 

Borrowing 3600 Zoro Equivalent to 3600 / 1806 ETH = 1.9936 ~ 2.
150% minimum collateral so 3 ETH minimum but 4 deposited.

4 / 1.5 = 2.666. 

3600 / 2.666 = 1350.

Price of ETH must drop to $1350

2 Automated Market Makers (AMMs)

2.1 Prices and invariants

The formula for a Constant Product Market Maker (CPMM), a type of AMM, is k = xy. Assume Bob deposits
250 units of asset x into the CPMM and 10 units of asset y.

1. What is the price of asset x in terms of asset y?

x = 1/25 y

2. If Bob now wants to exchange 10 units of asset x for asset y, how many units of asset y will he receive
and what will be the new price of asset x in terms of asset y?

k = 250 * 10 = 2500.

If 10 more of x have been deposited, then x balance = 260, y balance is now:
2500 / 260 = 9.61538461538. The y he will receive is 10 - 9.61538461538 = 0.38461538462. 

The new price of x in terms of y = 9.61538461538 / 260

2.2 Impermanent loss

Impermanent loss is the difference in value resulting from holding tokens A and B directly in a wallet vs providing
them as liquidity in an AMM, where the difference arises from price changes.
Assume there exists a CPMM with two tokens A and B. The pool is a 50:50 pool, such that the total dollar
value of token A in the pool should be equal to the dollar value of token B. Assume at time t = 1, the price of
token A is 1200 USD. It appreciates 20% in value by t = 2. Assume token B has an unchanging price of 400
USD.

1. Assume you start with 1000 USD of each token at t = 1 and you do not supply these to the AMM. What
would your balance in USD be at the end of period t = 2?


2. Assume you start with the same 1000 USD of each token at t = 1 and you do supply these to the AMM.
What is the invariant value, k, at t = 1? What about at t = 2?


3. Given the new prices at t = 2, what are the new values of A and B? What is this in USD?


4. What is the impermanent loss in percent if you deposit the tokens to the pool?


3 Liquidation Calculations

Alice deposits 1 000 DAI into a PLF. The price of DAI is $1. The price of ETH is $1 250. The liquidation
threshold is 0.7.

1. What is the maximum amount of ETH that Alice can borrow against her deposited funds without getting
liquidated?

1000 / 1250 = 0.8 ETH is the fair price. 

0.7 Liquidation value, i.e loan to value ratio, means its 0.8 * 0.7 = 0.56 ETH.

2. Under what circumstances would Alice get liquidated?

This is either if the value of ETH rises with respect to DAI collateral. Likewise, if the value of DAI falls with respect to the borrowed asset.


Alice wants to ensure that she has a sufficient amount of collateral and deposits 100 UNI tokens into the same
PLF. The price of UNI is $6. The liquidation threshold remains the same at 0.7. Alice borrows 0.68 ETH in
total. The price of ETH remains the same.

1. What is Alice’s new health factor? Keep in mind that she has two assets deposited, which would be used
as collateral.

LT is still 0.7.
She has chosen to collateralize $600 + $1000 = $1600 of collateral. 
0.68 ETH = 0.68 * $1250 = $850 of ETH.

She can borrow up to $850 / 0.7 of ETH at most.

Health factor = $1600 / ($850 / 0.7) = 1.3176


2. How much more ETH could she borrow before getting liquidated?

($1600 / $1250) = VALUE OF ETH COLLATERALISED.
VALUE OF ETH COLLATERALISED * 0.7 = Maximum safe amount.
Maximum safe amount - 0.68 = 0.216

Assume that Alice continues to borrow 0.68 ETH and that the price of ETH appreciates to $1 700. The
liquidation penalty is 5%.

1. What is the new health factor?

$1700 / $1600 = 0.94117647
0.94117647 * 0.7 = 0.65882353

Health factor = 0.65882353 / 0.68 = 0.9689

2. If Alice gets liquidated at that price, what will be the value (USD) of the collateral she has left afterwards?

A health factor < 1 opens Alice up to getting liquidated. This will always happen as liquidators walk away with a risk free profit. The Liquidator comes in and pays off the debt that Alice owes, which in this case is:

0.68 * $1700 = $1156

The liquidators are rewarded by being paid $1.05 for every $1 of debt they pay off, with this all coming from the collateral.  

Liquidators paid: $1156 * 1.05 = $1213.80

This comes out of Alice's collateral, so she now has: $1600 - $1213.80 = $386.20 remaining.


3. Assume that there are multiple liquidators that try to liquidate Alice’s position, and therefore several
liquidation transactions get included in the next block. Assuming that a single transaction liquidates the
entire position, which transaction will be the one to succeed (liquidate the position)? What are the costs
to the liquidators whose transactions did not succeed yet were included in the block?

The first transaction to execute will obviously be the one that completes first. All of the other transactions will fail since the loan has already been repaid, so there is nothing to do. The other transactions that attempt this liquidation risk their gas fees and transaction fees going to waste. Bots will engage in "priority gas auction" with some kind of arbritage as to the prices to offer so that the transaction is still profitable.

4 Flash Loans

Assume that Bob sees that there is an arbitrage opportunity: he can purchase token A on one exchange and
then sell it for a higher price on the second exchange.

1. How could this be a risk-free transaction for Bob?

By utilising flash loans and the fact that all transactions in Ethereum must be completed, he could package the flash loan and purchase of A on one exchange and sale on the other all in one transaction. If there was any change in price, he could allow the transaction to fail so he would not lose anything.

2. If someone noticed Bob’s pending transaction while it is still in the mempool (i.e., not confirmed/included
in a block), what would need to happen so that Bob’s transaction is no longer profitable by the time it
gets included in a block? (Hint: think about how transactions are processed)

Someone could manipulate the price on the first exchange to drive the price of token A really high so that it is higher than the second exchange and at that point the price on the second exchange may be lower so Bob would make a loss. 

3. Why are flash loans not feasible in traditional financial systems (the “real” world)?

The validity of a flash loan is tied into the transactions being atomic- this means that everything must complete in one go. This sort of guarentee is obviously not feasible in real markets. 

4. At a high level, describe how you would implement a smart contract that provides/issues a flash loan (i.e.,
is the lender). How would you ensure that a flash loan will be repaid at the end of a transaction?

The flash loan lender would be able to provide a huge amount of liquidity, so first it would need to check if its balance has the resources to fulfil the request. If this is the case, then:
  
  It would supply the requested funds to the contract caller, and then through some mechanism, maybe before the block has been validated or through some kind of time-out sequence, check to see if the balance on the contract is the borrowed amount + some small lending fee.

  This could perhaps be done by exposing a function signature like: borrowFlashLoan(uint256 amount, uint256 token, void* ExecuteLogic())

When the caller calls the borrowFlashLoan function they pass their ExecuteLogic function as a parameter, and this is where the check can be done before and after that execution. 

COMPOSABILITY :)




