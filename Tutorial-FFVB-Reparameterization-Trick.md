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

**See:** [Variational Bayes Introduction]({% link Tutorial-VB.md%}), [Fixed Form Variational Bayes]({% link Tutorial-FFVB.md%})

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
The reparameterization trick is an attractive alternative to the [control variate]({%link Tutorial-FFVB-Control-Variate.md %}) method.
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

Algorithm \ref{algorithm 4} provides a detailed implementation of the FFVB approach that uses the reparameterization trick and adaptive learning.
A small modification of Algorithm \ref{algorithm 4} (not presented) gives the implementation of the FFVB approach that uses the reparameterization trick and natural gradient.
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

\begin{algorithm}[FFVB with reparameterization trick and adaptive learning]\label{algorithm 4} 
{\bf Input}: Initial $\l^{(0)}$, adaptive learning weights $\beta_1,\beta_2\in(0,1)$, fixed learning rate $\eps_0$, threshold $\tau$, rolling window size $t_W$ and maximum patience $P$. {\bf Model-specific requirement}: function $h(\theta):=\log\big(p(\theta)p(y|\theta)\big)$ and its gradient $\nabla_\theta h(\theta)$.
\begin{itemize}
  \item Initialization
  \begin{itemize}
	  \item Generate $\varepsilon_s\sim p_{\veps}(\cdot)$, $s=1,...,S$.
	  \item Compute the unbiased estimate of the LB gradient
	  \[\wh{\nabla_\l\text{LB}}(\l^{(0)}):=\frac1S\sum_{s=1}^S\nabla_\l g(\l,\veps_s)^\top \nabla_\t \big\{h_\lambda(g(\l,\veps_s))\big\}\big|_{\lambda=\lambda^{(0)}}.\]
	  \item Set $g_0:=\wh{\nabla_\l\text{LB}}(\l^{(0)})$, $v_0:=(g_0)^2$, $\bar g:=g_0$, $\bar v:=v_0$. 
	  \item Set $t=0$, $\text{patience}=0$ and \texttt{stop=false}.
  \end{itemize}
  \item While \texttt{stop=false}:
  \begin{itemize}
	  \item Generate $\veps_s\sim p_{\veps}(\cdot)$, $s=1,...,S$
	  \item Compute the unbiased estimate of the LB gradient
\[g_t:=\wh{\nabla_\l\text{LB}}(\l^{(t)})=\frac1S\sum_{s=1}^S\nabla_\l g(\l,\veps_s)^\top \nabla_\t \big\{h_\lambda(g(\l,\veps_s))\big\}\big|_{\lambda=\lambda^{(t)}}.\]
	  \item Compute $v_t=(g_t)^2$ and 
	  \[\bar g =\beta_1 \bar g+(1-\beta_1)g_t,\;\;\bar v =\beta_2 \bar v+(1-\beta_2)v_t.\]
	  \item Compute $\alpha_t=\min(\varepsilon_0,\varepsilon_0\frac{\tau}{t})$ and update
	  \[\l^{(t+1)}=\l^{(t)}+\a_t \bar g/\sqrt{\bar v}\]
	  \item Compute the lower bound estimate
	  \[\wh{\text{LB}}(\l^{(t)}):=\frac{1}{S}\sum_{s=1}^S h_{\lambda^{(t)}}(\theta_s),\;\;\;\theta_s=g(\lambda^{(t)},\veps_s).\]
	  \item If $t\geq t_W$: compute the moving average lower bound
	  \[\overline {\text{LB}}_{t-t_W+1}=\frac{1}{t_W}\sum_{k=1}^{t_W} \wh{\text{LB}}(\l^{(t-k+1)}),\]
	  and if $\overline {\text{LB}}_{t-t_W+1}\geq\max(\overline\LB)$ patience = 0; else $\text{patience}:=\text{patience}+1$.
\item If $\text{patience}\geq P$, \texttt{stop=true}.
\item Set $t:=t+1$.
  \end{itemize}
\end{itemize}
\end{algorithm}

</div>

As is typical of stochastic optimization algorithms,
the performance of Algorithm \ref{algorithm 1} depends greatly on the variance of the noisy gradient.
Variance reduction for the noisy gradient is a key ingredient in FFVB algorithms.   

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

$$\V\left(\wh{\nabla_{\lambda_i}\LB}(\l)\right)=\V\left(\wh{\nabla_{\lambda_i}\LB}(\l)^{\text{naive}}\right)(1-\rho^2_i) \leq \V\left(\hat{\nabla_{\lambda_i}\LB}(\l)^{\text{naive}}\right)$$,

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

Algorithm \ref{algorithm 2} provides a detailed pseudo-code implementation of the FFVB approach that uses the control variate for variance reduction 
and moving average adaptive learning,
and Algorithm \ref{algorithm 3} implements the FFVB approach that uses the control variate and natural gradient.  
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm [FFVB with control variates and adaptive learning]\label{algorithm 2} 

{\bf Input}: Initial $\l^{(0)}$, adaptive learning weights $\beta_1,\beta_2\in(0,1)$, fixed learning rate $\eps_0$, threshold $\tau$, rolling window size $t_W$ and maximum patience $P$. {\bf Model-specific requirement}: function $h(\theta):=\log\big(p(\theta)p(y|\theta)\big)$.
\begin{itemize}
  \item Initialization
  \begin{itemize}
	  \item Generate $\theta_s\sim q_{\lambda^{(0)}}(\theta)$, $s=1,...,S$.
	  \item Compute the unbiased estimate of the LB gradient
	  \[\wh{\nabla_\l\text{LB}}(\l^{(0)}):=\frac{1}{S}\sum_{s=1}^S\nabla_\lambda \log q_\lambda(\theta_s)\times h_\lambda(\theta_s)|_{\lambda=\lambda^{(0)}}.\]
	  \item Set $g_0:=\wh{\nabla_\l\text{LB}}(\l^{(0)})$, $v_0:=(g_0)^2$, $\bar g:=g_0$, $\bar v:=v_0$. 
	  \item Estimate the vector of control variates $c$ as in \eqref{eq:optimal c_i} using the samples $\{\theta_s,s=1,...,S\}$.
	  \item Set $t=0$, $\text{patience}=0$ and \texttt{stop=false}.
  \end{itemize}
  \item While \texttt{stop=false}:
  \begin{itemize}
	  \item Generate $\theta_s\sim q_{\lambda^{(t)}}(\theta)$, $s=1,...,S$.
	  \item Compute the unbiased estimate of the LB gradient\footnote{The term $\nabla_\lambda \log q_\lambda(\theta_s)\circ \big(h_\lambda(\theta_s)-c\big)$ should be understood component-wise, i.e. it is the vector whose $i$th element is $\nabla_{\lambda_i} \log q_\lambda(\theta_s)\times \big(h_\lambda(\theta_s)-c_i\big)$.}
	  \[g_t:=\wh{\nabla_\l\text{LB}}(\l^{(t)})=\frac{1}{S}\sum_{s=1}^S\nabla_\lambda \log q_\lambda(\theta_s)\circ \big(h_\lambda(\theta_s)-c\big)|_{\lambda=\lambda^{(t)}}.\]
	  \item Estimate the new control variate vector $c$ as in \eqref{eq:optimal c_i} using the samples $\{\theta_s,s=1,...,S\}$.
	  \item Compute $v_t=(g_t)^2$ and 
	  \[\bar g =\beta_1 \bar g+(1-\beta_1)g_t,\;\;\bar v =\beta_2 \bar v+(1-\beta_2)v_t.\]
	  \item Compute $\alpha_t=\min(\epsilon_0,\epsilon_0\frac{\tau}{t})$ and update
	  \[\l^{(t+1)}=\l^{(t)}+\a_t \bar g/\sqrt{\bar v}\]
	  \item Compute the lower bound estimate
	  \[\wh{\text{LB}}(\l^{(t)}):=\frac{1}{S}\sum_{s=1}^S h_{\lambda^{(t)}}(\theta_s).\]
	  \item If $t\geq t_W$: compute the moving averaged lower bound
	  \[\overline {\text{LB}}_{t-t_W+1}=\frac{1}{t_W}\sum_{k=1}^{t_W} \wh{\text{LB}}(\l^{(t-k+1)}),\]
	  and if $\overline {\text{LB}}_{t-t_W+1}\geq\max(\overline\LB)$ patience = 0; else $\text{patience}:=\text{patience}+1$.
\item If $\text{patience}\geq P$, \texttt{stop=true}.
\item Set $t:=t+1$.
  \end{itemize}
\end{itemize}
\end{algorithm}

</div>

\begin{example}\label{exa:Example 2}
With the model and data in Example \ref{exa:Example 1}, let's derive a FFVB procedure for  
approximating the posterior $p(\mu,\s^2| y)\propto p(\mu)p(\s^2)p(y|\mu,\s^2)$ using Algorithm \ref{algorithm 2}. 
Suppose that the VB approximation is $q_\l(\mu,\sigma^2)=q(\mu)q(\sigma^2)$ with $q(\mu)=\N(\mu_\mu,\sigma_\mu^2)$ and $q(\sigma^2)=\text{Inverse-Gamma}(\a_{\sigma^2},\b_{\sigma^2})$.
This toy example is simply to demonstrate the use of Algorithm \ref{algorithm 2}, we do not
focus on the approximation accuracy here.

The model parameter is $\theta=(\mu,\sigma^2)^\top$ and the variational parameter $\lambda=(\mu_\mu,\sigma_\mu^2,\a_{\sigma^2},\b_{\sigma^2})^\top$.
In order to implement Algorithm \ref{algorithm 2}, we need $h_\lambda(\theta)=h(\theta)-\log q_\l(\t)$ with 
\bean
h(\theta)&=&\log \big(p(\mu)p(\sigma^2)p(y|\mu,\sigma^2)\big)\\
&=&-\frac{n+1}{2}\log(2\pi)-\frac12\log(\sigma_0^2)-\frac{(\mu-\mu_0)^2}{2\sigma_0^2}+\alpha_0\log(\beta_0)-\log\Gamma(\a_0)-(\frac{n}{2}+\a_0+1)\log(\s^2)\\
&&\phantom{cccc}-\frac{\b_0}{\s^2}-\frac{1}{2\s^2}\sum_{i=1}^n(y_i-\mu)^2,\\
\log q_\l(\t)&=&\a_{\s^2}\log\b_{\s^2}-\log\Gamma(\a_{\s^2})-(\a_{\s^2}+1)\log\s^2-\frac{\b_{\s^2}}{\s^2}-\frac12\log(2\pi)-\frac12\log(\s_{\mu}^2)-\frac{(\mu-\mu_\mu)^2}{2\s_\mu^2},
\eean
and
\[\nabla_\l\log q_\l(\t)=\Big(\frac{\mu-\mu_\mu}{\s_\mu^2},-\frac{1}{2\s_\mu^2}+\frac{(\mu-\mu_\mu)^2}{2\s_\mu^4},\log\b_{\s^2}-\frac{\Gamma'(\a_{\s^2})}{\Gamma(\a_{\s^2})}-\log\s^2,\frac{\a_{\s^2}}{\b_{\s^2}}-\frac{1}{\s^2} \Big)^\top.\]



---