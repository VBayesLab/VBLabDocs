---
layout: default
title: vbayesFit Method
parent: Models API Reference
nav_order: 5
permalink: /model/vbayesFit/
block_color: GhostWhite
---
# <samp>vbayesFit</samp> method
Fit a VBLab supported model 
{: .fs-6 .fw-300 }

[Tutorial]({{site.baseurl}}{% link Tutorial-FFVB.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [GitHub](https://github.com/VBayesLab/Tutorial-on-VB){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Syntax

```matlab
Estmdl = vbayesFit(mdl,data,Name,Value)
mdl.vbayesFit(data,Name,Value)
```
---
## Description
`mdl = DeepGLM(Network,Name,Value)` returns [DeepGLM model object](#deepglm-object) `mdl` given neural network structure `Network`. `Name` and `Value` specifie additional options using one or more name-value pair arguments. For example, users can specify the activation function or distribution of the output. 

đsdsd

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--Network-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Network</span> - Neuron Network structure of the deepGLM model </header>
#### Data type: 1D Array
<br>
Neuron Network structure of the deepGLM model. `[NumFeatures, L1,...,LM]`:
<dl>
<dt><code>Numfeatures</code></dt> <dd>Number of features (columns) of training data.</dd>
<dt><code>L1,...,LM</code></dt> <dd>Number of hidden nodes in each hidden layer. For example, <code>L1</code> is the number of hidden nodes in the first hidden layer and so on. </dd>
</dl>
**Note:** The output layer has always only 1 node.
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

## Output Arguments
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">mdl</span> - DeepGLM Object </header>
{: #deepglm-object}

#### Data type: DeepGLM Object
<br>
<samp>DeepGLM</samp> is an object of the DeepGLM class with pre-defined properties and functions.

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

### Object Properties
The DeepGLM object properties include information about model-specific information, coefficient estimates and fitting method as following. The * notation indicates properties whic are only available when the model is fitted. The default value for these properties is `None`.

|Properties|Data type |Description {: .text-center}|
|:-------------|:------------------|:------|
|`ModelName`    |string (r)| Name of the model, which is <samp>'DeepGLM'</samp>|
|`NumParams`    |integer (+) | Number of model parameters|
|`Network`      |Array | Neural network structure of DeepGLM models|
|`Distribution` |string | Neural network structure of DeepGLM models|
|`Activation`   |string | Neural network structure of DeepGLM models|
|`FitMethod` * |string  | &bull; The fittind method used to estimate model paramters <br> &bull; Currently, users can only use [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}) method to fit a <samp>DeepGLM</samp> model|
|`Coefficients` * |Cell array| &bull; Estimated Mean of weights of Deep Neuron Network <br> &bull; Used to doing point estimation for new test data|
|`Shrinkage` *    |Array| &bull; Array storing estimated values of group Lasso coefficients|
|`PPS` *          |Array| &bull; Array storing PPS values measured on validation data in each iteration during training phase|
|`Shrinkage` *    |Array| Array storing estimated values of group Lasso coefficients|
|`MSE` *          |Array| &bull; Array storing MSE values measured validation data in each iteration during training phase <br> &bull; Only available if responses follow normal or poisson distribution|
|`Accuracy` *     |1D Array| &bull; Array storing Classification Rate measured on validation data in each iteration during training phase<br> &bull; Only available if responses are binary variable|

</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

### Object Functions
Use the object functions to predict responses and to visualize the prediction.

|[<span style="font-family:monospace">vbayesFit</span>]({% link Model-Predict.md %})| Fit a deepGLM model|
|[<span style="font-family:monospace">vbayesPredict</span>]({% link Model-Predict.md %})| Predict responses of fitted DeepGLM models|
|[<span style="font-family:monospace">vbayesPlot</span>]({% link Model-Plot.md %})| Plot analytic figures of fitted DeepGLM models|

</div>


--- 

## Examples


---

## Reference
Tran, M.-N., Nguyen, T.-N., Nott, D., and Kohn, R. (2020). [Bayesian deep net GLM and GLMM](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747). *Journal of Computational and Graphical Statistics*, 29(1):97-113. 