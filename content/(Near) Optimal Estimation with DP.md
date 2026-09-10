Ref: [[Optimal and DP Acquisition.pdf]]
Pre-req readings: [[Differential Privacy (DP)]] ; [[Laplacian Noise and DP]]
# Setting
We want to estimate the mean:  $\theta(P) = \mathbb{E}_{X \sim P}[X]$ from $n$ i.i.d. samples $X_1,\ldots,X_n$.
Each user $i$ has a heterogeneous privacy requirement $\epsilon_i$.

Assume, WLOG,
$\epsilon_1 \leq \epsilon_2 \leq \cdots \leq \epsilon_n \leq 1$

$|X| \leq \frac12$ almost surely.

The minimax estimation error is

$$
L_c(P,\theta,\epsilon)
=
\inf_{\hat{\theta}\in Q_c(\epsilon)}
\sup_{P\in\mathcal P}
\mathbb{E}\left[(\hat{\theta}-\theta(P))^2\right]
$$

where $Q_c(\epsilon)$ is the class of estimators satisfying heterogeneous central DP.
## Linear estimator with Laplace noise

The estimator considered is

$$
\hat{\theta}
=
\sum_{i=1}^n w_i X_i
+
\operatorname{Laplace}(1/\eta)
$$

with $\sum_{i=1}^n w_i = 1$.

The sensitivity with respect to user $i$ is $w_i$, because $|X_i|\leq 1/2$ implies that changing $X_i$ can change it by at most $1$. Therefore, by the Laplace mechanism,

$\hat{\theta}$ is $(w_i\eta)_{i=1}^n$-centrally differentially private. Hence, to guarantee the required privacy level $\epsilon_i$,
$$\eta w_i \leq \epsilon_i ~~ \forall i$$
# Theorem 1

There are universal constants $c_l,c_u>0$ such that

$$
L_c(\mathcal P^*,\theta,\epsilon)
\geq
c_l
\left(
\max_{k\in\{0,\ldots,n\}}
\frac{1}{n-k+\left(\sum_{i=1}^k\epsilon_i\right)^2}
\wedge 1
\right)
$$

and there exists an $\epsilon$-centrally DP linear estimator satisfying

$$
\mathbb E[(\hat{\theta}-\theta)^2]
\leq
c_u\log(n+1)
\max_{k}
\frac{1}{n-k+\left(\sum_{i=1}^k\epsilon_i\right)^2}
$$

So the linear estimator is optimal up to a logarithmic factor.

**Proof for lower bound - yet to be done**

## Upper Bound Proof
$$\hat{\theta}
=
\sum_i w_iX_i+\operatorname{Laplace}(1/\eta)$$

Its MSE is
$$\operatorname{MSE}
=
\sum_i w_i^2\operatorname{var}
+
\frac{2}{\eta^2}$$

because the data are independent and the Laplace noise has variance

$$\operatorname{Var}(\operatorname{Laplace}(1/\eta))
=
\frac{2}{\eta^2}$$
The problem is therefore to choose $w_i$ and $\eta$ while satisfying $\eta w_i\leq\epsilon_i$.

A straightforward choice is to make $\eta w_i=\epsilon_i$ for every user.
Then $w_i=\frac{\epsilon_i}{\eta}$ and, since $\sum_iw_i=1$, $\eta=\sum_i\epsilon_i$.

Therefore,
$$w_i
=
\frac{\epsilon_i}{\sum_j\epsilon_j}$$

This gives more weight to users with larger $\epsilon_i$ i.e. gives the least privacy-constrained users very large weights. That is disastrous for the statistical error.

Quite unintuitively, we cap the weights of the least privacy constrained users giving them more privacy to improve estimation.

### Construction
Define $k^*$ as the largest $k$ such that
$\epsilon_{n-k}>\frac{1}{\sqrt{k+1}}$.

The users with the largest privacy budgets,
$\epsilon_{n-k^*},\ldots,\epsilon_n$,

are exactly the users for whom simply setting $\eta w_i=\epsilon_i$ would make their weights too large.
So instead of using their full $\epsilon_i$, we give them the common effective privacy level

$\frac{1}{\sqrt{k^*+1}}$.

Thus the estimator is
$$\hat{\theta}
=
\sum_{i=1}^{n-k^*-1}
\frac{\epsilon_i}{\eta}X_i
+
\sum_{i=n-k^*}^{n}
\frac{1/\sqrt{k^*+1}}{\eta}X_i
+
\operatorname{Laplace}(1/\eta)$$
where
$$\eta
=
\sum_{i=1}^{n-k^*-1}\epsilon_i
+
\sqrt{k^*+1}$$For the first group, $\eta w_i=\epsilon_i$.
For the capped group, by the definition of $k^*$.
$$\eta w_i
=
\frac{1}{\sqrt{k^*+1}}
<
\epsilon_i$$
So the estimator satisfies the required heterogeneous privacy guarantees.
### A concrete example
Suppose
$$\epsilon_1=\cdots=\epsilon_{n-\sqrt n}
=\frac{1}{\sqrt n}$$
while
$$\epsilon_{n-\sqrt n+1}
=\cdots
=\epsilon_n=1$$
If every privacy constraint is tight, then
$$w_i
=
\frac{\epsilon_i}{\sum_j\epsilon_j}$$
Since $\sum_j\epsilon_j\approx2\sqrt n$, the last $\sqrt n$ users receive weight approximately $\frac{1}{2\sqrt n}$ each.
$$\sqrt n\left(\frac{1}{2\sqrt n}\right)^2
=
\Theta\left(\frac{1}{\sqrt n}\right)$$
This dominates the desired $O(1/n)$ statistical error.

Instead, if we use
$w_i=\frac1n$ for every user and choose $\eta=\sqrt n$.
$$\eta w_i
=
\frac{\sqrt n}{n}
=
\frac1{\sqrt n}$$
So **every user**, including the users with $\epsilon_i=1$, actually receives privacy loss only
$\frac1{\sqrt n}$. For the first $n-\sqrt n$ users this uses essentially their full privacy budget. For the last $\sqrt n$ users:
$\frac1{\sqrt n}<1$.

The estimator is
$$
\hat{\theta}
=
\sum_{i=1}^n \frac{1}{n}X_i
+
\operatorname{Laplace}\left(\frac{1}{\eta}\right).
$$
Since $\eta=\sqrt n$,
$$
\hat{\theta}
=
\frac{1}{n}\sum_{i=1}^n X_i
+
\operatorname{Laplace}\left(\frac{1}{\sqrt n}\right).
$$
Because the $X_i$ are i.i.d. with variance $\operatorname{var}$,
$$
\operatorname{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right)
=
\sum_{i=1}^n
\frac{1}{n^2}\operatorname{Var}(X_i).
$$
$$
=
n\frac{\operatorname{var}}{n^2}
=
\boxed{\frac{\operatorname{var}}{n}}.
$$
So the sampling-error contribution is
$$
O\left(\frac{1}{n}\right).
$$If
$$
Z\sim\operatorname{Laplace}\left(\frac{1}{\eta}\right),
$$
then
$$
\operatorname{Var}(Z)=\frac{2}{\eta^2}.
$$
Here $\eta=\sqrt n$, so
$$
\operatorname{Var}(Z)
=
\frac{2}{(\sqrt n)^2}
=
\boxed{\frac{2}{n}}.
$$
Thus the privacy-noise contribution is also
$$
O\left(\frac{1}{n}\right).
$$

The data and the Laplace noise are independent, hence
$$
\operatorname{Var}(\hat{\theta})
=
\operatorname{Var}\left(\frac{1}{n}\sum_{i=1}^n X_i\right)
+
\operatorname{Var}(Z).
$$
$$
\operatorname{Var}(\hat{\theta})
=
\frac{\operatorname{var}}{n}
+
\frac{2}{n}
=
\boxed{\frac{\operatorname{var}+2}{n}}.
$$
$$
\boxed{\operatorname{Var}(\hat{\theta})=O\left(\frac{1}{n}\right)}.
$$
$$
\underbrace{\frac{\operatorname{var}}{n}}_{\text{sampling variance}}
+
\underbrace{\frac{2}{n}}_{\text{Laplace noise}}
=
\boxed{O(1/n)}.
$$