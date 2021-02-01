---
layout: default
title: Fixed Form Variational Bayes
parent: VB Tutorials
nav_order: 3
has_children: true
permalink: /tutorial/ffvb
---

# **Fixed Form Variational Bayes (FFVB)**
{: .fs-8 }

This tutorial gives a quick introduction to Mean Field Variational Bayes. 
{: .fs-6 .fw-300 }

**See:** [Variational Bayes Introduction]({% link Tutorial-VB.md%})

---
## FFVB
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
\def\d{d}
\newcommand{\eps}{\epsilon}$
<!-- End -->
FFVB assumes a fixed parametric form for the VB approximation density $q$, i.e. $q=q_\l$ belongs to some class of distributions $\mathcal Q$ indexed by a vector $\lambda$ called the *variational parameter*.
For example, $q_\l$ is a Gaussian distribution with mean $\mu$ and covariance matrix $\Sigma$.

FFVB finds the best $q_\lambda$ in the class $\mathcal{Q}$ by optimizing the lower bound
$$\tag{15}\label{eq: LB}
\LB(\lambda):= \LB(q_\lambda) =\E_{q_\lambda}\left[\log\frac{p(\theta)p(y|\theta)}{q_\lambda(\theta)}\right]=\E_{q_\lambda}\big[h_\lambda(\theta)\big],$$
with 
$$h_\lambda(\t):=\log\big(\frac{p(\t) p(y \mid \t)}{q_\lambda(\theta)}\big).$$

Later we also use $h(\theta)$, without the subscript, to denote the model-specific function $\log\;\big(p(\t) p(y|\t)\big)$.
Except for a few trivial cases where the LB can be computed analytically and optimized using classical optimization routines,
stochastic optimization is often used to optimize $\LB(\lambda)$.
The gradient vector of LB is

$$\begin{eqnarray}\tag{16}\label{eq:lb gradient}
\nabla_\lambda\LB(\lambda)&=&\int_\Theta\nabla_\lambda q_\lambda(\theta)\log\frac{p(\theta)p(y \mid \theta)}{q_\lambda(\theta)}d\theta-\int_\Theta q_\lambda(\theta)\nabla_\lambda\log q_\lambda(\theta)d\theta\\
&=&\int_\Theta q_\lambda(\theta)\nabla_\lambda \log q_\lambda(\theta)\log\frac{p(\theta)p(y \mid \theta)}{q_\lambda(\theta)}d\theta-\int_\Theta \nabla_\lambda q_\lambda(\theta)d\theta\\
&=&\int_\Theta q_\lambda(\theta)\nabla_\lambda \log q_\lambda(\theta)\log\frac{p(\theta)p(y \mid \theta)}{q_\lambda(\theta)}d\theta-\nabla_\lambda\int_\Theta  q_\lambda(\theta)d\theta\\
&=&\E_{q_\lambda}\left[\nabla_\lambda \log q_\lambda(\theta)\times\log\frac{p(\theta)p(y \mid \theta)}{q_\lambda(\theta)}\right]\\
&=&\E_{q_\lambda}\left[\nabla_\lambda \log q_\lambda(\theta)\times h_\lambda(\theta)\right].\end{eqnarray}$$

The gradient in this form is often referred to as *score-function gradient*, another way known as *reparameterization gradient* to compute the gradient of the lower bound is discussed later in \eqref{eq:Reparameterization trick gradient}.

In Monte Carlo simulation, by $\t\sim q_\lambda(\t)$ we mean that we draw a random variable or random vector $\theta$ from the probability distribution with density $q_\l(\t)$. That notation also means $\theta$ is a random variable/vector whose probability density function is $q_\l(\t)$. 
It follows from $\eqref{eq:lb gradient}$ that, by generating $\t\sim q_\lambda(\t)$,  it is straightforward to obtain an unbiased estimator $\wh{\nabla_\l\text{LB}}(\l)$ of the gradient $\nabla_\l\text{LB}(\l)$, i.e., $\E\big[\wh{\nabla_\l\text{LB}}(\l)\big]=\nabla_\l\text{LB}(\l)$.

Unbiased estimate of the gradient of the target function is theoretically required in stochastic optimization. Therefore, we can use stochastic optimization to optimize $\text{LB}(\lambda)$. The basic algorithm is as follows:
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 3: Basic FFVB algorithm
{: #algorithm-3}

- Initialize $\l^{(0)}$ and stop the following iteration if the [stopping criterion](#algorithm-1) is met.
- For $t=0,1,...$
    - Generate $\theta_s\sim q_{\lambda^{(t)}}(\theta)$, $s=1,...,S$
	- Compute the unbiased estimate of the LB gradient
	  $$\wh{\nabla_\l\text{LB}}(\l^{(t)}):=\frac{1}{S}\sum_{s=1}^S\nabla_\lambda \log q_\lambda(\theta_s)\times h_\lambda(\theta_s)|_{\lambda=\lambda^{(t)}}$$
	- Update 
	  $$\tag{17}\label{eq:update rule} \l^{(t+1)}=\l^{(t)}+a_t\wh{\nabla_\l\text{LB}}(\l^{(t)}).$$

</div>
The algorithmic parameter $S$ is referred to as the number of Monte Carlo samples (used to estimate the gradient of the lower bound).
The sequence of learning rates $\\{a_t\\}$ should satisfy the theoretical requirements $a_t>0$, $\sum_t a_t=\infty$ and $\sum_t a_t^2<\infty$.
However, this basic VB algorithm hardly works in practice and requires some refinements to make it work.
Much of the rest of this section focuses on presenting and explaining those refinements.

---

## Stopping criterion
{: #algorithm-1}

Let us first discuss on the stopping rule. An easy-to-implement stopping rule is to terminate the updating
procedure if the change between $\lambda^{(t+1)}$ and $\lambda^{(t)}$, e.g. in terms of the Euclidean distance, is less than some threshold $\epsilon$.

However, it is difficult to select a meaningful $\epsilon$ as such a distance depends on the scales and the length of the vector $\l$.
Denote by $\wh{\text{LB}}(\l)$ an estimate of $\text{LB}(\l)$ by sampling from $q_\lambda(\theta)$, i.e.,
$$\wh{\text{LB}}(\l)=\frac{1}{S}\sum_{s=1}^S h_{\lambda}(\theta_s),\quad \theta_s\sim q_\lambda(\theta).$$

Although $\text{LB}(\l)$ is expected to be non-decreasing over iterations, its sample estimate $\wh{\text{LB}}(\l)$ might not be.
To account for this, we can use a moving average of the lower bounds
over a window of $t_W$ iterations, $\overline {\text{LB}}(\l^{(t)})=(1/t_W)\sum_{k=1}^{t_W} \wh{\text{LB}}(\l^{(t-k+1)})$.

At convergence, the values $\text{LB}(\l^{(t)})$ stay roughly the same,
therefore $\overline {\text{LB}}(\l^{(t)})$ will average out the noise in $\wh{\text{LB}}(\l^{(t)})$ and is stable. 
The stopping rule that is widely used in machine learning is to stop training if the moving averaged lower bound does not improve
after $P$ iterations; and $P$ is sometimes fancily referred to as the *patience* parameter. 

Typical choice: 
- $P=20$ or $P=50$
- $t_W=20$ or $t_W=50$. 

**Note:** we must not use the last $\lambda^{(t)}$ as the final estimate of $\lambda$, but the one corresponding to the largest $\overline {\text{LB}}(\l^{(t)})$. 