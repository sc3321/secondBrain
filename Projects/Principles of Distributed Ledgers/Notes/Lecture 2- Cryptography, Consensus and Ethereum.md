
Main point here is the public <---> private <---> signature loop.

- Private key is the main agent. This is where everything is derived from. A private key corresponds to an address. The owner of the private key owns access for the funds in that address.
- This is where Cryptography enters the picture:
	- A BTC private key is a random number between 1 - $2^{256}$ 
	- The UTXO transaction message (<mark style="background: #FF5582A6;">contents and header?</mark>) is hashed ->> H
		- H is now combined with the private key using some very computationally difficult cryptography protocols to generate a **signature** which is sent **alongside** the message. This combination is not trivial and is specific to the private key and H values. A slightly different H or private key will result in a different output at this stage.
		- Transaction composed of: message, signature, public key.
		- Now at the verification stage, every miner will look at a given transaction Tn.
		  Using again some cryptographic mechanism they will do some check to do a comparison of the hashed message against the signature. This check- the details of which are too complex effectively asks the question, did the signature and hash originate from the same private key that this public key was also derived from. If yes, then happy days. And since the private key controls access to the funds, only someone with a private key can send a valid signature to authorise access and transactions from a specific address.