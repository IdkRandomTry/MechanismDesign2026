# Central Theme

[[(Near) Optimal Estimation with DP]] treats privacy loss levels $\epsilon_i$ as **given**.

Here we try to design the platform which chooses how much privacy loss to impose on each user based on the user's privacy sensitivity.

The platform's objectives:
1. which users' data to use,
2. how much privacy loss $\epsilon_i$ each user receives,
3. how much to pay each user.

## Privacy sensitivity

Each user $i$ has a private type $c_i \in \mathbb{R}_+,$ called their **privacy sensitivity**. It represents the user's **cost per unit of privacy loss**.
- small $c_i$ → user is less sensitive to privacy loss,
- large $c_i$ → user is more sensitive to privacy loss.

The privacy sensitivities are independently drawn from publicly known distributions.
For each user $i$:
- $F_i(c)$ = CDF of privacy sensitivity,
- $f_i(c)$ = PDF of privacy sensitivity.

Let $c=(c_1,\ldots,c_n)$ denote the vector of all users' privacy sensitivities. The vector $c$ is **private information**: the platform knows the distribution of $c_i$, but does not know the actual $c_i$ of a user.
## Strategic (mis)reporting

Users participate by:
1. reporting their privacy sensitivity,
2. sharing their data.

**Assumptions:**
- users **cannot manipulate their data** - it can be collected or verified by the platform;
- users **can misreport their privacy sensitivity**.

In exchange for the user's data, the platform provides a **compensation/payment**.
## Private data acquisition mechanism
A private data acquisition mechanism is a tuple $(\hat{\theta},\epsilon,t).$
### Estimator
$$
\hat{\theta}:\mathcal{X}^n\times\mathbb{R}_+^n\rightarrow\mathbb{R}
$$

is a centrally or locally differentially private estimator.

It takes acquired user data $x=(x_i)_{i=1}^n$, privacy-loss vector $\epsilon=(\epsilon_i)_{i=1}^n$, and produces an estimate $\hat{\theta}(x,\epsilon).$
### Privacy-loss functions
For every user $i$, $\epsilon_i:\mathbb{R}_+^n\rightarrow\mathbb{R}_+$ maps the vector of privacy sensitivities to the privacy loss assigned to user $i$:
$$
\epsilon_i=\epsilon_i(c).
$$

Thus, the mechanism can give different users different privacy guarantees depending on the reported sensitivities of **all users**.
### Payment functions
For every user $i$, $t_i:\mathbb{R}_+^n\rightarrow\mathbb{R}_+$ maps the vector of privacy sensitivities to the payment received by user $i$:

$$
t_i=t_i(c).
$$

> [!Recap]
> The mechanism therefore specifies $$ (\hat{\theta},\epsilon(c),t(c)). $$
> 
> The functions are assumed to be differentiable, with Riemann-integrable derivatives [[Doubts]].

## User's cost

A participating user receives a benefit from a more accurate estimate, but incurs a privacy cost. For user $i$ with true privacy sensitivity $c_i$, the cost has two important components:
### Estimation cost
The mean square error of the platform's estimate:
$$
\operatorname{MSE}
=
\mathbb{E}_x
\left[
\left|\hat{\theta}(x,\epsilon)-\theta\right|^2
\right].
$$

### Privacy cost
The privacy loss $\epsilon_i$ costs the user $c_i\epsilon_i.$
### Payment
The user receives payment $t_i$, which reduces their overall cost.
## Misreporting
Suppose user $i$ has true type $c_i$, but reports $c_i'.$ Let $c_{-i}$ denote the privacy sensitivities of all other users. The user's expected cost is
$$
\operatorname{cost}(c_i',c_i;
\epsilon,t,\hat{\theta})
=
\mathbb{E}_{c_{-i}}
\left[
\operatorname{MSE}(c_i',c_{-i};\epsilon,\hat{\theta})
+
c_i\epsilon_i(c_{-i},c_i')
-
t_i(c_{-i},c_i')
\right].
$$

Here:
- $c_i'$ = what the user **reports**,
- $c_i$ = the user's **true** privacy sensitivity,
- $c_{-i}$ = other users' types,
- $\operatorname{MSE}$ = estimation error resulting from the reported types,
- $c_i\epsilon_i$ = true privacy cost, 
- $t_i$ = payment.
>[!Note]
>The privacy loss $\epsilon_i$ depends on the **reported** type. But the privacy cost is determined by the **true** type $\text{privacy cost}=c_i\times\epsilon_i$

## Non-participation
If user $i$ does not participate:
- her data is not used,
- she has no privacy loss,
- she receives no payment,
- she does not obtain the benefit of the platform's more accurate estimate.
Her best estimate of $\theta$ using only her own data is $X_i$.

Therefore,
$$
\mathbb{E}_{X_i}
\left[
|\hat{\theta}(X_i)-\theta|^2
\right]
=
\mathbb{E}_{X_i}
\left[
|X_i-\theta|^2
\right]
=
\operatorname{var}.
$$

Hence $\operatorname{var}$ is the user's opportunity cost. A variation of this to allow the user the benefit of the accurate estimate, but the paper claims it does not affect the proposed mechanism.
## Platform's objective
For a fixed estimator $\hat{\theta}$, the platform chooses $\epsilon_i(\cdot), t_i(\cdot)$  for every user to minimizes
$$
\mathbb{E}_c
\left[
\operatorname{MSE}(c,\epsilon,\hat{\theta})
+
\sum_{i=1}^n t_i(c)
\right]
$$

## Incentive compatibility (IC)
Because privacy sensitivity is private information, users might lie about it. The mechanism should make truthful reporting optimal. For every user $i$ and for every possible report $c_i'$.
$$
\operatorname{cost}(c_i,c_i;\epsilon,t,\hat{\theta})
\le
\operatorname{cost}(c_i',c_i;\epsilon,t,\hat{\theta})
$$
The equilibrium notion used is **Bayesian Nash equilibrium**, because users must optimize on the other users' unknown private types.
## Individual rationality (IR)
A user should not be worse off by participating. If the user does not participate, her benchmark cost is $\operatorname{var}$. Therefore: $\operatorname{cost}(c_i,c_i;\epsilon,t,\hat{\theta}) \le\operatorname{var}.$
## Recap
The platform's optimization problem is therefore
$$
\begin{aligned}
\min_{\epsilon(\cdot),\,t(\cdot)}\quad
& \mathbb{E}_c\left[
\operatorname{MSE}(c,\epsilon,\hat{\theta})
+ \sum_{i=1}^n t_i(c)
\right] \\[4pt]
\text{s.t.}\quad
& \operatorname{cost}(c_i,c_i;\epsilon,t,\hat{\theta})
\le
\operatorname{cost}(c_i',c_i;\epsilon,t,\hat{\theta})
\qquad [\mathrm{IC}]
\quad \forall i,c_i,c_i' \\[4pt]
& \operatorname{cost}(c_i,c_i;\epsilon,t,\hat{\theta})
\le \operatorname{var}
\qquad [\mathrm{IR}]
\quad \forall i,c_i
\end{aligned}
$$
