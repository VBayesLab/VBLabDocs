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

**See:** [Variational Bayes Introduction]({{site.baseurl}}{% link Tutorial-VB.md%})

---
## FFVB
<!--- Define custom latex syntax -->
$\def\t{\theta}
\def\LB{\text{LB}}
\def\E{\mathbb{E}}
\def\KL{\text{KL}}
\def\F{\cal{F}}
\def\N{\cal{N}}
\def\s{\sigma}
\def\a{\alpha}
\def\b{\beta}
\def\l{\lambda}
\def\V{\mathbb{V}}
\def\cov{\text{cov}}
\newcommand{\eps}{\epsilon}
\newcommand{\wh}{\widehat}
\newcommand{\wt}{\widetilde}$
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

The gradient in this form is often referred to as *score-function gradient*, another way known as [*reparameterization gradient*]({{site.baseurl}}{% link Tutorial-FFVB-Reparameterization-Trick.md %}) to compute the gradient of the lower bound.

In Monte Carlo simulation, by $\t\sim q_\lambda(\t)$ we mean that we draw a random variable or random vector $\theta$ from the probability distribution with density $q_\l(\t)$. That notation also means $\theta$ is a random variable/vector whose probability density function is $q_\l(\t)$. 
It follows from $\eqref{eq:lb gradient}$ that, by generating $\t\sim q_\lambda(\t)$,  it is straightforward to obtain an unbiased estimator $\wh{\nabla_\l\text{LB}}(\l)$ of the gradient $\nabla_\l\text{LB}(\l)$, i.e., $\E\big[\wh{\nabla_\l\text{LB}}(\l)\big]=\nabla_\l\text{LB}(\l)$.

Unbiased estimate of the gradient of the target function is theoretically required in stochastic optimization. Therefore, we can use stochastic optimization to optimize $\text{LB}(\lambda)$. The basic algorithm is as follows:
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 3: Basic FFVB algorithm
{: #algorithm-3}

- Initialize $\l^{(0)}$ and stop the following iteration if the [stopping criterion](#stopping-criterion) is met.
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
{: #stopping-criterion}

Let us first discuss on the stopping rule. An easy-to-implement stopping rule is to terminate the updating
procedure if the change between $\lambda^{(t+1)}$ and $\lambda^{(t)}$, e.g. in terms of the Euclidean distance, is less than some threshold $\epsilon$.

However, it is difficult to select a meaningful $\epsilon$ as such a distance depends on the scales and the length of the vector $\l$.
Denote by $\wh{\text{LB}}(\l)$ an estimate of $\text{LB}(\l)$ by sampling from $q_\lambda(\theta)$, i.e.,

$$\wh{\text{LB}}(\l)=\frac{1}{S}\sum_{s=1}^S h_{\lambda}(\theta_s),\quad \theta_s\sim q_\lambda(\theta).$$

Although $\text{LB}(\l)$ is expected to be non-decreasing over iterations, its sample estimate $\wh{\text{LB}}(\l)$ might not be.
To account for this, we can use a moving average of the lower bounds
over a window of $t_W$ iterations, $\overline{\text{LB}}(\l^{(t)})=(1/t_W)\sum_{k=1}^{t_W} \wh{\text{LB}}(\l^{(t-k+1)})$.

At convergence, the values $\text{LB}(\l^{(t)})$ stay roughly the same,
therefore $\overline {\text{LB}}(\l^{(t)})$ will average out the noise in $\wh{\text{LB}}(\l^{(t)})$ and is stable. 
The stopping rule that is widely used in machine learning is to stop training if the moving averaged lower bound does not improve
after $P$ iterations; and $P$ is sometimes fancily referred to as the *patience* parameter. 

Typical choice: 
- $P=20$ or $P=50$
- $t_W=20$ or $t_W=50$. 

**Note:** we must not use the last $\lambda^{(t)}$ as the final estimate of $\lambda$, but the one corresponding to the largest $\overline {\text{LB}}(\l^{(t)})$. 

---

## Adaptive learning rate and natural gradient
{: #adaptive-learning-natural-gradient}

Let's write the update in \eqref{eq:update rule} as

$$\begin{cases}
	  \l^{(t+1)}_1=\l^{(t)}_1+a_t\wh{\nabla_{\l_1}\text{LB}}(\l^{(t)})\\
	  ...\\
  	  \l^{(t+1)}_{d_\l}=\l^{(t)}_{d_\l}+a_t\wh{\nabla_{\l_{d_\l}}\text{LB}}(\l^{(t)}),	  
\end{cases}$$

with $d_\l$ the size of vector $\lambda$, which shows that a common scalar learning rate $a_t$ is used for all the coordinates of $\lambda$.
Intuitively, each coordinate of vector $\lambda^{(t+1)}$
might need a different learning rate that can take into account the scale of that coordinate or the geometry of the space $\lambda$ living in.

It turns out that the basic [Algorithm 3](#algorithm-3) rarely works in practice without a method for selecting the learning rate adaptively.

### Adaptive learning rate
{: #adaptive-learning}

For a coordinate $i$ with a large variance $\V(\wh{\nabla_{\l_i}\text{LB}}(\l^{(t)}))$, its learning rate $a_{t,i}$ should be small, otherwise the new update $\lambda^{(t+1)}_i$ jumps all over the place and destroys everything the process has learned so far.

Denote $g_t:=\wh{\nabla_{\lambda}\LB}(\l^{(t)})$ be the gradient vector at step $t$,
and $v_t:=(g_t)^2$ (this is a coordinate-wise operator).
The commonly used adaptive learning rate methods such as ADAM and AdaGrad work by scaling the coordinates of $g_t$ by their corresponding variances.
These variances are estimated by moving average.

The algorithm below is a basic version of this class of adaptive learning methods: 

<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

1. Initialize $\l^{(0)}$, $g_0$ and $v_0$ and set $\bar g=g_0$, $\bar v=v_0$. Let $\beta_1,\beta_2\in(0,1)$ be adaptive learning weights.
2. For $t=0,1,...$, update

$$\begin{eqnarray}
\bar g &=&\beta_1 \bar g+(1-\beta_1)g_t\\
\bar v &=&\beta_2 \bar v+(1-\beta_2)v_t\\
\l^{(t+1)}&=&\l^{(t)}+\a_t \bar g/\sqrt{\bar v},
\end{eqnarray}$$

with $\a_t$ a scalar step size. Here $\bar g/\sqrt{\bar v}$ should be understood component wise.
</div>

Note that the LB gradients $g_t$ have also been smoothened out using moving average.
This helps to accelerate the convergence - a method known as the momentum method in the stochastic optimization literature.
Typical choice of the scalar $\a_t$ is

$$\tag{18}\label{eq:scalar learning rate}
\a_t=\min\left(\eps_0,\eps_0\frac{\tau}{t}\right)=\begin{cases}
\eps_0,&t\leq\tau\\
\eps_0\frac{\tau}{t},&t>\tau
\end{cases} $$

for some small *fixed learning rate* $\eps_0$ (e.g. 0.1 or 0.01) and some threshold $\tau$ (e.g., 1000).  
In the first $\tau$ iterations, the training procedure explores the learning space with a fixed learning rate $\eps_0$,
then this exploration is settled down by reducing the step size after $\tau$ iterations.

### Natural gradient
{: #natural-gradient}

Natural gradient can be considered as an adaptive learning method that exploits the geometry of the $\lambda$ space.
The ordinary gradient $\nabla_\l{\LB}(\l)$ does not adequately capture the geometry of the approximating family $\mathcal Q$ of $q_\l(\t)$.
A small Euclidean distance between $\l$ and $\l'$ does not necessarily mean a small $\KL$ divergence between $q_\l(\t)$ and $q_{\l'}(\t)$.

Statisticians and machine learning researchers have long realized the importance of information geometry on the manifold of a statistical model,
and that the steepest direction for optimizing the objective function $\LB (\l)$ on the manifold formed by the family $q_\l(\t)$ is directed by the so-called natural gradient 
which is defined by pre-multiplying the ordinary gradient with the inverse of the Fisher information matrix

$$\nabla_{\lambda}\LB (\l)^{\text{nat}} := I_F^{-1}(\l)\nabla_\l\LB(\l),$$

with  $I_F(\lambda)=\cov_{q_\l}(\nabla_\l\log q_\l(\t))$ the Fisher information matrix about $\lambda$ with respect to the distribution $q_\lambda$. Given an unbiased estimate $\wh{\nabla_\l\text{LB}}(\l)$, the unbiased estimate of the natural gradient is 

$$\tag{19}\label{eq:natural gradient}
\wh{\nabla_{\lambda}\LB}(\l)^{\text{nat}} = I_F^{-1}(\l)\wh{\nabla_\l\LB}(\l).$$

The main difficulty in using the natural gradient is the computation of $I_F(\lambda)$, and the solution of 
the linear systems required to compute \eqref{eq:natural gradient}. 
The problem is more severe in high dimensional models because this matrix has a large size.
An efficient method for computing $I_F(\lambda)^{-1}\wh{\nabla_\l \LB}(\l)$ is using iterative conjugate gradient methods which solve the linear system $I_F(\lambda)x=\wh{\nabla_\lambda \LB} (\l)$ for $x$ using only matrix-vector products involving $I_F(\lambda)$.

In some cases this matrix vector product can be done efficiently both in terms of computational time and memory requirements by exploiting the structure
of the Fisher matrix $I_F(\lambda)$. 
See Section [CGVB]({{site.baseurl}}{% link Tutorial-FFVB-CGVB.md %}) for a special case where the natural gradient is computed efficiently in high dimensional problems.

As mentioned before, the gradient momentum method is often useful in stochastic optimization that helps accelerate and stabilize the optimization procedure.
The momentum update rule with the natural gradient is 

{%raw%}
$$\begin{eqnarray}
\overline{{\nabla_\l{\LB}}} &=& \alpha_\text{m} \overline{{\nabla_\l{\LB}}}+(1-\alpha_\text{m})\wh{\nabla_{\lambda}\LB}(\l^{(t)})^{\text{nat}},\\
\l^{(t+1)}&=&\l^{(t)}+\a_t \overline{{\nabla_\l{\LB}}},
\end{eqnarray}$$ 
{%endraw%}

where $\alpha_\text{m}\in[0,1]$ is the momentum weight; $\alpha_m$ around 0.6-0.9 is a typical choice. 

The use of the moving average gradient {%raw%}$\overline{{\nabla_\l{\LB}}}${%endraw%} also helps remove some of the noise inherent in the estimated gradients of the lower bound.
Note that the momentum method is already embedded in the moving-average-based adaptive learning rate methods in [Adaptive learning rate](#adaptive-learning) 