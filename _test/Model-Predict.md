---
layout: default
title: vbayesPredict Method
parent: Models API Reference
nav_order: 6
permalink: /model/vbayesPredict/
block_color: GhostWhite
---

# <samp>vbayesPredict</samp> method [Github code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/Models/vbayesPredict.m){: .fs-4 .btn .btn-purple  .float-right}
{: .fs-8 }

Make prediction on new data with a fitted model. 
{: .fs-6 .fw-300 }

---

## Syntax

```matlab
Pred = vbayesPredict(Mdl,Name,Value)
```
---
## Description
`Pred = vbayesPredict(Mdl,Name,Value)` returns prediction result `Pred` given a fitted model `Mdl`. `Name` and `Value` specifie additional options using one or more name-value pair arguments. For example, users can specify if the true responses are provided to compute predictive scores. The fitted model `Mdl` must have a method named `vbayesPredict` to make predictions.  

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--model-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Mdl</span> - VBLab supported or custom models</header>
#### Data type: VBLab model object | custome model object | function handle
<br>
The statistical models containing unknown parameters, can be specified as:

- [VBLab model object](/VBLabDocs/model#vblab-model).
- or [function handle to compute the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms](/VBLabDocs/model/custom#custom-handler).
- or [custom model object including method to compute the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms](http://localhost:4000/VBLabDocs/model/custom/#class-model)
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
|`FitMethod` * |string  | &bull; The fittind method used to estimate model paramters <br> &bull; Currently, users can only use [NAGVAC]({% link VB-NAGVAC.md %}) method to fit a <samp>DeepGLM</samp> model|
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