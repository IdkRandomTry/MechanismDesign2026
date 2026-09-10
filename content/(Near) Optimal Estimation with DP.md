Ref: [[Optimal and DP Acquisition.pdf]]
Pre-req readings: [[Differential Privacy (DP)]] ; [[Laplacian Noise and DP]]
# Setting
We want to estimate the mean:  $\theta(P) = \mathbb{E}_{X \sim P}[X]$ from $n$ i.i.d. samples $X_1,\ldots,X_n$.
Each user $i$ has a heterogeneous privacy requirement $\epsilon_i$.
## Central DP
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
$$
\hat{\theta}
=
\sum_i w_iX_i+\operatorname{Laplace}(1/\eta)
$$
Its MSE is
$$
\operatorname{MSE}
=
\sum_i w_i^2\operatorname{var}
+
\frac{2}{\eta^2}
$$
because the data are independent and the Laplace noise has variance

$$
\operatorname{Var}(\operatorname{Laplace}(1/\eta))
=
\frac{2}{\eta^2}
$$
The problem is therefore to choose $w_i$ and $\eta$ while satisfying $\eta w_i\leq\epsilon_i$.

A straightforward choice is to make $\eta w_i=\epsilon_i$ for every user.
Then $w_i=\frac{\epsilon_i}{\eta}$ and, since $\sum_iw_i=1$, $\eta=\sum_i\epsilon_i$.

Therefore,
$$
w_i
=
\frac{\epsilon_i}{\sum_j\epsilon_j}
$$

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
$$
\hat{\theta}
=
\sum_{i=1}^{n-k^*-1}
\frac{\epsilon_i}{\eta}X_i
+
\sum_{i=n-k^*}^{n}
\frac{1/\sqrt{k^*+1}}{\eta}X_i
+
\operatorname{Laplace}(1/\eta)
$$
where
$$

\eta=\sum_{i=1}^{n-k^* - 1}\epsilon_i +\sqrt{k^* + 1}
$$For the first group, $\eta w_i=\epsilon_i$.
For the capped group, by the definition of $k^*$.
$$
\eta w_i
=
\frac{1}{\sqrt{k^*+1}}
<
\epsilon_i
$$
So the estimator satisfies the required heterogeneous privacy guarantees.
### A concrete example
Suppose
$$
\epsilon_1=\cdots=\epsilon_{n-\sqrt n}
=\frac{1}{\sqrt n}
$$
while
$$
\epsilon_{n-\sqrt n+1}
=\cdots
=\epsilon_n=1
$$
If every privacy constraint is tight, then
$$
w_i
=
\frac{\epsilon_i}{\sum_j\epsilon_j}
$$
Since $\sum_j\epsilon_j\approx2\sqrt n$, the last $\sqrt n$ users receive weight approximately $\frac{1}{2\sqrt n}$ each.
$$
\sqrt n\left(\frac{1}{2\sqrt n}\right)^2
=
\Theta\left(\frac{1}{\sqrt n}\right)
$$
This dominates the desired $O(1/n)$ statistical error.

Instead, if we use
$w_i=\frac1n$ for every user and choose $\eta=\sqrt n$.
$$
\eta w_i
=
\frac{\sqrt n}{n}
=
\frac1{\sqrt n}
$$
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

## Local Setting
In the **local privacy** setting, users do not give their raw data $X_i$ to the platform. Instead, each user first privatizes their own data:
$$
\hat X_i := C_i(X_i)
$$
where $C_i$ is an $\epsilon_i$-LDP channel. The platform only observes
$$
\hat X_1,\ldots,\hat X_n.
$$
This differs from the central setting in Section 2.2:
- **Central DP:** platform sees $X_i$ and adds noise to the final estimator.
- **Local DP:** each user adds noise before sending their data.
An important consequence is:
> The privacy guarantee is enforced at the user level, before the data reaches the platform.

Also, an $(\epsilon_i)_{i=1}^n$-LDP algorithm is automatically $(\epsilon_i)_{i=1}^n$-central DP.

## Laplace mechanism

For user $i$, use
$$
\hat X_i
=
X_i+\operatorname{Laplace}\left(\frac{1}{\epsilon_i}\right).
$$
Since
$$
\operatorname{Var}(\operatorname{Laplace}(\eta))=2\eta^2,
$$
$$
\operatorname{Var}(\hat X_i)
=
\operatorname{Var}(X_i)
+
\frac{2}{\epsilon_i^2}.
$$
$$
\boxed{
\operatorname{Var}(\hat X_i)
=
\operatorname{var}+\frac{2}{\epsilon_i^2}
}.
$$
### Linear estimator
The platform forms
$$
\hat\theta
=
\sum_{i=1}^n w_i\hat X_i
$$
where
$$
\sum_{i=1}^n w_i=1.
$$
Substituting $\hat X_i=X_i+\operatorname{Laplace}(1/\epsilon_i)$,
$$
\hat\theta
=
\sum_{i=1}^n
w_i
\left(
X_i+\operatorname{Laplace}\left(\frac{1}{\epsilon_i}\right)
\right).
$$
Because the Laplace noise is zero mean,
$$
\mathbb E[\hat X_i]
=
\mathbb E[X_i]
=
\theta.
$$
Therefore,
$$
\mathbb E[\hat\theta]
=
\sum_iw_i\theta
=
\theta.
$$
So the estimator is **unbiased**.

### MSE / Variance
Since the observations are independent,
$$
\operatorname{MSE}(\hat\theta)
=
\operatorname{Var}(\hat\theta)
=
\sum_{i=1}^n
w_i^2
\left(
\operatorname{var}
+
\frac{2}{\epsilon_i^2}
\right).
$$
$$
\boxed{
\operatorname{MSE}
=
\sum_{i=1}^n
w_i^2
\left(
\operatorname{var}
+
\frac{2}{\epsilon_i^2}
\right)
}
$$
# Theorem 2

Assume
$$
\epsilon_i\leq1
$$
for all $i$, and

$$
|X|\leq\frac12
$$
Then there exists a universal constant $\ell_l>0$ such that
$$
\boxed{
L_l(\mathcal P^*,\theta,\epsilon)
\geq
\ell_l
\left(
\frac{1}{\sum_{i=1}^n\epsilon_i^2}
\wedge1
\right)
}
$$
and there exists an $\epsilon$-LDP linear estimator satisfying
$$
\boxed{
\mathbb E[(\hat\theta-\theta)^2]
\leq
\frac{\ell_u}{\sum_{i=1}^n\epsilon_i^2}
}
\tag{13}
$$
for every $P\in\mathcal P^*$.

Thus the minimax rate is
$$
\boxed{
\Theta\left(
\frac{1}{\sum_i\epsilon_i^2}
\right)
}
$$
**Proof of the lower bound yet to be done**

**Proof of the upper bound** 
We only need to construct an estimator with the desired MSE.
Recall
$$
\operatorname{MSE}
=
\sum_i
w_i^2
\left(
\operatorname{var}+\frac{2}{\epsilon_i^2}
\right).
$$
For the upper bound, choose
$$
\boxed{
w_i
=
\frac{\epsilon_i^2}
{\sum_{j=1}^n\epsilon_j^2}
}.
$$
These weights sum to $1$:

$$
\sum_iw_i
=
\frac{\sum_i\epsilon_i^2}
{\sum_j\epsilon_j^2}
=
1.
$$
We have
$$
w_i^2
=
\frac{\epsilon_i^4}
{\left(\sum_j\epsilon_j^2\right)^2}.
$$
Therefore,
$$
\operatorname{MSE}
=
\sum_i
\frac{\epsilon_i^4}
{\left(\sum_j\epsilon_j^2\right)^2}
\left(
\operatorname{var}+\frac{2}{\epsilon_i^2}
\right).
$$
$$
=
\frac{
\operatorname{var}\sum_i\epsilon_i^4
+
2\sum_i\epsilon_i^2
}
{\left(\sum_i\epsilon_i^2\right)^2}.
$$

Since $\epsilon_i\leq1$, $\epsilon_i^4\leq\epsilon_i^2.$
Also, because $|X|\leq1/2$, $\operatorname{var}\leq\frac14.$
Thus,
$$
\operatorname{MSE}
\leq
\frac{
\left(\frac14\right)\sum_i\epsilon_i^2
+
2\sum_i\epsilon_i^2
}
{\left(\sum_i\epsilon_i^2\right)^2}.
$$
$$
\operatorname{MSE}
\leq
\frac{9/4}
{\sum_i\epsilon_i^2}.
$$
So for a universal constant $\ell_u$,
$$
\boxed{
\operatorname{MSE}
\leq
\frac{\ell_u}{\sum_i\epsilon_i^2}
}.
$$
