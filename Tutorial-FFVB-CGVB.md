---
layout: default
title: Gaussian VB - CGVB
parent: Fixed Form Variational Bayes
grand_parent: VB Tutorials
nav_order: 3
permalink: /tutorial/ffvb/cgvb
usemathjax: true
---
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
# **GVB with Cholesky decomposed covariance (CGVB)**
{: .fs-8 }

This section describes the GVB with Cholesky decomposed covariance (CGVB) technique

**See:** [Variational Bayes Introduction]({{site.baseurl}}{% link Tutorial-VB.md%}), [Fixed Form Variational Bayes]({{site.baseurl}}{% link Tutorial-FFVB.md%})

---
## CGVB 
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
\def\vech{\text{vech}}$
<!-- End -->
The most popular VB approaches are probably Gaussian VB (GVB) where the approximation $q_\l(\theta)$ is a Gaussian distribution with mean $\mu$ and covariance matrix $\Sigma$. This section presents several variants of this GVB approach.

GVB with Cholesky decomposed covariance (CGVB) uses the Cholesky decomposition for the covariance matrix $\Sigma$, $\Sigma=LL^\top$ with $L$ a lower triangular matrix whose diagonal entries are strictly positive. 

We will use the reparameterization trick for variance reduction.
A sample $\theta\sim q_\lambda(\theta)$ can be written as $\theta=g(\lambda,\veps)=\mu+L\veps$ with $\veps\sim \N_d(0,I_d)$, and $d$ the dimension of $\theta$. The variational parameter vector $\lambda$ includes $\mu$ and the non-zero elements of $L$.

As Jacobian matrix $\nabla_\mu g(\lambda,\veps)=I$, the identity matrix, from [(24)](/VBLabDocs/tutorial/ffvb/reparameterization-trick#mjx-eqn-eq%3AReparameterization_trick_gradient),
the gradient of the lower bound w.r.t. $\mu$ is

$$\nabla_\mu\LB(\l)=\E_\veps\big[\nabla_\theta h_\lambda(\theta)\big],\;\;\;\text{with}\;\;\;\theta=\mu+L\veps.$$

To compute the gradient w.r.t. $L$, we first need some notations.
For a $d\times d$ matrix $A$, denote by $\vec(A)$
the $d^2$-vector obtained by stacking the columns of $A$ from left to right one underneath the other,
by $\vech(A)$ the $\frac12d(d+1)$-vector obtained by stacking the columns of the lower triangular part of $A$,
and by $A\otimes B$ the Kronecker product of matrices $A$ and $B$.

For any matrices $A$, $B$ and $X$ of suitable sizes, we shall use the fact that $\text{vec}(AXB)=(B^\top\otimes A)\text{vec}(X)$.
Then, $L\veps=\text{vec}(I_dL\veps)=(\veps^\top\otimes I_d)\text{vec}(L)$ and hence $\nabla_{\text{vec}(L)} g(\lambda,\veps)=\veps^\top\otimes I_d$.

From [(24)](/VBLabDocs/tutorial/ffvb/reparameterization-trick#mjx-eqn-eq%3AReparameterization_trick_gradient),

$$\begin{eqnarray} \nabla_{\text{vec}(L)}\LB(\lambda)&=&\E_{\veps}\Big[\nabla_{\text{vec}(L)}g(\lambda,\veps)^\top\nabla_\theta h_\lambda(\theta)\Big]\\
&=&\E_{\veps}\Big[(\veps\otimes I_d) \nabla_{\theta} h_\lambda(\theta)\Big]\\
&=&\E_{\veps}\Big[\text{vec}\big(\nabla_{\theta} h_\lambda(\theta)\veps^\top\big)\Big],\;\;\;\text{with}\;\;\;\theta=\mu+L\veps.\end{eqnarray}$$

This implies that
$$\nabla_{\vech(L)}\LB(\l)=\E_{\veps}\big[\vech\big(\nabla_{\theta} h_\lambda(\theta)\veps^\top\big)\big].$$
From [Algorithm 6](/VBLabDocs/tutorial/ffvb/reparameterization-trick#algorithm-6), we arrive at the following GVB algorithm, referred to below as Cholesky GVB.
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 7: Cholesky GVB
- **Input:** Initial $\mu^{(0)}$, $L^{(0)}$ and $\l^{(0)}:=({\mu^{(0)}}^\top,\vech(L^{(0)})^\top)^\top$, number of samples $S$, adaptive learning weights $\beta_1,\beta_2\in(0,1)$, fixed learning rate $\eps_0$, threshold $\tau$, rolling window size $t_W$ and maximum patience $P$. **Model-specific requirement:** function $h(\theta)$ and $\nabla_\theta h(\theta)$.

- **Initialization**
    - Generate $\varepsilon_s\sim N_d(0,I)$, $s=1,...,S$.
	- Compute the estimate of the lower bound gradient
	
      $$\wh{\nabla}_\l\LB(\l^{(0)}) = (\wh{\nabla}_\mu\LB(\l^{(0)})^\top,\wh{\nabla}_{\vech(L)}\LB(\l^{(0)})^\top)^\top$$ 
      where

      $$\begin{eqnarray}
	  \wh{\nabla}_\mu\LB(\l^{(0)})&:=&\frac1S\sum_{s=1}^S\nabla_\theta h_\lambda(\theta_s),\\
	  \wh{\nabla}_{\vech(L)}\LB(\l^{(0)})&:=&\frac1S\sum_{s=1}^S\vech\big(\nabla_\theta h_\lambda(\theta_s)\veps_s^\top\big),
	  \end{eqnarray}$$
	  
      with $\theta_s=\mu^{(0)}+L^{(0)}\veps_s$.
	- Set $g_0:=\wh{\nabla}_\l\mathcal{L}(\l^{(0)})$, $v_0:=(g_0)^2$, $\bar g:=g_0$, $\bar v:=v_0$. 
	- Set $t=0$, $\text{patience}=0$ and $\texttt{stop=false}$.
- **While** $\texttt{stop=false}$:
	- Generate $\veps_s\sim p_{\veps}(\cdot)$, $s=1,...,S$. Recalculate $\mu^{(t)}$ and $L^{(t)}$ from $\lambda^{(t)}$.
	- Compute the estimate of the lower bound gradient
	  
	  $$g_t:=\wh{\nabla}_\l\LB(\l^{(t)})=(\wh{\nabla}_\mu\LB(\l^{(t)})^\top,\wh{\nabla}_{\vech(L)}\LB(\l^{(t)})^\top)^\top$$
      
      where
	  
      $$\begin{eqnarray}
	  \wh{\nabla}_\mu\LB(\l^{(t)})&:=&\frac{1}{S}\sum_{s=1}^S\nabla_\theta h_\lambda(\theta_s),\\
	  \wh{\nabla}_{\vech(L)}\LB(\l^{(t)})&:=&\frac{1}{S}\sum_{s=1}^S\vech\big(\nabla_\theta h_\lambda(\theta_s)\veps_s^\top\big),
	  \end{eqnarray}$$

	  with $\theta_s=\mu^{(t)}+L^{(t)}\veps_s$.
	- Compute $v_t=(g_t)^2$ and 

	  $$\bar g =\beta_1 \bar g+(1-\beta_1)g_t,\;\;\bar v =\beta_2 \bar v+(1-\beta_2)v_t.$$
	
	- Compute $\alpha_t=\min(\varepsilon_0,\varepsilon_0\frac{\tau}{t})$ and update
	
	  $$\l^{(t+1)}=\l^{(t)}+\a_t \bar g/\sqrt{\bar v}$$
	
	- Compute the lower bound estimate
	
	  $$\wh{\mathcal{L}}(\l^{(t)}):=\frac{1}{S}\sum_{s=1}^S h_\lambda(\theta_s).$$
	
	- If $t\geq t_W$: compute the moving averaged lower bound

	  $$\overline{\mathcal{L}}_{t-t_W+1}=\frac{1}{t_W}\sum_{k=1}^{t_W} \wh{\mathcal{L}}(\l^{(t-k+1)}),$$
	
      and if $\overline {\mathcal{L}}_{t-t_W+1}\geq\max(\overline\LB)$ patience = 0; else $\text{patience}:=\text{patience}+1$.
	- If $\text{patience}\geq P$, $\texttt{stop=true}$.
	- Set $t:=t+1$.

</div>

---

**Next:** [GVB with factor decomposed covariance]({{site.baseurl}}{% link Tutorial-FFVB-VAFC.md%})