---
id: tutorial4
aliases: []
tags: []
---

Q1:
    
  The key bug is in the withdraw() function where balances[msg.sender] is only updated after the call function. In ETH these "call" functions can do one of either recieve() or fallback(). Either of these functions can call back into withdraw of the simpleContract again and make a withdrawal request. Since the balance for the msg.sender has not been updated, this is valid according to withdraw(). It can keep doing this recursively and drain the simpleContract reserve. The attacker can do this to extract more than their fair share from the simpleContract reserve, potentially until the entire reserve is empty. Only at the end of this recursive sequence is there any check completed by which point it is too late. 

Q2:
      
  1. The lottery contract instantiates a round with a given round-duration. This contract decides the length of each round of the lottery with its constructor. After it does this, for the round-duration period, lets say x days, different contracts can deposit an amount to the contract. The contract will maintain a mapping of every sender and the amount they have deposited. For every deposit, this amount is incremented to the global prize pool and the sender is also added to an array called participants[]. At the end of the round duration, the contract has the "reward distrbution perdiod" which is 2 weeks in this case, to pick a winner. It does this by randomly selected an index in the participants array based on a kekkac hash. The winner is then awarded the entire prize pot and every balance is then set to 0 for the round to be reset. If the winner is not selected in the 2 week reward distrbution period, then contracts can ask for a refund and all of the deposited amounts are returned back and the round is reset.

  2. Bugs in the code:
    -  
