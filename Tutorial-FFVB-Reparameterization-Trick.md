---
layout: default
title: Reparameterization Trick
parent: Fixed Form Variational Bayes
grand_parent: VB Tutorials
nav_order: 2
permalink: /tutorial/ffvb/reparameterization-trick
---
# **FFVB: Reparameterization Trick**
{: .fs-8 }

This section describes a control variate technique for variance reduction.

**See:** [Variational Bayes Introduction]({{site.baseurl}}{% link Tutorial-VB.md%}), [Fixed Form Variational Bayes]({{site.baseurl}}{% link Tutorial-FFVB.md%})

---
## Reparameterization Trick
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
\newcommand{\eps}{\epsilon}
\def\veps{\varepsilon}
\def\vech{\text{vech}}
\def\diag{\text{diag}}
\def\V{\mathbb{V}}
\def\cov{\text{cov}}$
<!-- End -->
The reparameterization trick is an attractive alternative to the [control variate]({{site.baseurl}}{%link Tutorial-FFVB-Control-Variate.md %}) method.
Suppose that for $\t\sim q_\l(\cdot)$, there exists a deterministic function $g(\l,\veps)$ such that $\t=g(\l,\veps)\sim q_\l(\cdot)$ where $\veps\sim p_\veps(\cdot)$.

We emphasize that $p_\veps(\cdot)$ must not depend on $\l$.
For example, if $q_\l(\t)=\N(\t;\mu,\sigma^2)$ then $\t=\mu+\sigma \veps$ with $\veps\sim\N(0,1)$. 
Writing $\LB(\l)$ as an expectation with respect to $p_\varepsilon(\cdot)$

$$\begin{aligned}
  \LB(\l) & = \E_{\veps\sim p_\veps}\Big(h_\lambda(g(\veps,\l) )\Big),
\end{aligned}$$

where $\E_{\veps\sim p_\veps}(\cdot)$ denotes expectation with respect to $p_\veps(\cdot)$, and differentiating under the integral sign gives

\begin{align}
  \nabla_\l\LB(\l) & = \E_{\veps\sim p_\veps}\Big(\nabla_\l g(\l,\veps)^\top\nabla_\theta h_\l (\theta)\Big) + \E_{\veps\sim p_\veps}\Big(\nabla_\lambda h_\l(\theta)\Big)\notag
\end{align}

where the $\theta$ within $h_\l(\theta)$ is understood as $\theta=g(\veps,\lambda)$ with $\lambda$ fixed. 

In particular, the gradient $\nabla_\lambda h_\l(\theta)$ is taken when $\theta$ is not considered as a function of $\lambda$.
Here, with some abuse of notation, $\nabla_\l g(\l,\veps)$ denotes the Jacobian matrix of size $d_\theta\times d_\lambda$ of the vector-valued function $\theta=g(\lambda,\veps)$. 
Note that

$$\begin{aligned}
\E_{\veps\sim p_\veps}\Big(\nabla_\lambda h_\l(\theta)\Big)=\E_{\veps\sim q_\veps}\Big(\nabla_\lambda h_\l\big(\theta=g(\veps,\l)\big)\Big)&=-\E_{\veps\sim q_\veps}\Big(\nabla_\lambda \log q_\l\big(\theta=g(\veps,\l)\big)\Big)\\
&=-\E_{\theta\sim q_\l}\Big(\nabla_\lambda \log q_\l(\theta)\Big)=0,
\end{aligned}$$

hence

$$\begin{equation}\tag{24}\label{eq:Reparameterization trick gradient}
  \nabla_\l\LB(\l) = \E_{\veps\sim q_\veps}\Big(\nabla_\l g(\l,\veps)^\top\nabla_\theta h_\l (\theta)\Big).
\end{equation}$$

The gradient \eqref{eq:Reparameterization trick gradient} can be estimated unbiasedly using  i.i.d samples $\veps_s\sim p_\veps(\cdot)$, $s=1,...,S$, as

$$\begin{equation}\tag{25}\label{roedergradest}
\wh{\nabla_\l{\LB}}(\l) =\frac1S\sum_{s=1}^S\nabla_\l g(\l,\veps_s)^\top \nabla_\t \big\{h_\lambda(g(\l,\veps_s))\big\}. 
\end{equation}$$

The *reparametrization gradient* estimator $\eqref{roedergradest}$ is often more efficient than alternative approaches to estimating
the lower bound gradient, partly because it takes into account the information from the gradient $\nabla_\theta h_\l(\theta)$. 

In typical VB applications, the number of Monte Carlo samples $S$ used in estimating the lower bound gradient can be as small as 5 if 
the reparameterization trick is used, while the control variates method requires an $S$ of about hundreds or more.
However, there is a dilemma about choosing $S$ that we must be careful of.

With the reparameterization trick, a small $S$ might be enough for estimating the lower bound gradient,
however, we still need a moderate $S$ in order to obtain a good estimate of the lower bound if lower bound is used in the stopping criterion.
Also, compared to score-function gradient, FFVB approaches that use reparameterization gradient require not only the model-specific function $h(\theta)$ but also its gradient $\nabla_\theta h(\theta)$.

[Algorithm 6](#algorithm-6) provides a detailed implementation of the FFVB approach that uses the reparameterization trick and adaptive learning.
A small modification of [Algorithm 6](#algorithm-6) (not presented) gives the implementation of the FFVB approach that uses the reparameterization trick and natural gradient.
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 6: FFVB with reparameterization trick and adaptive learning
{: #algorithm-6}

- **Input:** Initial $\l^{(0)}$, adaptive learning weights $\beta_1,\beta_2\in(0,1)$, fixed learning rate $\eps_0$, threshold $\tau$, rolling window size $t_W$ and maximum patience $P$. **Model-specific requirement:** function $h(\theta):=\log\big(p(\theta)p(y\mid\theta)\big)$ and its gradient $\nabla_\theta h(\theta)$.
- **Initialization**
	- Generate $\varepsilon_s\sim p_{\veps}(\cdot)$, $s=1,...,S$.
	- Compute the unbiased estimate of the LB gradient

	  $$\wh{\nabla_\l\text{LB}}(\l^{(0)}):=\frac1S\sum_{s=1}^S\nabla_\l g(\l,\veps_s)^\top \nabla_\t \big\{h_\lambda(g(\l,\veps_s))\big\}\big|_{\lambda=\lambda^{(0)}}$$
	
	- Set $g_0:=\wh{\nabla_\l\text{LB}}(\l^{(0)})$, $v_0:=(g_0)^2$, $\bar g:=g_0$, $\bar v:=v_0$. 
	- Set $t=0$, $\text{patience}=0$ and $\texttt{stop=false}$.
- **While** $\texttt{stop=false}$:
	- Generate $\veps_s\sim p_{\veps}(\cdot)$, $s=1,...,S$
	- Compute the unbiased estimate of the LB gradient
		
		$$g_t:=\wh{\nabla_\l\text{LB}}(\l^{(t)})=\frac1S\sum_{s=1}^S\nabla_\l g(\l,\veps_s)^\top \nabla_\t \big\{h_\lambda(g(\l,\veps_s))\big\}\big|_{\lambda=\lambda^{(t)}}$$
	
	- Compute $v_t=(g_t)^2$ and 

	  $$\bar g =\beta_1 \bar g+(1-\beta_1)g_t,\;\;\bar v =\beta_2 \bar v+(1-\beta_2)v_t.$$
	
	- Compute $\alpha_t=\min(\varepsilon_0,\varepsilon_0\frac{\tau}{t})$ and update
	
	  $$\l^{(t+1)}=\l^{(t)}+\a_t \bar g/\sqrt{\bar v}$$
	
	- Compute the lower bound estimate
	  
	  $$\wh{\text{LB}}(\l^{(t)}):=\frac{1}{S}\sum_{s=1}^S h_{\lambda^{(t)}}(\theta_s),\;\;\;\theta_s=g(\lambda^{(t)},\veps_s).$$
	
	- If $t\geq t_W$: compute the moving average lower bound

	  $$\overline {\text{LB}}_{t-t_W+1}=\frac{1}{t_W}\sum_{k=1}^{t_W} \wh{\text{LB}}(\l^{(t-k+1)}),$$
	  
	  and if $\overline {\text{LB}}_{t-t_W+1}\geq\max(\overline\LB)$ patience = 0; else $\text{patience}:=\text{patience}+1$.
	- If $\text{patience}\geq P$, $\texttt{stop=true}$
	- Set $t:=t+1$

</div>

---

**Next:** [GVB with Cholesky decomposed covariance]({{site.baseurl}}{% link Tutorial-FFVB-CGVB.md%})