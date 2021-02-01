---
layout: default
title: Control Variate
parent: Fixed Form Variational Bayes
grand_parent: VB Tutorials
nav_order: 1
permalink: /tutorial/ffvb/control-variate
---

# **FFVB: Control variate**
{: .fs-8 }

This section describes a control variate technique for variance reduction.

**See:** [Variational Bayes Introduction]({{site.baseurl}}{% link Tutorial-VB.md%}), [Fixed Form Variational Bayes]({{site.baseurl}}{% link Tutorial-FFVB.md%})

---
## Control variate
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
As is typical of stochastic optimization algorithms, the performance of [Algorithm 3](/VBLabDocs/tutorial/ffvb/#algorithm-3) depends greatly on the variance of the noisy gradient. Variance reduction for the noisy gradient is a key ingredient in FFVB algorithms.   

Let $\t_s\sim q_\l(\t)$, $s=1,...,S$, be $S$ samples from the variational distribution $q_{\l}(\t)$. A naive estimator of the $i$th element of the vector $\nabla_\lambda\text{LB}(\l)$ is
$$\tag{20}\label{eq: naive KL estimate}\wh{\nabla_{\lambda_i}\LB}(\l)^{\text{naive}}=\frac1S\sum_{s=1}^S\nabla_{\lambda_i}[\log q_\lambda(\t_s)]\times h_\lambda(\t_s),$$
whose variance is often too large to be useful. 

For any number $c_i$, consider   

$$\tag{21}\label{eq: reduced var KL estimate}
\wh{\nabla_{\lambda_i}\LB}(\l)=\frac1S\sum_{s=1}^S\nabla_{\lambda_i}[\log q_\lambda(\t_s)](h_\lambda(\t_s)-c_i),$$

which is still an unbiased estimator of $\nabla_{\lambda_i}\text{LB}(\l)$ since $\E(\nabla_{\lambda}[\log q_\lambda(\t)])=0$,
whose variance can be greatly reduced by an appropriate choice of control variate $c_i$.

The variance of $\wh{\nabla_{\lambda_i}\LB}(\l)$ is 

$$\frac1S\V\Big(\nabla_{\lambda_i}[\log q_\lambda(\t)]h_\lambda(\t)\Big)+\frac{c_i^2}{S}\V\Big(\nabla_{\lambda_i}[\log q_\lambda(\t)]\Big)-\frac{2c_i}{S}\cov\Big(\nabla_{\lambda_i}[\log q_\lambda(\t)]h_\lambda(\t),\nabla_{\lambda_i}[\log q_\lambda(\t)]\Big).$$

The optimal $c_i$ that minimizes this variance is 

$$\tag{22}\label{eq:optimal c_i}
c_i=\cov\Big(\nabla_{\lambda_i}[\log q_\lambda(\t)] h_\lambda(\t),\nabla_{\lambda_i}[\log q_\lambda(\t)]\Big)\Big/\V\Big(\nabla_{\lambda_i}[\log q_\lambda(\t)]\Big).$$

Then 

$$\V\left(\wh{\nabla_{\lambda_i}\LB}(\l)\right)=\V\left(\wh{\nabla_{\lambda_i}\LB}(\l)^{\text{naive}}\right)(1-\rho^2_i) \leq \V\left(\wh{\nabla_{\lambda_i}\LB}(\l)^{\text{naive}}\right),$$

where $\rho_i$ is the correlation between $\nabla_{\lambda_i}[\log q_\lambda(\t)]h_\lambda(\t)$ and $\nabla_{\lambda_i}[\log q_\lambda(\t)]$.
Often, $\rho_i^2$ is very close to 1, which leads to a large variance reduction.

One can estimate the numbers $c_i$ in $\eqref{eq:optimal c_i}$ using samples $\t_s\sim q_{\l}(\t)$.
In order to ensure the unbiasedness of the gradient estimator, the samples used to estimate $c_i$ must be independent of the samples used to estimate the gradient.

In practice, the $c_i$ can be updated sequentially as follows.
At iteration $t$, we use the $c_i$ computed in the previous iteration $t-1$, i.e. based on the samples from $q_{\l^{(t-1)}}(\t)$,
to estimate the gradient $\wh{\nabla_\l\text{LB}}(\l^{(t)})$,
which is computed using new samples from $q_{\l^{(t)}}(\t)$.

We then update the $c_i$ using this new set of samples.
By doing so, the unbiasedness is guaranteed while no extra samples are needed in updating the control variates $c_i$.

[Algorithm 4](#algorithm-4) provides a detailed pseudo-code implementation of the FFVB approach that uses the control variate for variance reduction 
and moving average adaptive learning, and [Algorithm 5](#algorithm-5) implements the FFVB approach that uses the control variate and natural gradient.  
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 4: FFVB with control variates and adaptive learning
{: #algorithm-4}

- **Input:** Initial $\l^{(0)}$, adaptive learning weights $\beta_1,\beta_2\in(0,1)$, fixed learning rate $\eps_0$, threshold $\tau$, rolling window size $t_W$ and maximum patience $P$. **Model-specific requirement**: function $h(\theta):=\log\big(p(\theta)p(y\mid\theta)\big)$.
- **Initialization**
	- Generate $\theta_s\sim q_{\lambda^{(0)}}(\theta)$, $s=1,...,S$.
	- Compute the unbiased estimate of the LB gradient

	  $$\wh{\nabla_\l\text{LB}}(\l^{(0)}):=\frac{1}{S}\sum_{s=1}^S\nabla_\lambda \log q_\lambda(\theta_s)\times h_\lambda(\theta_s)|_{\lambda=\lambda^{(0)}}.$$
	
	- Set $g_0:=\wh{\nabla_\l\text{LB}}(\l^{(0)})$, $v_0:=(g_0)^2$, $\bar g:=g_0$, $\bar v:=v_0$. 
	- Estimate the vector of control variates $c$ as in \eqref{eq:optimal c_i} using the samples $\{\theta_s,s=1,...,S\}$.
	- Set $t=0$, $\text{patience}=0$ and $\texttt{stop=false}$.
- **While** $\texttt{stop=false}$:
    - Generate $\theta_s\sim q_{\lambda^{(t)}}(\theta)$, $s=1,...,S$.
	- Compute the unbiased estimate of the LB gradient
	  
	  $$g_t:=\wh{\nabla_\l\text{LB}}(\l^{(t)})=\frac{1}{S}\sum_{s=1}^S\nabla_\lambda \log q_\lambda(\theta_s)\circ \big(h_\lambda(\theta_s)-c\big)|_{\lambda=\lambda^{(t)}}.$$

	- Estimate the new control variate vector $c$ as in \eqref{eq:optimal c_i} using the samples $\{\theta_s,s=1,...,S\}$.
	- Compute $v_t=(g_t)^2$ and 

	  $$\bar g =\beta_1 \bar g+(1-\beta_1)g_t,\;\;\bar v =\beta_2 \bar v+(1-\beta_2)v_t.$$
	
	- Compute $\alpha_t=\min(\epsilon_0,\epsilon_0\frac{\tau}{t})$ and update
	  
	  $$\l^{(t+1)}=\l^{(t)}+\a_t \bar g/\sqrt{\bar v}$$
    
	- Compute the lower bound estimate
	
	  $$\wh{\text{LB}}(\l^{(t)}):=\frac{1}{S}\sum_{s=1}^S h_{\lambda^{(t)}}(\theta_s).$$
	  \item If $t\geq t_W$: compute the moving averaged lower bound
	  $$\overline {\text{LB}}_{t-t_W+1}=\frac{1}{t_W}\sum_{k=1}^{t_W} \wh{\text{LB}}(\l^{(t-k+1)}),$$
	
	  and if $\overline {\text{LB}}_{t-t_W+1}\geq\max(\overline\LB)$ patience = 0; else $\text{patience}:=\text{patience}+1$.
    - If $\text{patience}\geq P$, $\texttt{stop=true}$.
    - Set $t:=t+1$.

</div>

**Note:** The term $\nabla_\lambda \log q_\lambda(\theta_s)\circ \big(h_\lambda(\theta_s)-c\big)$ should be understood component-wise, i.e. it is the vector whose $i$th element is $\nabla_{\lambda_i} \log q_\lambda(\theta_s)\times \big(h_\lambda(\theta_s)-c_i\big)$.

<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 5: FFVB with control variates and natural gradient
{: #algorithm-5}

- **Input:** Initial $\l^{(0)}$, momentum weight $\alpha_m$, fixed learning rate $\eps_0$, threshold $\tau$, rolling window size $t_W$ and maximum patience $P$. **Model-specific requirement**: function function $h(\theta):=\log\big(p(\theta)p(y\mid\theta)\big)$.
- **Initialization**
	- Generate $\theta_s\sim q_{\lambda^{(0)}}(\theta)$, $s=1,...,S$.
	- Compute the unbiased estimate of the LB gradient

	  $$\wh{\nabla_\l\text{LB}}(\l^{(0)}):=\frac{1}{S}\sum_{s=1}^S\nabla_\lambda \log q_\lambda(\theta_s)\times h_\lambda(\theta_s)|_{\lambda=\lambda^{(0)}}.$$

	  and the natural gradient 

	  $$\wh{\nabla_{\lambda}\LB} (\l^{(0)})^{\text{nat}} := I_F^{-1}(\l^{(0)})\wh{\nabla_\l\LB}(\l^{(0)}).$$
	
	- Set momentum gradient 
	
	{%raw%}
	$$\overline{{\nabla_\l{\LB}}}:=\wh{\nabla_{\lambda}\LB} (\l^{(0)})^{\text{nat}}.$$
	{%endraw%}
	
	- Estimate control variate vector $c$ as in \eqref{eq:optimal c_i} using the samples $\{\theta_s,s=1,...,S\}$.
	- Set $t=0$, $\text{patience} =0$ and $\texttt{stop=false}$.
- **While** $\texttt{stop=false}$:
    - Generate $\theta_s\sim q_{\lambda^{(t)}}(\theta)$, $s=1,...,S$.
	- Compute the unbiased estimate of the LB gradient
	  
	  $$\wh{\nabla_\l\text{LB}}(\l^{(t)})=\frac{1}{S}\sum_{s=1}^S\nabla_\lambda \log q_\lambda(\theta_s)\circ \big(h_\lambda(\theta_s)-c\big )|_{\lambda=\lambda^{(t)}}$$

	  and the natural gradient 

	  $$\wh{\nabla_{\lambda}\LB} (\l^{(t)})^{\text{nat}} = I_F^{-1}(\l^{(t)})\wh{\nabla_\l\LB}(\l^{(t)}).$$

	- Estimate the new control variate vector $c$ as in \eqref{eq:optimal c_i} using the samples $\{\theta_s,s=1,...,S\}$.
	- Compute the momentum gradient

	  {%raw%}$$\overline{{\nabla_\l{\LB}}} = \alpha_\text{m} \overline{{\nabla_\l{\LB}}}+(1-\alpha_\text{m})\wh{\nabla_{\lambda}\LB}(\l^{(t)})^{\text{nat}}.$${%endraw%}
	
	- Compute $\alpha_t=\min(\epsilon_0,\epsilon_0\frac{\tau}{t})$ and update 

	  {%raw%}$$\l^{(t+1)}=\l^{(t)}+\a_t \overline{{\nabla_\l{\LB}}}.$${%endraw%}
    
	- Compute the lower bound estimate
	
	  {%raw%}$$\wh{\text{LB}}(\l^{(t)}):=\frac{1}{S}\sum_{s=1}^S h_{\lambda^{(t)}}(\theta_s).$${%endraw%}

	- If $t\geq t_W$: compute the moving average lower bound
	  
	  {%raw%}$$\overline {\text{LB}}_{t-t_W+1}=\frac{1}{t_W}\sum_{k=1}^{t_W} \wh{\text{LB}}(\l^{(t-k+1)}),$${%endraw%}
	
	  and if $\overline {\text{LB}}_{t-t_W+1}\geq\max(\overline\LB)$ patience = 0; else $\text{patience}:=\text{patience}+1$.
    - If $\text{patience}\geq P$, $\texttt{stop=true}$.
    - Set $t:=t+1$.

</div>

---