---
layout: default
title: readData
parent: Utilities Reference
nav_order: 1
permalink: /utilities/readdata
---

# <samp>readData</samp> [Github code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/Utilities/readData.m){: .fs-4 .btn .btn-purple  .float-right}
{: .fs-8 }

Load built-in datasets
{: .fs-6 .fw-300 }

---

## Syntax

```matlab
data = readData(DataName,Name,Value)
```
---
## Description
`PostVB = CGVB(Mdl,data,Name,Value)` returns the Bayesian approximation `PostVB` to provided model `Mdl` and data `data`. The model `Mdl` can be a VBLab supported or user-defined model. `Name` and `Value` specifies additional options using one or more name-value pair arguments. For example, you can specify how many samples used to estimate the lower bound. 

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--model-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Mdl</span> - VBLab supported or custom models</header>
#### Data type: VBLab model object | function handler
<br>
Number of model parameters, which is also the number of data features or covariates or independent variables. 
</div>

<!--data-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">data</span> - VBLab supported or custom models</header>
#### Data type: Array
<br>
`data` is an innovation series with mean 0 and conditional variance characterized by the model specified in `Mdl`.

The last observation of `data` is the latest observation.
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
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">mdl</span> - LogisticRegression Object </header>
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
|`FitMethod` * |string  | &bull; The fittind method used to estimate model paramters |
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

### Object Functions
Use the object functions to fit the model, predict responses, and to visualize the prediction.

|[<span style="font-family:monospace">vbayesFit</span>]({% link Model-Predict.md %})| Fit a deepGLM model|
|[<span style="font-family:monospace">vbayesInit</span>]({% link Model-Plot.md %})| Initialization method of model parameters|
|[<span style="font-family:monospace">vbayesPredict</span>]({% link Model-Predict.md %})| Predict responses of fitted DeepGLM models|
|[<span style="font-family:monospace">vbayesPlot</span>]({% link Model-Plot.md %})| Plot analytic figures of fitted DeepGLM models|

</div>

--- 

## Examples
<!--Continuous response-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

## <span style="font-weight:bold;font-size:20px">Fit a LogisticRegression model for binary response</span> [Github code](https://github.com/VBayesLab/Tutorial-on-VB){: .fs-4 .btn .btn-purple  .float-right}
{: #logistic-binary}
Fit a LogisticRegression model to [LabourForce](/VBLabDocs/datasets/#labour-force) data using [CGVB]({% link VB-CGVB.md %})

Load the LabourForce data using the [<span style="font-family:monospace">readdata()</span>]({% link Utilities-Read-Data.md%}) function. 
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
n_features = size(marketing,2)-1;
% Define a LogisticRegression model object
mdl = LogisticRegression(n_features,...
                         'Prior',{'Normal',[0,10]});
```
Run CGVB to obtain VB approximation of the posterior distribution of model parameters. Use [<span style="font-family:monospace">trainTestSplit()</span>]({% link Utilities-Read-Data.md%}) function to split `labour` to training and test data. 
```matlab
% Train/Test split
[labour_train, labour_test] = trainTestSplit(labour,0.2);
% Run CGVB to obtain VB approximation of the posterior distribution
[~,Estmdl] = CGVB(mdl,labour_train,...
                  'LearningRate',0.01,...    % Use a small learning rate
                  'Validation',0.15,...      % Use 15% number of observations fpr validation    
                  'Loss','PPS');             % Use PPS as the predictive metric
```
Alternatively, we can also run CGVB by calling the [<span style="font-family:monospace">vbayesFit()</span>]({% link Model-Fit.md %}) method of `mdl` with the same training setting
```m
Estmdl = vbayesFit(mdl,labour_train,...
                   'FitMethod','CGVB',...
                   'LearningRate',0.01,...       
                   'Validation',0.15,...    
                   'Loss','PPS');           
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

