# High level overview
We need to accept money from players, identify a winner and send them money. We’re sensitive to fees & time, not privacy.

## The input problem
Need a way for 100 different customers to deposit funds into different “things” such that I can tell who deposited how much and when.

### Possible solutions: 

#### Accounts
	* Generate a different account for each customer. Monitor it’s balance.
		* Question: I’m overall pretty unclear on what an account is concretely. I’ve come across 2 uses of the word account:
			* [Wiki Accounts](https://en.bitcoin.it/wiki/Help:Accounts_explained)
			* [bip32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
			* I’m pretty sure these are separate concepts considering the one in the wiki is deprecated and non-HD (difficulty with backups).
			* In either case how does crafting a transaction across these entities work? You grab a bunch of UTXO’s across different addresses, do you have to sign the transaction several times? Or each UTXO?
			* Is there anything that stops me from crafting a transaction with UTXO’s from different accounts?

#### 1 Wallet 
	* Generate 1 HD wallet and then just use subsequent addresses to identify each customer. 
		* Seems inherently simpler, and makes it so I don’t need to know the answer to those questions. 

## The output problem 
Once the winner is identified need a way to send them money. 

### Possible solutions:

#### Batching
	* 100 players, 100 transactions
		* Can save on fees through batching
		* Question: *what if one of the inputs is unconfirmed?* Does that block the whole transaction? 
		
#### Many inputs 1 output
	* Go across accounts / addresses and grab all the UXTO’s the player’s have sent. Craft a transaction with 1 output (winner). 
	* Problems:
		* *What if one of the inputs is unconfirmed?* transaction will be stuck in mempool. 
		* Solution:
			* Add a rule to only select confirmed UTXO’s
				* Problem: delays due to unavailability of confirmed UTXO’s 
					* Solution: Always have some number of confirmed UTXO’s (buffer). 
						* Problem: Presents financial risk
		* Solution:
			* ~~Detect when a transaction meets a certain criteria and [Replace by fee](https://en.bitcoin.it/wiki/Replace_by_fee)~~ This is actually not possible since you're not allowed to replace inputs during RBF
			* If we have not detected a double spend upon win send time, and someone has a really low fee, we can [CPFP](https://bitcoinelectrum.com/how-to-do-a-manual-child-pays-for-parent-transaction/)
			* If someone has a very low fee should we not allow them to enter matchmaker until 1 confirmation? How do we determine what fees are risky? *Will this incentivize our transaction over the double spend attempt? Assuming yes. And therefore:* 
				* [Search · child pays for parent · GitHub](https://github.com/bcoin-org/bcoin/search?q=child+pays+for+parent&unscoped_q=child+pays+for+parent) sad. How does this work? 

Side mini questions:
	* So what exactly happens in the following *scenario 1*:
		* transaction created with unconfirmed inputs, and broadcasted.
		* 1 of those inputs was double spent post broad cast?  
	* It doesn’t really matter what wallet or address a transaction came from, right? You just take the UTXO’s and sign them with the various receiving address’ private key’s and craft a transaction composed of those, right? 


# Intended course of action [RFC]:

## Before Launch
On joining:
	* Proceed user immediately

During matchmaker and game
	* Subscribe to double spend event detection 

After winner announced:
	* Use unconfirmed inputs, detect if mini question 1 scenario 1 occurs. Fix the transaction (need more clarity on scenario 1, and how we can fix the transaction).  - keep status page updated. 
	* If it’s not trivial to “fix a transaction”:
		* Wait for 1-2 confirmations - keep status page updated
		* Send alert to increase the size of the buffer
		* 

## After Launch 
On joining:
	* When user joins if fee is very low make them wait for 1 confirmation.
	* If fee is normal, proceed immediately

During matchmaker and game:
	* Subscribe to double spend event detection.
	* Remove any detected double spend attempts

After winner announced:
	* CPFP to incentivize these unconfirmed transactions

#bitcoin