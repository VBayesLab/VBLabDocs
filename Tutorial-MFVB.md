---
layout: default
title: Mean Field Variational Bayes
parent: VB Tutorials
nav_order: 2
has_children: true
permalink: /tutorial/mfvb
---
# **Mean Field Variational Bayes (MFVB)**
{: .fs-8 }

This tutorial gives a quick introduction to Mean Field Variational Bayes. 
{: .fs-6 .fw-300 }

**See:** [Variational Bayes Introduction]({% link Tutorial-VB.md%})

---
<!--- Define custom latex syntax -->
$\def\t{\theta}
\def\LB{\text{LB}}
\def\E{\mathbb{E}}
\def\KL{\text{KL}}
\newcommand{\wh}{\widehat}
\newcommand{\wt}{\widetilde}
\def\F{\cal{F}}$
<!-- End -->
Let's write $\theta$ as $\theta=(\theta_1^\top,\t_2^\top)^\top$. Here $a^\top$ denotes the transpose of vector $a$; and all vectors in this tutorial are column vectors.
MFVB assumes the following factorization form for $q$

$$q(\t)=q_1(\t_1)q_2(\t_2),$$

i.e., we ignore the posterior dependence between $\theta_1$, $\theta_2$ and attempt to approximate $p(\t_1,\t_2\|y)$ by $q(\t)=q_1(\t_1)q_2(\t_2)$. This is the only assumption/restriction we put on the class $\mathcal Q$.

The lower bound in $\eqref{1}$ is 

$$\begin{eqnarray}
\LB(q_1,q_2)&=&\int q_1(\t_1)q_2(\t_2)\log\frac{p(\t,y)}{q_1(\t_1)q_2(\t_2)}d\t_1d\t_2\\
&=&\int q_1(\t_1)q_2(\t_2)\log p(\t,y)d\t_1d\t_2\\
&&-\int q_1(\t_1)\log q_1(\t_1)d\t_1-\int q_2(\t_2)\log q_2(\t_2)d\t_2\\
&=&\int q_1(\t_1)\mathbb{E}_{-\t_1}[\log p(y,\t)]d\t_1-\int q_1(\t_1)\log q_1(\t_1)d\t_1+C(q_2)
\end{eqnarray}$$

where $\E_{-\t_1}[\log p(y,\t)]:=\E_{q_2(\t_2)}[\log p(y,\t)]=\int q_2(\t_2)\log p(y,\t) d\t_2$ and $C(q_2)$ is the term independent of $q_1$.

The funny-looking notation $\E_{-\t_1}(\cdot)$, meaning we take the expectation with respect to everything except $\theta_1$, turns out to be very convenient when we deal with the general MFVB procedure later.
Hence,

$$\begin{eqnarray}
\LB(q_1,q_2)&=&\int q_1(\t_1)\log\frac{\exp\big(\E_{-\t_1}[\log p(y,\t)]\big)}{q_1(\t_1)}d\t_1+C(q_2)\notag\\
&=&\int q_1(\t_1)\log\frac{\wt q_1(\theta_1)}{q_1(\t_1)}d\t_1+C(q_2)+\log\wt C(q_2)\notag\\
&=&-\KL(q_1\|\wt q_1)+C(q_2)+\log\wt C(q_2),
\end{eqnarray}$$

where $\wt q_1(\t_1)$ is the probability density function determined by
$$\wt q_1(\t_1):=\frac{\exp(\E_{-\t_1}[\log p(y,\t)])}{\wt C(q_2)}\propto \exp(\E_{-\t_1}[\log p(y,\t)]),$$
with $\wt C(q_2):=\int \exp(\E_{-\t_1}[\log p(y,\t)])d\t_1$ also independent of $q_1$.

We therefore have that

$$\tag{2}\label{MFVB-1}
\LB(q_1,q_2)=-\KL(q_1\|\wt q_1)+\text{ constant independent of $q_1$}.
$$

Similarly,

$$\tag{3}\label{MFVB-2}
\LB(q_1,q_2)=-\KL(q_2\|\wt q_2)+\text{ constant independent of $q_2$},
$$

where $\wt q_2(\t_2)\propto \exp(\E_{-\t_2}[\log p(y,\t)])$ with $\E_{-\t_2}[\log p(y,\t)]:=\int q_1(\t_1)\log p(y,\t) d\t_1$.

The expressions in \eqref{MFVB-1}-\eqref{MFVB-2} suggest a coordinate ascent optimization procedure for maximizing the lower bound: given $q_2$, we minimize $\KL(q_1\|\wt q_1)$ to find $q_1$, and given $q_1$ we minimize $\KL(q_2\|\wt q_2)$ to find $q_2$.

The hope is that solving the optimization problems

$$\tag{4}\label{MFVB-3}
\min_{q_1}\big\{\KL(q_1\|\wt q_1)\big\}\;\;\;\text{ and }\;\;\;\min_{q_2}\big\{\KL(q_2\|\wt q_2)\big\}
$$

is easier than minimizing the original KL divergence between $q(\theta_1,\theta_2)$ and $p(\theta_1,\theta_2\|y)$.

If $\wt q_1$ and $\wt q_2$ are tractable and standard distributions (distribution that is well-understood and widely used, such as Gaussian, Gamma, etc.), then of course the solution to \eqref{MFVB-3} is 
$q_1=\wt q_1$ and $q_2=\wt q_2$.

The most useful scenario is the case of [*conjugate prior*](https://en.wikipedia.org/wiki/Conjugate_prior): the prior $p(\t_1)$ belongs to a parametric density family $\F_1$, then
$\wt q_1(\t_1)$ also belongs to $\F_1$. Similarly,  the prior $p(\t_2)$ belongs to a parametric density family $\F_2$, then
$\wt q_2(\t_2)$ also belongs to $\F_2$.

Then the solutions to $\eqref{MFVB-3}$ are
$$q_1(\theta_1)=\wt q_1(\t_1)\in\F_1\;\;\;\text{ and }\;\;\;q_2(\theta_2)=\wt q_2(\t_2)\in\F_2,$$
and in order to identify $q_1$ and $q_2$ it's only necessary to compute their parameters.

Computing the parameter in $q_1$ requires $q_2$ and vice versa,
which suggests the following coordinate ascent-type algorithm for maximizing the lower bound:   
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 1: Mean Field Variational Bayes
{: #algorithm-1}

- **Step 1:** Initialize the parameter of $q_1(\t_1)$
- **Step 2:** Given $q_1(\t_1)$, update the parameter of $q_2(\t_2)$ using

$$
\tag{5}\label{eq:optimal VB 1}
q_2(\t_2)\propto \exp\big(\E_{-\t_2}[\log p(y,\t)]\big)=\exp\Big(\int q_1(\t_1)\log p(y,\t_1,\t_2)d\t_1\Big)
$$

- **Step 3:** Given $q_2(\t_2)$, update the parameter of $q_1(\t_1)$ using 

$$\tag{6}\label{eq:optimal VB 2}
q_1(\t_1)\propto \exp\big(\E_{-\t_1}[\log p(y,\t)]\big)=\exp\Big(\int q_2(\t_2)\log p(y,\t_1,\t_2)d\t_2\Big).
$$

- **Step 4:** Repeat Steps 2 and 3 until the stopping condition is met.

</div>
<br>
A stopping rule is to terminate the update if the change in the parameters of the VB posterior $q(\t)=q_1(\t_1)q_2(\t_2)$ between two consecutive iterations is less than some threshold $\epsilon$.

In the case the lower bound $\LB(q_1,q_2)$ can be computed, one can stop the algorithm if the increase (or the percentage of the increase) in the lower bound is less than some threshold. 

**Note:** $\LB(q)$ increases after each iteration.