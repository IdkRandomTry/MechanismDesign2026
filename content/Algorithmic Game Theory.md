Algorithmic Game Theory (AGT) lies at the intersection of Theoretical Computer Science and Economics. 

> [!Differs from Algorithm Design]
> Traditional Algorithm Design assumes a centralized system where a single program dictates all actions. AGT models decentralized systems with self-interested, strategic participants (e.g., Auctions, routing, blockchains).

# Selfish may be suboptimal
## Braess's Paradox

![[Pasted image 20260831145400.png]]

Before adding a new road:
Traffic splits evenly to minimize delay. 
- $1/2$ goes $s \to v \to t$. Cost = $1/2 + 1 = 1.5$.
- $1/2$ goes $s \to w \to t$. Cost = $1 + 1/2 = 1.5$.
The Nash Equilibrium delay is $1.5$.

After adding a new road
Suppose civil engineers build an incredibly fast edge $(v,w)$ with $c(x) = 0$.
Now, the path $s \to v \to w \to t$ has cost $x + 0 + x = 2x$.
- As individuals selfishly minimize their own travel time, they all switch to the new path because at any intermediate state, going through $(v,w)$ appears faster.
- At the new Nash Equilibrium, *all* traffic takes $s \to v \to w \to t$.
- New delay = $1 + 0 + 1 = 2$.

*Adding a zero-cost edge increased the delay for everyone! Selfish behavior may leads to suboptimal outcomes.*

### Price of Anarchy (PoA)
PoA measures the degradation of system efficiency due to selfish behavior.
$$ \text{PoA} = \frac{\text{Cost of Nash Equilibrium}}{\text{Cost of Social Optimum}} $$
- In the Braess's Paradox example, the PoA is $\frac{2}{1.5} = \frac{4}{3}$.
- PoA of 1 is ideal as selfish behavior leads to optimal performance.