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
`vbayesPlot(type,Name,Value)` plots figures defined in `type`. `Name` and `Value` specifie additional options using one or more name-value pair arguments. For example, users can specify the distribution name and parameters for density plots. 

---

## Input Arguments
<!--type-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">type</span> - Type of plot </header>
#### Data type: string
<br>
Type of figure to be plotted. Can be specify as:
- `'Density'` plot a density. The argument `'Distribution'` must be specified.  
- `'Interval'` plot prediction interval. 

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
Distribution of density to be plotted, specified as one of the following:

| `'Normal'`   | Normal distribution | mean, variance |
| `'Beta'`     | Beta distribution   | |
| `'Gamma'`     | Gamma distribution   | |
| `'Inverse-Gamma'`     | Inverse-Gamma distribution   | |
|`'Custom'`| Custom distribution| array of samples|

**Note:** Only available for density plot.

**Example:** <code style="color:#A020F0">'Distribution',{'Normal',[0,1]}</code>
</div>

<!--Ytrue-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Ytrue'</span> - True responses</h3></header>

#### Data Type: String 
<br>


**Example:** <code style="color:#A020F0">'Activation','relu'</code>
</div>
</div>
