Prereqs: [[Myerson's Lemma]], [[(Near) Optimal Estimation with DP]] , and [[Platforms Objective]]
We attempt to design mechanism based off [[Platforms Objective]]
# Payment Identity
The mechanism is a tuple $(\hat{\theta},\epsilon,t)$ Where the estimator $\hat{\theta}$ is fixed for this subsection.
The platform's remaining decisions are therefore:
- the **privacy loss functions** $\epsilon_i(\cdot)$
- the **payment functions** $t_i(\cdot)$
## Interim quantities

Because user $i$'s payment and privacy loss depend on the reports of all users, define their **interim** versions by averaging over the other users' types:

$$
t_i(c_i)
=
\mathbb{E}_{c_{-i}}
\left[
t_i(c_i,c_{-i})
\right]
$$
$$
\epsilon_i(c_i)
=
\mathbb{E}_{c_{-i}}
\left[
\epsilon_i(c_i,c_{-i})
\right].
$$

Here:
- $c_i$ = user $i$'s true/reported privacy sensitivity
- $c_{-i}$ = privacy sensitivities of all other users
- $t_i(c_i)$ = expected payment conditional on user $i$ reporting $c_i$
- $\epsilon_i(c_i)$ = expected privacy loss conditional on user $i$ reporting $c_i$

# Proposed Payment identity
For a fixed estimator $\hat{\theta}$, a central or local privacy data acquisition mechanism is **incentive compatible (IC)** and **individually rational (IR)** iff
$$
t_i(c_i)
=
\mathbb{E}_{c_{-i}}
\left[
\operatorname{MSE}(c,\epsilon,\hat{\theta})
\right]
-
\operatorname{var}
+
c_i\epsilon_i(c_i)
+
\int_{z=c_i}^{\infty}\epsilon_i(z)\,dz
+
d_i
$$
for some constant $d_i\geq 0,$ and $\epsilon_i(z) \text{is non-increasing in }z.$ So the payment is completely characterized by the privacy-loss function, up to the nonnegative constant $d_i$. [[Doubts]] 

Recall the [[Myerson's Lemma]] payment identity from the separate note:
$$
p_i(\hat v_i,\hat V_{-i})
=
\hat v_i x_i(\hat v_i,\hat V_{-i})
-
\int_0^{b_i}x_i(z,\hat V_{-i})\,dz.
$$

The parallels are:

| Classical Myerson                                | Privacy data acquisition                                    |
| ------------------------------------------------ | ----------------------------------------------------------- |
| Valuation $v_i$                                  | Privacy sensitivity $c_i$                                   |
| Allocation $x_i$                                 | Privacy loss $\epsilon_i$                                   |
| Payment $p_i$                                    | Payment $t_i$                                               |
| Higher valuation $\rightarrow$ higher allocation | Higher privacy sensitivity $\rightarrow$ lower privacy loss |
The important difference is that here the user's type affects the estimation error as well. In particular, changing user $i$'s report can change the overall estimator and therefore the MSE that all users benefit from.
## Derivation
Ignoring the MSE for a moment, the user's type-dependent cost is
$$
c_i\epsilon_i(c_i)-t_i(c_i),
$$
which has exactly the form of a single-dimensional procurement mechanism. However, the user's cost also contains
$$
\mathbb{E}_{c_{-i}}
[
\operatorname{MSE}(c_i,c_{-i})
].
$$
Define the interim MSE
$$
M_i(c_i)
=
\mathbb{E}_{c_{-i}}
\left[
\operatorname{MSE}(c_i,c_{-i},\epsilon,\hat{\theta})
\right].
$$

For truthful reporting, the user's interim cost is
$$
M_i(c_i)+c_i\epsilon_i(c_i)-t_i(c_i).
$$

The IC first-order condition gives
$$
t_i'(c_i)
=
M_i'(c_i)
+
c_i\epsilon_i'(c_i).
$$

Integrating from $0$ to $c_i$ gives
$$
t_i(c_i)
=
t_i(0)
+
M_i(c_i)-M_i(0)
+
c_i\epsilon_i(c_i)
-
\int_0^{c_i}\epsilon_i(z)\,dz.
$$
We use IR to determine $t_i(0)$ IR requires
$$
M_i(c_i)
+
c_i\epsilon_i(c_i)
-
t_i(c_i)
\leq
\operatorname{var}.
$$

At $c_i=0$,
$$
M_i(0)-t_i(0)\leq\operatorname{var}.
$$

Using the payment identity and IR, the paper obtains
$$
t_i(0)
=
M_i(0)
-
\operatorname{var}
+
\int_0^\infty \epsilon_i(z)\,dz
+
d_i,
$$

where $d_i\geq 0.$

Substituting $t_i(0)$ into the IC identity gives
$$
t_i(c_i)
=
M_i(c_i)
-
\operatorname{var}
+
c_i\epsilon_i(c_i)
+
\int_{c_i}^{\infty}\epsilon_i(z)\,dz
+
d_i
$$

where $M_i(c_i)=\mathbb{E}_{c_{-i}}\left[\operatorname{MSE}(c_i,c_{-i},\epsilon,\hat{\theta})\right].$ Therefore,
$$
\boxed{
t_i(c_i)
=
\mathbb{E}_{c_{-i}}
[
\operatorname{MSE}(c,\epsilon,\hat{\theta})
]
-
\operatorname{var}
+
c_i\epsilon_i(c_i)
+
\int_{c_i}^{\infty}\epsilon_i(z)\,dz
+
d_i
}
$$
## Intuition
The payment consists of four components:
$$
\underbrace{
\mathbb{E}_{c_{-i}}[\operatorname{MSE}]
}_{\text{compensates for the MSE term}}
-
\underbrace{\operatorname{var}}_{\text{outside option}}
+
\underbrace{c_i\epsilon_i(c_i)}_{\text{current privacy cost}}
+
\underbrace{\int_{c_i}^{\infty}\epsilon_i(z)\,dz}_{\text{myerson magic}}
+
\underbrace{d_i}_{\text{Integration Constant}}.
$$
## Choose $d_i=0$
For an optimal mechanism, $d_i=0.$ The constant $d_i$ increases the payment without helping satisfy the IC or IR constraints once those constraints are already satisfied.
$$
t_i(c_i)
=
\mathbb{E}_{c_{-i}}
\left[
\operatorname{MSE}(c,\epsilon,\hat{\theta})
\right]
-
\operatorname{var}
+
c_i\epsilon_i(c_i)
+
\int_{c_i}^{\infty}\epsilon_i(z)\,dz
$$
