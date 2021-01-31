---
layout: default
title: Gaussian VB - VAFC
parent: Fixed Form Variational Bayes
grand_parent: VB Tutorials
nav_order: 4
permalink: /tutorial/ffvb/vafc
---

# **GVB with factor decomposed covariance (VAFC)**
{: .fs-8 }

This tutorial gives a quick introduction to Mean Field Variational Bayes. 
{: .fs-6 .fw-300 }

**See:** [Variational Bayes Introduction]({% link Tutorial-VB.md%}), [Fixed Form Variational Bayes]({% link Tutorial-FFVB.md%})

---
## VAFC
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
\def\diag{\text{diag}}$
<!-- End -->
An alternative to the Cholesky decomposition is the factor decomposition $\Sigma=BB^\top+C^2,$
where $B$ is the factor loading matrix of size $d\times f$ with $f\ll d$ the number of factors and $C$ is a diagonal matrix, $C=\text{diag}(c_1,...,c_d)$.

GVB with this factor covariance structure is useful in high-dimensional settings where $d$ is large,
as the number of variational parameters reduces from $d+d*(d+1)/2$ in the case of full Gaussian to $(f+2)d$ in the case of factor decomposition.

This VB method is first developed in [Ong et. al (2018)](#Ong-2018) who term the method Variational Approximation with Factor Covariance (VAFC)
and use Algorithm \ref{algorithm 4} for training, as computing the natural gradient in this case is difficult.

This section describes the case with one factor, $f=1$, which achieves a great computational speed-up for approximate Bayesian inference in big models such as deep neural networks where $d$ can be very large.
Also, with $f=1$, Tran et. al (2020) show that it is possible to calculate the natural gradient efficiently   and term their method NAtural gradient Gaussian Variational Approximation with factor Covariance (NAGVAC).


With $f=1$, we rewrite the factor decomposition as 
$$\Sigma=bb^\top+C^2,\quad\;\;C=\text{diag}(c),$$  
where $b=(b_1,...,b_d)^\top$ and $c=(c_1,...,c_d)^\top$ are vectors. The variational parameter vector is $\lambda=(\mu^\top,b^\top,c^\top)^\top$.
Using the reparameterization trick, $\theta\sim\N(\mu,\Sigma)$ can be written as
$$\theta = g(\lambda,\veps)=\mu+\veps_1b+c\circ\veps_2$$
where $\veps=(\veps_1,\veps_2^\top)^\top\sim\N_{d+1}(0,I)$, and $c\circ\veps_2$ denotes the component-wise product of vectors $c$ and $\veps_2$.
Note that
$$\nabla_\mu g(\lambda,\veps)=I_d,\;\;\;\;\nabla_b g(\lambda,\veps)=\veps_1I_d,\;\;\;\;\nabla_c g(\lambda,\veps) =\diag(\veps_2),$$

hence the reparameterization gradient is

$$\tag{30}\label{eq: GVB factor grad est}
\nabla_\l\LB(\l)=\E_{q_\veps}\begin{pmatrix}\nabla_\theta h_\lambda(\mu+\veps_1b+c\circ\veps_2)\\
\veps_1 \nabla_\theta h_\lambda(\mu+\veps_1b+c\circ\veps_2)\\
\veps_2 \circ \nabla_\theta h_\lambda(\mu+\veps_1b+c\circ\veps_2)
\end{pmatrix}.$$  

The gradient of function $h_\lambda(\theta)$ is
$$\nabla_\theta h_\lambda(\theta)=\nabla_\theta h(\theta)-\nabla_\theta\log q_\lambda(\theta)=\nabla_\theta\log\big(p(\theta)p(y|\theta)\big)-\nabla_\theta\log q_\lambda(\theta),$$
where the first term is model-specific and the second term is $\nabla_\theta\log q_\lambda(\theta)=-\Sigma^{-1}(\theta-\mu)$.

To avoid computing directly the inverse $\Sigma^{-1}$ and the matrix-vector multiplication, noting that $\Sigma^{-1}=C^{-2}-\frac{1}{1+b^\top C^{-2}b} C^{-2}bb^\top C^{-2}$, we have
$$\nabla_\theta\log q_\lambda(\theta)=-(\theta-\mu)\circ c^{-2}+\frac{(b\circ c^{-2})^\top(\theta-\mu)}{1+(b\circ c^{-1})^\top (b\circ c^{-1})}(b\circ c^{-2}),$$
with $c^{-1}:=(1/c_1,...,1/c_d)^\top$ and $c^{-2}:=(1/c_1^2,...,1/c_d^2)^\top$.
To compute lower bound estimates, we need
$$\log q_\lambda(\theta) = -\frac{d}{2}\log(2\pi)-\frac12\log|\Sigma|-\frac12(\theta-\mu)^\top\Sigma^{-1}(\theta-\mu).$$
As $\Sigma=C\big((C^{-1}b)(C^{-1}b)^\top+I\big)C$,
$$|\Sigma| = |C|^2\big(1+(C^{-1}b)^\top(C^{-1}b)\big)=\big(\prod_{i=1}^d{c_i^2}\big)\big(1+\sum_{i=1}^d\frac{b_i^2}{c_i^2}\big).$$
Hence, a computationally efficient version of $\log q_\lambda(\theta)$ is

$$\begin{eqnarray}
\log q_\lambda(\theta) &=& -\frac{d}{2}\log(2\pi)-\frac12\sum_{i=1}^d\log c_i^2-\frac12\log\big(1+\sum_{i=1}^d\frac{b_i^2}{c_i^2}\big)\\
&&\phantom{ccc}-\frac12(\theta-\mu)^\top\big((\theta-\mu)\circ c^{-2}\big)+\frac{\big((b\circ c^{-2})^\top(\theta-\mu)\big)^2}{2\big(1+(b\circ c^{-1})^\top(b\circ c^{-1})\big)}.
\end{eqnarray}$$

Finally, it can be shown that the natural gradient in $\eqref{eq:natural gradient}$ can be approximately computed in closed form 
as in the following algorithm (see [Tran et al. (2020)](#Tran-2020)), whose computational complexity is $O(d)$.

<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 8: Computing the natural gradient
{: #algorithm-8}
<br>
**Input:** Vector $b$, $c$ and ordinary gradient of the lower bound $g = (g_1^\top,g_2^\top,g_3^\top)^\top$ with $g_1$ the vector formed by the first $d$ elements of $g$,
$g_2$ formed by the next $d$ elements, and $g_3$ the last $d$ elements. 
**Output:** The natural gradient $g^{\text{nat}}=I_F^{-1}g$.
- Compute the vectors $v_1=c^2-2b^2\circ c^{-4}$, $v_2=b^2\circ c^{-3}$, and the scalars $\kappa_1=\sum_{i=1}^db_i^2/c_i^2$, $\kappa_2=\frac{1}{2}(1+\sum_{i=1}^d v_{2i}^2/v_{1i})^{-1}$.
- Compute 

$$g^{\text{nat}}=\begin{pmatrix}
(g_1^\top b)b+c^2\circ g_1\\
\frac{1+\kappa_1}{2\kappa_1}\Big((g_2^\top b)b+c^2\circ g_2\Big)\\
\frac12v_1^{-1}\circ g_3+\kappa_2 \big[(v_1^{-1}\circ v_2)^\top g_3\big](v_1^{-1}\circ v_2)
\end{pmatrix}.$$

</div>
---

## NAGVAC

We now describe the NAGVAC algorithm that can be used as a fast VB method for approximate Bayesian inference 
in high-dimensional applications such as Bayesian deep neural networks.
In such applications, instead of using the lower bounds for stopping rule,
one often uses a loss function evaluated on a validation dataset for stopping.
Then, the updating is stopped if the loss function is not decreased after $P$ iterations.

<div class="code-example" markdown="1" style="background-color:GhostWhite;padding:20px;">

### Algorithm 9: NAGVAC
{: #algorithm-9}
<br>
**Input:** Initial $\lambda^{(0)}:=(\mu^{(0)},b^{(0)},c^{(0)})$, number of samples $S$, momentum weight $\a_m$, fixed learning rate $\eps_0$, threshold $\tau$, rolling window size $t_W$ and maximum patience $P$. **Model-specific requirement:** function $h(\theta)$ and $\nabla_\theta h(\theta)$.

- Initialization
	- Generate $\varepsilon_{1,s}\sim \N(0,1)$ and $\varepsilon_{2,s}\sim \N_d(0,I_d)$, $s=1,...,S$.
	- Compute the lower bound gradient estimate $\wh{\nabla}_\l\LB(\l^{(0)})$ as in then compute 
	- Set momentum gradient $\overline{\nabla_\l\LB}:=\wh{\nabla_{\lambda}\LB} (\l^{(0)})^{\text{nat}}$.
	- Set $t=0$, $\text{patience}=0$ and $\texttt{stop=false}$.
- While $\texttt{stop=false}:$
	- Generate $\varepsilon_{1,s}\sim \N(0,1)$ and $\varepsilon_{2,s}\sim \N_d(0,I_d)$, $s=1,...,S$.	
	- Compute the lower bound gradient estimate $\wh{\nabla}_\l\LB(\l^{(t)})$, and then compute the natural gradient 
	- Compute the momentum gradient
	
    - Compute  and update 
	
	- Compute the validation loss $\text{Loss}(\lambda^{(t)})$. 
        - If $\text{Loss}(\lambda^{(t)})\leq \min\{\text{Loss}(\lambda^{(1)}),...,\text{Loss}(\lambda^{(t-1)})\}$ patience = 0; 
        - else $\text{patience}:=\text{patience}+1$.
    - If $\text{patience}\geq P$, $\texttt{stop=true}$.
    - Set $t:=t+1$.

</div>

---

## Reference
Tran, M.-N., Nguyen, T.-N., Nott, D., and Kohn, R. (2020). Bayesian deep net GLM and GLMM. *Journal of Computational and Graphical Statistics*, 29(1):97-113. [Read the paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747)
{: #Tran-2020}

Ong, V. M.-H., Nott, D. J., and Smith, M. S. (2018). Gaussian variational approximation with a factor covariance structure. *Journal of Computational and Graphical Statistics*, 27(3):465-478. [Read the paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2017.1390472?journalCode=ucgs20)
{: #Ong-2018}
