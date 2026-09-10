# Single-Parameter Environments
Auctions where each bidder has a single private value for a unit of service, but we can serve multiple people at varying levels.
- $n$ bidders.
- Bidder $i$ has private valuation $v_i$ for receiving service.
- Feasible sets of allocations $X$ (e.g., in a $k$-item auction, $\sum x_i \le k$).
# Myerson's Lemma 
**Theorem:**
Consider a single-parameter environment.
1. An allocation rule $\mathbf{x}$ is implementable (i.e., there exists a payment rule $\mathbf{p}$ making the mechanism DSIC) **if and only if** $\mathbf{x}$ is **monotone**.
   - *Monotonicity*: If bidder $i$ increases their bid $\hat{v}_i$ (keeping other bids $b_{-i}$ fixed), their probability/amount of allocation $x_i(\hat{v}_i, \hat{V}_{-i})$ cannot decrease.
1. If $\mathbf{x}$ is monotone, then there is a **unique** payment rule $\mathbf{p}$ such that the mechanism is DSIC (assuming normalized payments where $p_i(0) = 0$).
2. This unique payment rule is given by the explicit formula:

   $$ p_i(\hat{v}_i, \hat{V}_{-i}) = \hat{v}_i \cdot x_i(\hat{v}_i, \hat{V}_{-i}) - \int_0^{b_i} x_i(z, \hat{V}_{-i}) \, dz $$

### Intuition for the Payment Formula
![[Pasted image 20260831152506.png]]
Think of the allocation function $x_i(z)$ plotted on a graph against the bid $z$.
- Because $x_i$ is monotone, the curve is non-decreasing.
- The term $b_i \cdot x_i(b_i)$ is the area of the bounding box/rectangle from $(0,0)$ to $(b_i, x_i(b_i))$.
- The integral $\int_0^{b_i} x_i(z) \, dz$ is the area strictly **under** the allocation curve.
- Therefore, the payment $p_i$ is the area **above** the curve and strictly to the left of the bid $b_i$.
- Bidder $i$'s utility ($v_i x_i - p_i$) is exactly the area **under** the curve.
