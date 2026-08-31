> [!What is an Auction]
> An auction is a mechanism to allocate a set of goods to a set of bidders on the basis of bids announced by the bidders. Apart from traditional setting, Auctions are now used in advertising space, search engine keywords, etc.
# Terminologies
**Resources:** The entities for which auctions are conducted
**Market Structure:** The dynamics between buyers and seller. Mainly classified as:
	*Forward Auction -* multiple buyers and 1 seller
	*Reverse Auction -* 1 buyer and multiple sellers
	*Double Auction/Exchanges -* multiple buyers and sellers
**Preference Structure:** Agents affinity to items and quantities.
**Bid Structure:** What all is included in the bid - price; price and quantity; etc
**Winner Determination:** Optimal choice of allocation of resources for maximum utility
**Information Feedback:** This is optional for auction. Mainly seen in multi-round auctions where agents can revise their bids based on information of the previous rounds.

**Solution Equilibria:** The solution of a mechanism is in equilibrium, if no agent wishes to change its bid, given the information it has about other agents.
**Incentive Compatibility:** An auction is said to be incentive compatible if the agents optimize their expected utilities by bidding their true valuations of the resources.
**Allocative Efficiency:** Allocative efficiency is achieved when the social utility (sum of utilities) of all the winners is maximized.
**Individual Rationality:** Every agent gains a nonnegative utility by participating in the mechanism.
**Budget Balance:** Budget balance ensures that the auctioneer or mechanism designer does not make losses. An auction is said to be weakly budget balanced if, in all feasible outcomes, the payments by buyers exceed or equal to the receipts of sellers. An auction is said to be strongly budget balanced if the net monetary transfer is zero. 
**Revenue Maximization:** Maximizing surplus (on both side - buyer and seller)
**Fairness:** There should be fairness especially when there are multiple optimal solutions.
**Cheat-proofness:** Bidders should not be able to manipulate the auction.

# Single Indivisible Item Auction
## Abstraction
- Seller - wishes to sell an indivisible object to 1 of $n$ buyers
- Buyers - $\{1...n\}$
- Buyer $i$ has a private valuation $v_i$ (valuation vector $V = \{v_1 ... v_n\}$ ). 
- At the start of auction, the buyers report their $v_i$. However, they may strategically misreport their valuation if it results in a better expected payoff. Lets call this value $\hat{v_i}$ and the corresponding vector $\hat{V}$ . $\hat{V}$ is announced (made public) once all buyers report their type.
- The mechanism defines 2 rules
	- Allocation Rule: Defines the probability of agent $i$ getting the object. It is denoted by $x_i(\hat{V})$
	- Payment Rule: Defined how much do the winning agent(s) pay. It is denoted by $p_i(\hat{V})$
- Expected Payoff / Utility - In a Linear Environment the expected payoff is $x_i(\hat{V}) * v_i - p_i(\hat{V})$ 
### Dominant Strategy Incentive Compatibility (DSIC)
A mechanism is **DSIC** (or truthful) if for every bidder $i$, revealing their true valuation $b_i = v_i$ maximizes their utility, *no matter what the other bidders do*.
$$ u_i(\hat{v_i}, V_{-i}) \ge u_i(\hat{v_i}', V_{-i}) \quad \forall \hat{v_i}', \forall V_{-i} $$

## Types of Auctions
1. **First-Price Auction**
   - Highest bidder wins, pays their bid $\hat{v_i}$.
   - *Not DSIC*: If you bid your true value, your utility is $v_i - v_i = 0$. You are incentivized to **shade** your bid ($\hat{v_i} < v_i$) to get positive utility. Bidding strategy depends heavily on assumptions about others.

- **Second-Price (Vickrey) Auction**
	- Highest bidder wins, pays the **second-highest bid**.
	- The Vickrey auction is DSIC.
		- *Proof Intuition*: Your bid only determines *if* you win, not *what* you pay. Check out https://youtu.be/9qZwchMuslk?si=Wjg8qO2xS3p7Fv-h for good intuition.
		- If you overbid ($b_i > v_i$), you risk winning against someone with a bid higher than $v_i$, forcing you to pay more than the item is worth to you (yielding negative utility). 
		- If you underbid ($b_i < v_i$), you risk losing an item you could have won at a profitable price.

The Vickrey auction also maximizes total social surplus $\sum v_i x_i$ 

Check out [[Myerson's Lemma]] to find how to design optimal auctions.
