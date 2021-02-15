---
layout: default
title: VAR(1) defined as function handle
parent: Code examples
nav_order: 4
permalink: /example/cgvb-var1-function-handle
---

# **Define a VAR(1) model as a function handle** 
{: #example-handler-var1}

This is example shows how to define a VAR(1) model as a function handle to work with VBLab supported VB algorithms. 

[Github code](https://github.com/VBayesLab/VBLab/blob/main/Example/CGVB_VAR1_Function_Handle.m){: .fs-4 .btn .btn-purple}

---

VAR models (vector autoregressive models) are used for multivariate time series. The structure is that each variable is a linear function of past lags of itself and past lags of the other variables.

Let $\{y_t,\ t=1,2,...\}$ be a $m$-dimensional time series. Consider the vector autoregressive model of order 1, denoted as VAR(1), is defined as:

$$y_t = c+ Ay_{t-1}  + u_t, \quad u_t \sim^{iid} \mathcal{N}(0,\Gamma)$$

where $c$ is a $m$-vector and $A$ a $(m\times m)$-matrix of regression coefficients. 
For simplicity, let's assume $\Gamma$ is known for now.
The vector of model parameters is $\theta=(c^\top,\vec(A)^\top)^\top$.
Suppose that the prior is $p(\theta) = \mathcal{N}(0,\sigma_0^2I_d)$ with $I_d$ the identity matrix and $d=m(m+1)$.

Then the $h(\theta)$ term of the VAR(1) model can be derived as

$$\begin{eqnarray}\tag{1}\label{eqn:var1-1}
h(\theta)&=&\log p(\theta)+\log p(y|\theta)\\
&=&-\frac{d}{2}\log(2\pi)-\frac{d}{2}\log(\sigma_0^2)-\frac{\theta^\top\theta}{2\sigma_0^2}-\frac{m(T-1)}{2}\log(2\pi)-\frac{T-1}{2}\log|\Gamma|\\
&-&\frac{1}{2} \sum_{t=2}^{T} \left((y_t-Ay_{t-1}-c)^\top \Gamma^{-1} (y_t-A y_{t-1}-c)\right). \end{eqnarray}$$ 

and the $\nabla_\theta h(\theta)$ term can be written as

$$\begin{eqnarray}\tag{2}\label{eqn:var1-2}
\nabla_\theta h(\theta) = \begin{pmatrix}
\nabla_c h(\theta)\\
\nabla_{\text{vec}(A)}h(\theta)
\end{pmatrix}
\end{eqnarray}$$

with

$$\tag{3}\label{eqn:var1-3}\nabla_c h(\theta) = -\frac{1}{\sigma_0^2}c+\Gamma^{-1}\big(\sum_{t=2}^T (y_t-A y_{t-1}-c)\big) $$

and

$$\tag{4}\label{eqn:var1-4}\nabla_{\text{vec}(A)}h(\theta) = -\frac{1}{\sigma_0^2}\vec(A)+\sum_{t=2}^T y_{t-1}\otimes \big(\Gamma^{-1} (y_t-A y_{t-1}-c)\big).$$

Given the equations to compute the $h(\theta)$ and $\nabla_\theta h(\theta)$ terms of the VAR(1) model, we will define a function, following the discussed template, to define the VAR(1) model that can work with VBLab supported VB algorithms. 