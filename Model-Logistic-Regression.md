---
layout: default
title: LogisticRegression Class
parent: Models API Reference
nav_order: 2
permalink: /model/logistics/
block_color: GhostWhite
---

# <samp>LogisticRegression</samp> class [Github code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/Models/LogisticRegression.m){: .fs-4 .btn .btn-purple  .float-right}
Create Logistic Regression model object 
{: .fs-6 .fw-300 }

---

## Syntax

```matlab
Mdl = LogisticRegression(NumFeatures,Name,Value)
```
---
## Description
`mdl = LogisticRegression(NumFeatures,Name,Value)` returns LogisticRegression model object `Mdl` given number of model parameters `NumFeatures`. `Name` and `Value` specifies additional options using one or more name-value pair arguments. For example, users can specify different choices of priors for model parameters.

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--Network-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">NumFeatures</span> - Number of model parameters </header>
#### Data type: positive integer
<br>
Number of model parameters, which is also the number of data features or covariates or independent variables. 
</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<!--Name-Value Pairs-->
<span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Name-Value Pair Arguments </span>

Specify optional comma-separated pairs of `Name,Value` arguments. `Name` is the argument name and `Value` is the corresponding value. `Name` must appear inside quotes. You can specify several name and value pair arguments in any order as `Name1,Value1,...,NameN,ValueN`.

**Example:** `'Prior',{'Normal',[0,10]},'CutOff',0.6` specifies that the prior distribution of model parameters is a normal distribution with $0$ mean and $10$ variance, and the cutoff probability is $0.06$.

<!--Prior-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Prior'</span> - Prior distribution of model parameters</h3></header>

#### Data Type: Cell Array 
<br>
Prior distribution of the model parameters, specified as the comma-separated pair consisting of <samp>'Prior'</samp> and a cell array of distribution name and distribution parameters. Distribution parameters are specified as an array. 

|Prior Name|  Description|
|:-------------|:--------------|:-------------|
|`'Normal'`|  Normal distribution $\mathcal{N}(\mu,\sigma^2)$ (**default**)     | 
|`'Gamma'`|  Gamma distribution $\Gamma(\alpha,\beta)$     | 
|`'Inverse-Gamma'`| Gamma distribution $\Gamma^{-1}(\alpha,\beta)$       | 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--CutOff-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'CutOff'</span> - Cut-off probability</h3></header>

#### Data Type: Float
<br>
The cut-off probability for class assignment. Must be between 0 and 1. 

**Default:** `0.5`

**Example:** `'CutOff',0.6`
</div>

<!--Description-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Description'</span> - Model description</h3></header>

#### Data Type: string
<br>
Model description, specified as a string scalar or character vector. Provide additional information about the model. 

**Default:** Empty string

**Example:** `'Description','Logistic Regression model with gamma prior'`
</div>

</div>

---

## Output Arguments
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Mdl</span> - LogisticRegression Object </header>
{: #logistic-object}

#### Data type: LogisticRegression Object
<br>
<samp>LogisticRegression</samp> is an object of the LogisticRegression class with pre-defined properties and functions.

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

### Object Properties
The LogisticRegression object properties include information about model-specific information, coefficient estimates and fitting method. 

|Properties|Data type |Description{: .text-center}|
|:-------------|:------------------|:------|
|`ModelName`    |string (r)| Name of the model, which is <samp>'LogisticRegression'</samp>|
|`NumParams`    |integer (+) | Number of model parameters|
|`Cutoff`       |float | Cut-off probabitlity|
|`Post` *       |struct  | Information about the fittedd method used to estimate model paramters|
|`Coefficient` * |cell array (r)| &bull; Estimated Mean of model parameters <br> &bull; Used to doing point estimation for new test data|
|`CoefficientVar` * |cell array (r)| Variance of coefficient estimates|
|`LogLikelihood` * |double (r)| Loglikelihood of the fitted model. |
|`FittedValue` * |array (r)| Fitted probability|

Notation:
- \* $\rightarrow$ object properties which are only available when the model is fitted. Default value is `None`.
- (+) $\rightarrow$ positive number.
- (r) $\rightarrow$ read-only properties.

</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

### Object Methods
Use the object methods to initialize model parameters, predict responses, and to visualize the prediction.

|[<span style="font-family:monospace">vbayesInit</span>]({{site.baseurl}}{% link Model-Plot.md %})| Initialization method of model parameters|
|[<span style="font-family:monospace">vbayesPredict</span>]({{site.baseurl}}{% link Model-Predict.md %})| Predict responses of fitted DeepGLM models|
|[<span style="font-family:monospace">vbayesPlot</span>]({{site.baseurl}}{% link Model-Plot.md %})| Plot analytic figures of fitted DeepGLM models|

</div>

--- 

## Examples
<!--Continuous response-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

## <span style="font-weight:bold;font-size:20px">Fit a LogisticRegression model for binary response</span> [Github code](https://github.com/VBayesLab/Tutorial-on-VB){: .fs-4 .btn .btn-purple  .float-right}
{: #logistic-binary}
Fit a LogisticRegression model to [LabourForce](/VBLabDocs/datasets/#labour-force) data using [CGVB]({{site.baseurl}}{% link VB-CGVB.md %})

Load the LabourForce data using the [<span style="font-family:monospace">readdata()</span>]({{site.baseurl}}{% link Utilities-Read-Data.md%}) function. 
The data is a matrix with the last column is the response variable. Set the `'Intercept'` argument to be `true` to add a column of 1 to the data matrix as intercepts.  
```matlab
% Load the LabourForce dataset
labour = readdata('LabourForce',...
                  'Type','Matrix',...
                  'Intercept',true);
```
Create a LogisticRegression model object by specifying the number of parameters as the input argument. Change the variance of the normal prior to $10$.
```matlab
% Number of input features
n_features = size(labour,2)-1;
% Define a LogisticRegression model object
Mdl = LogisticRegression(n_features,...
                         'Prior',{'Normal',[0,10]});
```
Run CGVB to obtain VB approximation of the posterior distribution of model parameters. Use [<span style="font-family:monospace">trainTestSplit()</span>]({{site.baseurl}}{% link Utilities-Read-Data.md%}) function to split `labour` to training and test data. 
```matlab
% Train/Test split
[labour_train, labour_test] = trainTestSplit(labour,0.2);
% Run CGVB to obtain VB approximation of the posterior distribution
Estmdl = CGVB(Mdl,labour_train,...
              'LearningRate',0.01,...    % Use a small learning rate
              'Validation',0.15,...      % Use 15% number of observations fpr validation    
              'Loss','PPS');             % Use PPS as the predictive metric
```
Given the fitted LogisticRegression model `Estmdl`, we can make prediction with new data. Set `'YTest'` to `true` to indicate that the last column of test data contains true responses. Use `'Loss'` argument to specify predictive scores we want to compute. Given the true labels, we can compute the miss-classification rate (MCR) and PPS as the predictive scores.    
```matlab
% Make prediction with new data and compute prediction scores in PPS and MCR
[Pred,Error] = vbayesPredict(Estmdl,labour_test,...
                             'YTest',true,...            % There are true response
                             'Loss',{'PPS','MCR'});      % Compute predictive scores in PPS and MCR
```
</div>

---

## Reference
[1] Tran, M.-N., Nguyen, T.-N., Nott, D., and Kohn, R. (2020). Bayesian deep net GLM and GLMM. *Journal of Computational and Graphical Statistics*, 29(1):97-113. [Read the paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747)

---

## See Also
{: #see-also}
[DeepGLM]({{site.baseurl}}{% link Model-DeepGLM.md %}) $\mid$ [rech]({{site.baseurl}}{% link Model-RECH.md %}) $\mid$ [Custom model]({{site.baseurl}}{% link Model-Custom.md%}) $\mid$ [CGVB]({{site.baseurl}}{%link VB-CGVB.md%})