---
layout: default
title: MFVB for elaborate models
parent: Mean Field Variational Bayes
grand_parent: VB Tutorials
nav_order: 2
permalink: /tutorial/mfvb/elaborate-models
---

# **MFVB: Elaborate Models**
{: .fs-8 }

This tutorial gives a quick introduction to Mean Field Variational Bayes for elaborate models.

**See:** [Variational Bayes Introduction]({{site.baseurl}}{% link Tutorial-VB.md%}), [Mean Field Variational Bayes]({{site.baseurl}}{% link Tutorial-MFVB.md%})

---
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
One of the difficulties in using MFVB is that the optimal variational distributions in [(9)](/VBLabDocs/tutorial/example#mjx-eqn-MFVB-9) sometimes do not admit a standard form.

In [Example 2.1](/VBLabDocs/tutorial/example#example2-1), for example, if the data $y_i$ does not follow a normal distribution but a Student's $t$ distribution $t_\nu(\mu,\sigma^2)$,
then it can be seen that the optimal variational distribution $q(\mu)$ does not have the form of a Gaussian distribution or any standard probability distribution.

In some situations, however, by introducing auxiliary variables, we can equivalently represent the model by augmenting the parameter space such that MFVB is applicable.
The use of auxiliary variables to facilitate statistical computations is widely used in many areas of statistics.

The term *elaborate model* to refer to a statistical model  in which its prior or its data density can be augmented using auxiliary variables such that the optimal variational distributions [(9)](/VBLabDocs/tutorial/example#mjx-eqn-MFVB-9) admit a standard form.
Introducing auxiliary variables makes MFVB tractable, but this might come at the price of reducing the variational approximation accuracy; however, we won't discuss this issue in any detail in this tutorial.

More precisely, consider the standard Bayesian model

$$y |\theta\sim p(y |\theta),\;\;\quad\quad \theta\sim p(\theta)\tag{10}\label{eq:ChaperMFVB:model 1}.$$

Suppose that there exists an auxiliary variable $\eta$ such that

$$\tag{11}p(y|\theta) = \int p(y|\theta,\eta) p(\eta |\theta)\d\eta,$$

then model \eqref{eq:ChaperMFVB:model 1} can be equivalently represented as

$$\tag{12}\label{eq:ChaperMFVB:model 2}y|\theta,\eta\sim p(y|\theta,\eta),\;\;\quad\quad \eta|\theta\sim p(\eta|\theta),\;\;\quad\quad
\theta\sim p(\theta).$$

The model \eqref{eq:ChaperMFVB:model 1} is said to be elaborate if it can be presented as 
the hierarchical model \eqref{eq:ChaperMFVB:model 2} and, under the variational factorization $q(\theta,\eta)=q(\theta)q(\eta)$, the optimal variational distributions $q(\theta)$ and $q(\eta)$ in [(9)](/VBLabDocs/tutorial/example#mjx-eqn-MFVB-9) admit a standard form. The idea of elaborate models applies to the prior too, in which one can represent the prior $p(\theta)$ in a hierarchical form using auxiliary variables.

We now demonstrate this idea in the Bayesian Lasso model. Consider the linear regression problem

$$
y =\mu 1_n +X\beta+\epsilon,
$$ 

where $y$ is the vector of responses, $X$ is the $n \times p$ matrix of covariates,
$1_n$ is the $n \times 1$ vector of 1s, and $\epsilon$ is the vector of i.i.d. normal errors $\N(0,\sigma^2)$. Without loss of generality, we assume that $y$ and $X$ have been centered so that $\mu$ is zero and omitted from the model.

Regression analysis is often concerned with estimating $\beta=(\beta_1,...,\beta_p)^\top$ and simultaneously identifying the important covariates.
The least absolute shrinkage and selection operator (Lasso) method solves this problem by minimizing the sum of squared errors and a regularization term

$$
\tag{13}\label{eq:ChapterMFVB:lasso}
\underset{\beta}{\mbox{min}} ~ \left \{ (y-X\beta)'(y-X\beta) + \wt\lambda \sum_{j=1}^p | \beta_j | \right \},
$$

where $\wt\lambda >0$ is the tuning parameter controlling the amount of regularization. 
The Lasso estimator, i.e. the solution of \eqref{eq:ChapterMFVB:lasso}, can be interpreted as the posterior mode in a Bayesian context
where a conditional Laplace prior is used for $\beta$

$$\tag{14}\label{eq:ChapterMFVB:prior}
p(\beta|\sigma^2) = \prod_{j=1}^p \frac{\lambda}{2\sqrt{\sigma^2}} e^{-\lambda|\beta_j|/\sqrt{\sigma^2}},$$

for some shrinkage parameter (this parameter shouldn't be confused with the variational parameter $\lambda$ in [FFVB]({% link Tutorial-FFVB.md %}) section) $\lambda$.
The posterior mode of $\beta$ is the Lasso estimator in \eqref{eq:ChapterMFVB:lasso} with $\wt\lambda=2\sqrt{\sigma^2}\lambda$.

It is difficult to use MFVB for approximating the posterior $p(\beta,\sigma^2|X,y)$ in this case,
as the optimal conditional variational distribution of $\beta$ does not admit a standard form.
However, it turns out that we can use auxiliary variables to make this Bayesian model elaborate and overcome the aforementioned difficulty.

It is well-known that a Laplace distribution can be represented as a mixture of normal and exponential distributions as follows

$$
\frac{\lambda}{2} e^{-\lambda|z|}
=\int_0^\infty \frac{1}{\sqrt{2\pi s}}e^{-z^2/(2s)}\frac{\lambda^2}{2}
e^{-\lambda^2s/2}\d s.
$$

Using this representation, after some algebra, we have that

$$\frac{\lambda}{2\sqrt{\sigma^2}} e^{-\lambda \mid \beta_j \mid /\sqrt{\sigma^2}}=\int_0^\infty \frac{1}{\sqrt{2\pi\sigma^2\tau}}e^{-\beta_j^2/({2\sigma^2\tau})}\frac{\lambda^2}{2}
e^{-\lambda^2\tau/2}\d \tau.$$

This motivates the following hierarchical representation of the Bayesian Lasso model

$$\begin{equation}\tag{15}\label{eq:ChapterMFVB:tau prior}\begin{aligned}
y \mid X,\beta,\sigma^2 &\sim  \N(X\beta,\sigma^2 I_n),\\
\beta_j \mid \sigma^2,\tau_j &\sim  \N (0,\sigma^2\tau_j),\\
\tau_j &\sim  \text{Exp}\big(\frac{\lambda^2}{2}\big) = \frac{\lambda^2}{2}e^{-\lambda^2\tau_j/2},\;\;\;j=1,...,p.
\end{aligned}\end{equation}$$

The conjugate prior for $\sigma^2$ is inverse Gamma and we use the improper prior $p(\sigma^2)\propto 1/\sigma^2$ in this example.
The shrinkage parameter $\lambda$ can be selected in some way, here we use a full Bayesian treatement and put a Gamma prior on $\lambda^2$

$$\tag{16}\label{eq:ChapterMFVB:prior lambda}
p(\lambda^2)=\frac{\delta^r}{\Gamma(r)}(\lambda^2)^{r-1}e^{-\delta\lambda^2},$$

with $r$ and $\delta$ hyperparameters and pre-specified.
Note that we use a prior for $\lambda^2$, not $\lambda$, as this leads to a tractable form for the optimal conditional variational distribution for $\lambda^2$.

The model parameters include $\beta$, $\tau=(\tau_1,...,\tau_p)^\top$, $\sigma^2$ and $\lambda^2$.
Let us use the following mean field variational distribution

$$q(\beta,\tau,\sigma^2,\lambda^2)=q(\beta)q(\tau)q(\sigma^2)q(\lambda^2).$$

With this factorization, all the optimal conditional variational distributions admit a standard form.
The optimal variational distribution for $\beta$ is $\N(\mu_\beta,\Sigma_\beta)$ with

$$\mu_\beta=\big(X^\top X+D_\tau\big)^{-1}X^\top y,\;\;\;\;\Sigma_\beta=\big(X^\top X+D_\tau\big)^{-1}/\E_q(\frac{1}{\sigma^2}),$$

where $D_\tau:=\text{diag}\big(\E_q(1/\tau_1),\cdots,\E_q(1/\tau_p)\big)$. Here $\E_q(\cdot)$ denotes expectation with respect to the variational distribution $q$.

The optimal variational distributions for $\tau_j$ are independent of each other,
where $\wt\tau_j:= 1/\tau_j$ follows an inverse-Gaussian with location and scale parameters

$$\mu_{\wt\tau_j} = \Big(\frac{\E_q(\lambda^2)}{\E_q\big(\beta_j^2/\sigma^2\big)}\Big)^{1/2},\;\;\;\;\lambda_{\wt\tau_j} = \E_q(\lambda^2).$$

The optimal distribution for $\sigma^2$ is inverse Gamma with the parameters

$$\alpha_{\sigma^2}=\frac12(n+p),\;\;\;\;\beta_{\sigma^2}=\frac12\E_q|y-X\beta|^2+\frac12\sum_{j=1}^p\E_q\big(\frac{\beta_j^2}{\tau_j}\big).$$

Finally, the optimal variatinoal distribution for $\lambda^2$ is Gamma with

$$\alpha_{\lambda^2}= r+1,\;\;\;\; \beta_{\lambda^2} = \delta+\frac12\sum_j\E_q(\tau_j).$$

Using the results regarding to the moments of these standard distributions, we have

$$\begin{aligned}
\E_q\big(\frac{1}{\tau_j}\big)&=\E_q(\wt\tau_j)=\mu_{\wt\tau_j},&\E_q(\tau_j)&=\E_q\big(\frac{1}{\wt\tau_j}\big)=\frac{1}{\mu_{\wt\tau_j}}+\frac{1}{\lambda_{\wt\tau_j}},\\
\E_q\big(\frac{1}{\sigma^2}\big)&=\frac{\alpha_{\sigma^2}}{\beta_{\sigma^2}},&\E_q\big(\lambda^2\big)&=\frac{\alpha_{\lambda^2}}{\beta_{\lambda^2}},\\
\E_q(\beta_j^2)&=\mu_{\beta,j}^2+\Sigma_{\beta,jj},
\end{aligned}$$

where $\mu_{\beta,j}$ is the $j$th element of vector $\mu_{\beta}$ and $\Sigma_{\beta,jj}$ is the $(j,j)$ element of matrix $\Sigma_{\beta}$.
We arrive at the MFVB procedure for Bayesian inference in the Bayesian Lasso model.
<br>
<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 2: MFVB for Bayesian Lasso
Initialize $\alpha_{\sigma^2}$, $\beta_{\sigma^2}$, $\mu_{\wt\tau_j}$ and $\lambda_{\wt\tau_j}$, $j=1,...,p$, then update the following until convergence:
- **Step 1:** Update $\mu_\beta$ and $\Sigma_\beta$
$$\mu_\beta=\big(X^\top X+D_\tau\big)^{-1}X^\top y,\;\;\;\;\Sigma_\beta=\frac{\beta_{\sigma^2}}{\alpha_{\sigma^2}}\big(X^\top X+D_\tau\big)^{-1},$$
where $D_\tau:=\text{diag}\big(\mu_{\wt\tau_1},\cdots,\mu_{\wt\tau_p}\big)$. 
- **Step 2:** Update $\alpha_{\lambda^2}$ and $\beta_{\lambda^2}$ 
$$\alpha_{\lambda^2}= r+1,\;\;\;\; \beta_{\lambda^2} = \delta+\frac12\sum_j\Big(\frac{1}{\mu_{\wt\tau_j}}+\frac{1}{\lambda_{\wt\tau_j}}\Big).$$
- **Step 3:** Update $\mu_{\wt\tau_j}$ and $\lambda_{\wt\tau_j}$, $j=1,...,p$
$$\mu_{\wt\tau_j} = \Big(\frac{\alpha_{\lambda^2}/\beta_{\lambda^2}}{\big(\alpha_{\sigma^2}/\beta_{\sigma^2}\big)\big(\mu_{\beta,j}^2+\Sigma_{\beta,jj}\big)}\Big)^{1/2},\;\;\;\;\lambda_{\wt\tau_j} = \frac{\alpha_{\lambda^2}}{\beta_{\lambda^2}}.$$
- **Step 4:** Update $\alpha_{\sigma^2}$ and $\beta_{\sigma^2}$
$$\alpha_{\sigma^2}=\frac12(n+p),\;\;\;\;\beta_{\sigma^2}=\frac12 \mid y-X\mu_{\beta} \mid ^2+\frac12\text{tr}(X\Sigma_\beta X^\top)+\frac12\sum_{j=1}^p(\mu_{\beta,j}^2+\Sigma_{\beta,jj})\mu_{\wt\tau_j}.$$

</div>
---

**Next:** [Fixed Form Variational Bayes (FFVB)]({{site.baseurl}}{% link Tutorial-FFVB.md%})