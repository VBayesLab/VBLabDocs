---
layout: default
title: Example
parent: VB Tutorials
nav_order: 4
permalink: /tutorial/example
---

# **Tutorial examples**
{: .fs-8 }

Examples to illustrate the VB algorithms discussed in the tutorial paper. The examples are numbered in the same order as presented in the [VB tutorial paper](https://www.researchgate.net/publication/340006729_A_practical_tutorial_on_Variational_Bayes)
- [Example 2.1: Mean Field Variational Bayes for Linear Regression](#example2-1)
- [Example 3.1: Mean Field Variational Bayes for Linear Regression](#example3-1)

---
## Example 2.1: Mean Field Variational Bayes for Linear Regression
{: #example2-1}
<!--- Define custom latex syntax -->
$\def\t{\theta}
\def\LB{\text{LB}}
\def\E{\mathbb{E}}
\def\KL{\text{KL}}
\newcommand{\wh}{\widehat}
\newcommand{\wt}{\widetilde}
\def\F{\cal{F}}
\def\N{\cal{N}}
\def\s{\sigma}
\def\a{\alpha}
\def\b{\beta}
\def\l{\lambda}
\newcommand{\eps}{\epsilon}$
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">
**Given:**
- $ y=(11; 12; 8; 10; 9; 8; 9; 10; 13; 7)$ be observations from $\N(\mu,\s^2)$, the normal distribution with mean $\mu$ and variance $\s^2$. 
- Suppose that we use the prior $\N(\mu_0,\s_0^2)$ for $\mu$ and $\text{Inverse-Gamma}(\a_0,\b_0)$ for $\s^2$, with hyperparameters $\mu_0=0$, $\s_0=10$, $\a_0=1$ and $\b_0=1$. 
- Assume the VB factorization $q(\mu,\sigma^2)=q(\mu)q(\sigma^2)$.

**Quesion:** Derive the MFVB procedure for approximating the posterior 
$$p(\mu,\s^2\mid y)\propto p(\mu)p(\s^2)p( y\mid \mu,\s^2).$$
We can view $\mu$ and $\sigma^2$ respectively as $\theta_1$ and $\theta_2$ in [Algorithm 1](/VBLabDocs/tutorial/mfvb#algorithm-1).

</div>

**Solution**

From [(7)](/VBLabDocs/tutorial/mfvb/#mjx-eqn-MFVB-7), the optimal VB posterior for $\s^2$ is

$$\begin{eqnarray}
q(\s^2)&\propto&\exp\left(\E_{-\s^2}[\log p( y,\mu,\s^2)]\right)=\exp\left(\E_{q(\mu)}[\log p( y,\mu,\s^2)]\right)\\
       &\propto&\exp\left( \E_{q(\mu)}\left[\log p(\s^2)+\log p( y \mid \mu,\s^2)\right] \right)\\
       &\propto&\exp\left(-(\a_0+\frac n2+1)\log\s^2-\left(\b_0+\frac12\E_{q(\mu)}\left[ \sum(y_i-\mu)^2\right]\right)/\s^2\right).
\end{eqnarray}$$

In the above derivation, we have ignored all the constants independent of $\sigma^2$ as they are unnecessary for identifying the distribution $q(\sigma^2)$.
It follows that $q(\s^2)$ is inverse-Gamma with parameters
$$\a_q=\a_0+\frac n2,\;\;\;\;\b_q=\b_0+\frac12\E_{q(\mu)}\left[\sum(y_i-\mu)^2\right].$$
Computation of the expecation $\E_{q(\mu)}(\cdot)$ becomes clear shortly after $q(\mu)$ is identified.
From [(8)](/VBLabDocs/tutorial/mfvb/#mjx-eqn-MFVB-8), the optimal VB posterior for $\mu$ is

$$\begin{eqnarray}
q(\mu)&\propto&\exp\left(\E_{q(\s^2)}[\log p(y,\mu,\s^2)]\right)\\
&\propto&\exp\left(\E_{q(\s^2)}[\log p(\mu)+\log p( y \mid \mu,\s^2)]\right)\\
&\propto&\exp\left(-\frac{1}{2\s_0^2}(\mu^2-2\mu_0\mu)-\frac n2\E_{q(\s^2)}[\frac{1}{\s^2}](-2\bar y\mu+\mu^2)\right)\\
&\propto&\exp\left(-\frac{1}{2}\underbrace{\left(\frac{1}{\s_0^2}+n\E_{q(\s^2)}[ \frac{1}{\s^2}]\right)}_{A}\mu^2+\mu\underbrace{\left(\frac{\mu_0}{\s_0^2}+n\bar y\E_{q(\s^2)}[ \frac{1}{\s^2}]\right)}_B\right)\\
&=&\exp\left(-\frac{1}{2}A\mu^2+B\mu\right)\\
&\propto&\exp\left(-\frac12\frac{(\mu-{B}/{A})^2}{1/A}\right).
\end{eqnarray}$$

It follows that $q(\mu)$ is Gaussian with mean $\mu_q$ and variance $\s_q^2$

$$
\mu_q=\frac{\frac{\mu_0}{\s_0^2}+n\bar y\E_{q(\s^2)}[\frac{1}{\s^2}]}{\frac{1}{\s_0^2}+n\E_{q(\s^2)}[\frac{1}{\s^2}]},\;\;\;\;\;
\s_q^2=\Big(\frac{1}{\s_0^2}+n\E_{q(\s^2)}[\frac{1}{\s^2}]\Big)^{-1}.
$$

With the distributions $q(\mu)$ and $q(\sigma^2)$ having identified, we are now able to compute the expectations w.r.t. $q(\mu)$ and $q(\sigma^2)$ in the above:

$$\begin{eqnarray}
\b_q&=&\b_0+\frac12\E_{q(\mu)}\left[\sum(y_i-\mu)^2\right]\\
&=&\b_0+\frac12\left(\sum y_i^2-2n\bar y\E_{q(\mu)}[\mu]+n\E_{q(\mu)}[\mu^2]\right)\\
&=&\b_0+\frac12\sum y_i^2-n\bar y\mu_q+\frac{n}{2}(\mu_q^2+\s_q^2).
\end{eqnarray}$$

As $q(\s^2)\sim\text{Inverse-Gamma}(\a_q,\b_q)$, $\E(1/\s^2)=\a_q/\b_q$. Hence,

$$\mu_q=\Big(\frac{\mu_0}{\s_0^2}+n\bar y\frac{\a_q}{\b_q}\Big)/\Big(\frac{1}{\s_0^2}+n\frac{\a_q}{\b_q}\Big),\;\;\text{ and }\;\;\s_q^2=\Big(\frac{1}{\s_0^2}+n\frac{\a_q}{\b_q}\Big)^{-1}.$$

Note that we did not make any assumption on the parametric form of optimal variational distributions $q(\mu)$ and $q(\sigma^2)$, it is the model (the prior and the likelihood) that determines their form. 

We arrive at the following updating procedure:

- Initialize $\mu_q,\s_q^2$
- Update the following recursively

$$
\begin{eqnarray}
\a_q&\leftarrow&\a_0+\frac n2,\\
\b_q&\leftarrow&\b_0+\frac12\sum y_i^2-n\bar y\mu_q+\frac{n}{2}(\mu_q^2+\s_q^2),\\
\mu_q&\leftarrow&\Big(\frac{\mu_0}{\s_0^2}+n\bar y\frac{\a_q}{\b_q}\Big)/\Big(\frac{1}{\s_0^2}+n\frac{\a_q}{\b_q}\Big),\\
\s_q^2&\leftarrow&\Big(\frac{1}{\s_0^2}+n\frac{\a_q}{\b_q}\Big)^{-1},
\end{eqnarray}
$$

until convergence

We can stop the iterative scheme when the change of the $\ell_2$-norm of the vector $\l=(\a_q,\b_q,\mu_q,\s_q^2)^\top$ is smaller than some $\eps$, $\eps=10^{-5}$ for example.
We can also initialize $\a_q,\b_q$ and then update the variational parameters recursively in the order of $\mu_q$, $\s_q^2$, $\a_q$ and $\b_q$. 

However, it's often a better idea to initialize $\mu_q,\s_q^2$ as it is easier to guess the values related to location parameters than the scale parameters.
Figure 1 plots the posterior densities estimated by the MFVB algorithm derived above, and by Gibbs sampling.  

<img src="/VBLabDocs/assets/images/Example2-1.JPG" class="center"/>

It is straightforward to extend the MFVB procedure in [Algorithm 1](/VBLabDocs//tutorial/mfvb/#algorithm-1) to the general case where $\theta$ is divided into $k$ blocks $\t=(\t_1^\top,\t_2^\top,...,\t_k^\top)^\top$,
and where we want to approximate the posterior  $p(\t_1,\t_2,...,\t_k|y)$ by $q(\t)=q_1(\t_1)q_2(\t_2)...q_k(\t_k)$.

The optimal $q_j(\t_j)$ that maximizes $\LB(q)$, when $q_1,...,q_{j-1},q_{j+1},...,q_{k}$ are fixed, is

$$\tag{9}\label{MFVB-9}
q_j(\t_j)\propto \exp\big(\E_{-\t_j}[\log p(y,\t)]\big),\;\;\;j=1,...,k.
$$

Here $\E_{-\t_j}(\cdot)$ denotes the expectation w.r.t. $q_1$,..., $q_{j-1}$, $q_{j+1}$,..., $q_{k}$, i.e.,
$$\E_{-\t_j}\big[\log p(y,\t)\big]:=\int q_1(\theta_1)...q_{j-1}(\theta_{j-1})q_{j+1}(\theta_{j+1})...q_{k}(\theta_k)\log p(y,\t)d\theta_1....d\theta_{j-1}d\theta_{j+1}...d\theta_k.$$

A similar procedure to Algorithm [Algorithm 1](/VBLabDocs//tutorial/mfvb/#algorithm-1) can be developed, in which we first initialize the parameters in the $k-1$ factors $q_1,...,q_{k-1}$, then update $q_k$ and the other factors recursively.

---

## Example 3.1: Fixed From VB with control variates
{: #example3-1}