---
layout: default
title: Example
parent: VB Tutorials
nav_order: 4
permalink: /tutorial/example
---

# **Tutorial examples**
{: .fs-8 }

Examples to illustrate the VB algorithms discussed in the tutorial paper. 

---
## Example 2.1: Mean Field Variational Bayes for Linear Regression
{: #example2-1}
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
\newcommand{\eps}{\epsilon}$
**Given:**
- $ y=(11; 12; 8; 10; 9; 8; 9; 10; 13; 7)$ be observations from $\N(\mu,\s^2)$, the normal distribution with mean $\mu$ and variance $\s^2$. 
- Suppose that we use the prior $\N(\mu_0,\s_0^2)$ for $\mu$ and $\text{Inverse-Gamma}(\a_0,\b_0)$ for $\s^2$, with hyperparameters $\mu_0=0$, $\s_0=10$, $\a_0=1$ and $\b_0=1$. 
- Assume the VB factorization $q(\mu,\sigma^2)=q(\mu)q(\sigma^2)$.