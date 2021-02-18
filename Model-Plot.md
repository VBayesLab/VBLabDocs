---
layout: default
title: vbayesPlot
parent: Utilities Reference
nav_order: 7
permalink: /utilities/vbayesPlot/
block_color: GhostWhite
---

# <samp>vbayesPlot</samp> method [Github code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/Utilities/vbayesPlot.m){: .fs-4 .btn .btn-purple  .float-right}
{: .fs-8 }

Plot analytic figures for VBLab fitted models
{: .fs-6 .fw-300 }

---

## Syntax

```matlab
vbayesPlot(type,Name,Value)
```
---
## Description
`vbayesPlot(Type,Name,Value)` returns [DeepGLM model object](#deepglm-object) `mdl` given neural network structure `Network`. `Name` and `Value` specifie additional options using one or more name-value pair arguments. For example, users can specify the activation function or distribution of the output. 

đsdsd

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--type-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">type</span> - Type of plot </header>
#### Data type: string
<br>

- `'Distribution'`
- `'Interval'`

</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<!--Name-Value Pairs-->
<span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Name-Value Pair Arguments </span>

Specify optional comma-separated pairs of `Name,Value` arguments. `Name` is the argument name and `Value` is the corresponding value. `Name` must appear inside quotes. You can specify several name and value pair arguments in any order as `Name1,Value1,...,NameN,ValueN`.

**Example:** `'Distribution','normal','Activation','relu'` specifies that the distribution of the response is normal, and the activation function of hidden layers is the Rectified Linear Unit (ReLU) function.

<!--Distibution-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Distribution'</span> - Distribution of the response variable</h3></header>

#### Data Type: String 
<br>
Distribution of the response variable, specified as the comma-separated pair consisting of <samp>'Distribution'</samp> and one of the following:

| `'normal'`   | Normal distribution (**default**)     | 
| `'binomial'` | Binomial distribution                 | 
| `'poisson'`  | Poisson distribution                  | 

**Example:** <code style="color:#A020F0">'Distribution','normal'</code>
</div>

<!--Activation-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Activation'</span> - Activation function of hidden layers</h3></header>

#### Data Type: String 
<br>
Activation function of hidden layers, specified as the comma-separated pair consisting of <samp>'Activation'</samp> and one of the following:

| `'relu'`    | $f(x)= \text{max}(x,0)$ | Rectified Linear Unit (ReLU) function (**default**)        | 
| `'sigmoid'` | $f(x)=\frac{1}{1+e^{-x}}$| Sigmoid function  | 
| `'tanh'`    | $f(x) = \frac{e^{2x}-1}{e^{2x}+1}$  | Tanh function                      | 

**Example:** <code style="color:#A020F0">'Activation','relu'</code>
</div>
</div>
