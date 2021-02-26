---
layout: default
title: RECH Class
parent: Models API Reference
nav_order: 3
permalink: /model/rech/
block_color: GhostWhite
---

# <samp>RECH</samp> class
{: .fs-8 }
Create a RECH model object 
{: .fs-6 .fw-300 }

---

## Syntax

```matlab
Mdl = RECH(Specs,Name,Value)
```
---
## Description
`Mdl = RECH(Specs,Name,Value)` returns RECH model object `Mdl` given particular specification `Specs`. `Name` and `Value` specifies additional options using one or more name-value pair arguments. For example, users can specify different choices of priors for model parameters.

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--Specs-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Specs</span> - RECH model specification </header>
#### Data type: string
<br>
Specification of the RECH model, specified as one of these values:

| `'SRN-GARCH'`   | SRN-GARCH specification    | 
| `'SRN-GJR'`     | SRN-GJR specification      | 
| `'SRN-EGARCH'`  | SRN-EGARCH specification   | 

See [Nguyen et al. (2020)](#reference) for mathematical description of specification of the RECH model.   
</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<!--Name-Value Pairs-->
<span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Name-Value Pair Arguments </span>

Specify optional comma-separated pairs of `Name,Value` arguments. `Name` is the argument name and `Value` is the corresponding value. `Name` must appear inside quotes. You can specify several name and value pair arguments in any order as `Name1,Value1,...,NameN,ValueN`.

**Example:** {%raw%}`'Prior',{{'alpha','beta'},'Uniform',[0,1]},'Distribution','Normal'`{%endraw%} specifies that the prior distribution of parameters $\alpha$ and $\beta$ is a uniform distribution in $[0,1]$, and the innovation is standard normal. 

<!--Prior-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Prior'</span> - Prior distribution of model parameters</h3></header>
{: #rech-prior}
#### Data Type: Cell Array 
<br>
Prior distribution of the model parameters, specified as the comma-separated pair consisting of <samp>'Prior'</samp> and a 2D cell array whose each row consists of: 
- Parameter names, specified as a cell array of strings. 
- Distribution name, specified as a string.  
- Distribution parameters, specified as an array of numerical number. 

The RECH class use `VarName` property to store parameter names. The parameter names for each specification of the RECH model is defined as:   

| Specification   | Parameter names | 
|:----------------|:---------------------------|
| `'SRN-GARCH'`   |`'alpha','beta','beta0','beta1','v0','v1','v2','w','b'`     |
| `'SRN-GJR'`     |`'alpha','beta','gamma','beta0','beta1','v0','v1','v2','w','b'`     | 
| `'SRN-EGARCH'`  |`'alpha','beta','gamma','omega','beta0','beta1','v0','v1','v2','w','b'`|

The VBLab package provides following options for prior distribution of the RECH model parameters 

|Prior distribution|  Description|
|:-------------|:--------------|:-------------|
|`'Normal'`|  Normal distribution $\mathcal{N}(\mu,\sigma^2)$     | 
|`'Gamma'`|  Gamma distribution $\Gamma(\alpha,\beta)$     | 
|`'Inverse-Gamma'`|Inverse-Gamma distribution $\Gamma^{-1}(\alpha,\beta)$       | 
|`'Beta'`|  Beta distribution $\text{Beta}(\alpha,\beta)$     | 
|`'Uniform'`|  Uniform distribution $\mathcal{U}(\alpha,\beta)$     | 

**Default:** The default priors for each specification of the RECH model are as following

| Specification   | Prior | 
|:----------------|:---------------------------|
| `'SRN-GARCH'`   |{%raw%}`{{'alpha','beta'},'Uniform',[0,1]; {'beta0','beta1'},'Normal',[0,1];{'v0','v1','v2','w','b'},'Normal',[0,0.1]}`{%endraw%}  |
| `'SRN-GJR'`     |{%raw%}`{{'alpha','beta'},'Uniform',[0,1]; {'beta0','beta1'},'Normal',[0,1];{'v0','v1','v2','w','b'},'Normal',[0,0.1]}`{%endraw%} | 
| `'SRN-EGARCH'`  |{%raw%}`{{'alpha','beta'},'Uniform',[0,1]; {'beta0','beta1'},'Normal',[0,1];{'v0','v1','v2','w','b'},'Normal',[0,0.1]}`{%endraw%}|


**Example:** {%raw%}`'Prior',{{'beta0','beta1'},'Normal',[0,0.1]}`{%endraw%}
</div>

<!--Distribution-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Distribution'</span> - Conditional probability distribution of innovation process</h3></header>

#### Data Type: string
<br>
The probability of the innovation. The current version of the RECH model supports only standard normal innovation.  

| `'Normal'`   | Normal distribution (**default**)     |  

**Default:** `Normal`

**Example:** `'Innovation','Normal'`
</div>

<!--Description-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Description'</span> - Model description</h3></header>

#### Data Type: string
<br>
Model description, specified as a string scalar or character vector. Provide additional information about the model. 

**Default:** Empty string

**Example:** `'Description','SRG-GARCH with normal innovation'`
</div>

</div>

---

## Output Arguments
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">mdl</span> - RECH Object </header>
{: #rech-object}

#### Data type: RECH Object
<br>
<samp>RECH</samp> is an object of the RECH class with pre-defined properties and functions.

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

### Object Properties
The RECH object properties include information about model-specific information, coefficient estimates and fitting method. 

|Properties|Data type |Description{: .text-center}|
|:-------------|:------------------|:------|
|`ModelName`    |string (r)| Name of the model, which is <samp>'RECH'</samp>|
|`VarName`      |cell array (r) | Model parameter names. Stored in a cell array of strings|
|`NumParams`    |integer (+) | Number of model parameters|
|`Post` *       |struct  | Information about the fittedd method used to estimate model paramters <br> &bull; <samp>RECH</samp> models can only be fitted by [MGVB]({{site.baseurl}}{% link VB-MGVB.md %}) technique|
|`Coefficients` * |Cell array| &bull; Estimated Mean of weights of Deep Neuron Network <br> &bull; Used to doing point estimation for new test data|
|`CoefficientVar` * |cell array (r)| Variance of coefficient estimates|
|`LogLikelihood` * |double (r)| Loglikelihood of the fitted model. |

Notation:
- \* $\rightarrow$ object properties which are only available when the model is fitted. Default value is `None`.
- (+) $\rightarrow$ positive number.
- (r) $\rightarrow$ read-only properties.

</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

### Object Methods
Use the object methods to initialize model parameters, predict responses, and to visualize the prediction.

|<span style="font-family:monospace">vbayesInit</span>| Initialization method of model parameters|
|<span style="font-family:monospace">vbayesPredict</span>| Predict responses of fitted DeepGLM models|

</div>

--- 

## Examples
{: #examples}

To be updated...

{% comment %} 
<!--SP500-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

## <span style="font-weight:bold;font-size:20px">Fit a RECH model to stock index data</span> [Github code](https://github.com/VBayesLab/Tutorial-on-VB){: .fs-4 .btn .btn-purple  .float-right}
{: #rech-sp500}
Fit a RECH model to the SP500 index data using [MGVB]({{site.baseurl}}{% link VB-MGVB.md %})

Load the LabourForce data using the [<span style="font-family:monospace">readdata()</span>]({{site.baseurl}}{% link Utilities-Read-Data.md%}) function. 
The data is a matrix with the last column is the response variable. Set the `'Intercept'` argument to be `true` to add a column of 1 to the data matrix as intercepts.  
```matlab
% Load the SP500 daily return data
sp500 = reaDdata('RealizedLibrary',...
                 'Index','SP500',...
                 'Length',1000);
```
Create a RECH model object and specify priors. 

{%raw%}
```m
% Define priors for model parameters using 2D cell array
% Parameter names must be specified correctly
prior = {{'v','w','b'},'Normal',[0,1];...
         {'beta0','beta1'},'Inverse-Gamma',[0.25,2.5];...
         {'alpha','beta'},'Uniform',[0,1]};
% Define a RECH model with SRN-GARCH specification		 
rech_model = RECH('SRN-GARCH',...
                  'Prior', prior);
```
{%endraw%}
Run MGVB to obtain VB approximation of the posterior distribution of model parameters. Use [<span style="font-family:monospace">trainTestSplit()</span>]({{site.baseurl}}{% link Utilities-Read-Data.md%}) function to split `sp500` to in-sample and out-of-sample data. 
```matlab
% Train/Test split
[sp500_in, sp500_out] = trainTestSplit(sp500,0.2,...
                                       'Shuffle',false);
% Run MGVB to obtain VB approximation of the posterior distribution
[~,Estmdl] = MGVB(mdl,sp500_in,...
                  'NumSample',100,...
                  'LearningRate',0.01,...
                  'GradWeight',0.4,...
                  'MaxPatience',50,...
                  'MaxIter',2500,...
                  'GradientMax',100,...
                  'WindowSize',30);
```
Given the fitted RECH model `Estmdl`, we can make one-step-ahead forecast given out-of-sample data. Set `'YTest'` to `true` to indicate that the last column of test data contains true responses. Use `'Loss'` argument to specify predictive scores we want to compute. Given the true labels, we can compute the MSE and PPS as the predictive scores.    
```matlab
% Make prediction with new data and compute prediction scores in PPS and MCR
[Pred,Error] = vbayesPredict(Estmdl,T_out,...
                             'YTest',sp500_out,...    % There are true return
                             'VTest',sp500_re_out,... % Realized volatility to compute predictive scores  
                             'Loss',{'PPS','MSE'});   % Compute predictive scores in PPS and MSE
```
</div>
{% endcomment %}
---

## Reference
{: #reference}
[1] Nguyen, T.-N., Tran, M.-N., and Kohn, R. (2020). Recurrent conditional heteroskedasticity. arXiv:2010.13061. [Read the paper](https://arxiv.org/abs/2010.13061)

---

## See Also
{: #see-also}
[LogisticRegression ]({{site.baseurl}}{% link Model-Logistic-Regression.md %}) $\mid$ [DeepGLM]({{site.baseurl}}{% link Model-DeepGLM.md %}) $\mid$ [Custom model]({{site.baseurl}}{% link Model-Custom.md%}) $\mid$ [MGVB]({{site.baseurl}}{%link VB-MGVB.md%})