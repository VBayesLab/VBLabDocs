---
layout: default
title: Variational Bayes
parent: VB Tutorials
nav_order: 1
permalink: /tutorial/vb
---

# **Variational Bayes Introduction**
{: .fs-8 }

This tutorial gives a quick introduction to Variational Bayes (VB), also called Variational Inference or Variational Approximation.

---

Denote:
- $y$ $\rightarrow$ data
- $p(y\|\theta)$ $\rightarrow$ likelihood function based on a postulated model
- $\theta\in\Theta$ $\rightarrow$ vector of model parameters to be estimated.
- $p(\theta)$ $\rightarrow$ prior. 
- Notation $a:=b$ means $a$ is defined by $b$. 
- For any random variable or random vector $X$ and any function $g(X)$, we denote by $E_{f}\big[g(X)\big]$ (or $E_{X\sim f}\big[g(X)\big]$, or simply $E_{X}\big[g(X)\big]$ the expectation of $g(X)$ where $X$ follows a probability distribution with density function $f(x)$.

Bayesian inference encodes all the available information about the model parameter $\theta$ in its posterior distribution with density
$$p(\theta|y)=\frac{p(y,\theta)}{p(y)}=\frac{p(\theta)p(y|\theta)}{p(y)}\propto p(\theta)p(y|\theta),$$
where $p(y)=\int_\Theta p(\theta)p(y|\theta)d\theta$, called the [*marginal likelihood*](https://en.wikipedia.org/wiki/Marginal_likelihood) or *evidence*.

Here, the notation $\propto$ means proportional up to the normalizing constant that is independent of the parameter ($\theta$).
In most Bayesian derivations, such a constant can be safely ignored.
Bayesian inference typically requires computing expectations with respect to the posterior distribution.
For example, the posterior mean, which is often used for point estimation, is an expectation of $\theta$ with respect to the posterior distribution $p(\theta|y)$.  

However, it is often difficult to compute such expectations, partly because the density $p(\theta|y)$ itself is intractable as the normalizing constant $p(y)$ is often unknown.
For many applications, Bayesian inference is performed using MCMC, which estimates 
expectations w.r.t. $p(\theta|y)$ by sampling from it.
For other applications where $\theta$ is high dimensional or fast computation is of primary interest, VB is an attractive alternative to MCMC. 
VB approximates the posterior distribution by a probability distribution with density $q(\theta)$
belonging to some tractable family of distributions $\mathcal Q$ such as Gaussians. 

The best VB approximation $q^\*\in\mathcal{Q}$ is found by minimizing the Kullback-Leibler (KL) divergence *from* $q(\theta)$ *to* $p(\theta\|y)$ 

$$\tag{1}\label{1} q^*=\arg\min_{q\in\mathcal Q} { \text{KL}(q\|p(\cdot\|y)):=\int q(\theta)\log\frac{q(\theta)}{p(\theta\|y)}d\theta }. $$

Then, Bayesian inference is performed with the intractable posterior $p(\theta|y)$ replaced by the tractable VB approximation $q^\*(\theta)$.
It is easy to see that
$$\text{KL}(q||p(\cdot|y)) = -\int q(\theta)\log\frac{p(\theta)p(y|\theta)}{q(\theta)}d\theta+\log p(y),$$
thus minimizing  KL is equivalent to maximizing the lower bound on $\log p(y)$

$$\text{LB}(q):=\int q(\theta)\log\frac{p(\theta)p(y\|\theta)}{q(\theta)}d\theta=E_{q}\Big(\log\frac{p(\theta)p(y\|\theta)}{q(\theta)}\Big).$$

Without any constraint on $\mathcal Q$, the solution to $\eqref{1}$ is $q^*(\theta)=p(\theta|y)$; of course this solution is useless as it is itself intractable.
Depending on the constraint imposed on the class $\mathcal Q$, VB algorithms can be categorized into two classes: 
- [Mean Field VB (MFVB)]({% link Tutorial-MFVB.md %})
- [Fixed Form VB (FFVB)]({% link Tutorial-FFVB.md %})

which can be read completely separately depending on the reader's interest.

**Note:** VBLab provides only FFVB techniques.

---

**Next:** [Mean Field VB (MFVB)]({% link Tutorial-MFVB.md %})