---
layout: default
title: NAGVAC
parent: VB API Reference 
nav_order: 3
permalink: /gvb/nagvac
---

# **NAGVAC**
{: .fs-8 }

Run the NAGVAC method.
{: .fs-6 .fw-300 }

[Tutorial](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [GitHub](https://github.com/VBayesLab/Tutorial-on-VB){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Syntax

```matlab
PostVB = NAGVAC(data,Name,Value)
```
---
## Description
`PostVB = NAGVAC(model,data,Name,Value)` returns the Bayesian approximation `PostVB` to given the `model` and `data`. `Name` and `Value` specifie additional options using one or more name-value pair arguments. For example, you can specify how many samples used to estimate the lower bound. 

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--model-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Mdl</span> - VBLab supported or custom models</header>
#### Data type: VBLab model object | function handler
<br>
The statistical models containing unknown parameters, specified as:
- [VBLab model object](/VBLabDocs/model#vblab-model).
- or [function handler to compute the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms](/VBLabDocs/model/custom#custom-handler).
</div>

<!--data-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">data</span> - Input data</header>
#### Data type: table | dataset array
<br>
The data to which the model `Mdl` is fit, specified as a table or dataset array. 

For cross-sectional data, `CGVB` takes the last variable as the response variable and the others as the predictor variables.

For time series data, the data can be stored in a row or column 1D array. 
</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<!--Name-Value Pairs-->
<span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Name-Value Pair Arguments </span>

Specify optional comma-separated pairs of `Name,Value` arguments. `Name` is the argument name and `Value` is the corresponding value. `Name` must appear inside quotes. You can specify several name and value pair arguments in any order as `Name1,Value1,...,NameN,ValueN`.

**Example:** `'LearningRate',0.001,'LBPlot',false` specifies that the learning rate of the CGVB algorithm is set to be $0.001$ and the plot of the covergence of the lowerbound is shown at the end of the algorithm.  



|Name   | Default Value |Notation|Description |
|:------|:------------|:------------|:------------|
|[`'GradWeight1'`](#GradWeight1)|`0.9`| $\beta_1$ | Adaptive learning weight 1 |
|[`'GradWeight2'`](#GradWeight2)|`0.9`| $\beta_1$ | Adaptive learning weight 2 |
|[`'GradientMax'`](#GradientMax)| `10` | $\ell_\text{threshold}$ | Gradient clipping threshold|
|[`'InitMethod'`](#InitMethod)|`'Random'`| |Initialization method |
|[`'LBPlot'`](#LBPlot)|`true`| | Flag to plot the lowerbound or not |
|[`'LearningRate'`](#LearningRate)|`0.01`| $\epsilon_0$  | Fixed learning rate|
|[`'MaxIter'`](#MaxIter)|`1000`| | Maximum number of iterations |
|[`'MaxPatience'`](#MaxPatience)|`20` | $P$ | Maximum patience for early stopping |
|[`'MeanInit'`](#MeanInit)|`None`| | Initial values of varitional mean |
|[`'NumSample'`](#NumSample)|`50`| $S$ | Monte Carlo samples to estimate the lowerbound |
|[`'NumParams'`](#NumParams)|`None`| | Number of model parameters |
|[`'SaveParams'`](#SaveParams)|`false`| | Flag to save training parameters or not |
|[`'Setting'`](#Setting)|`None`| | Additional setting for custom models |
|[`'SigInitScale'`](#SigInitScale)|`0.1`| | Constant factor for initialization |
|[`'StdForInit'`](#StdForInit)|`0.01`| | Standard deviation of normal distribution for initialization |
|[`'StepAdaptive'`](#StepAdaptive)|`'MaxIter'/2`| $\tau$ | Threshold to start reducing learning rates |
|[`'Validation'`](#Validation)|`0.1`| | Percentage of training data used for validation |
|[`'ValidationLoss'`](#ValidationLoss)|`PPS`| | Validation loss computed during fitting phase |
|[`'Verbose'`](#Verbose)|`true`| | Flag to show real-time fitting information or not |
|[`'WindowSize'`](#WindowSize)|`50`| | Rolling window size to smooth the lowerbound |

<!--GradWeight1-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'GradWeight1'</span> - Adaptive learning weight 1</h3></header>
{: #GradWeight1}

#### Data Type: Double
<br>
Must be a number between $0$ and $1$.

**Default:** `0.9`

**Example:** `'GradWeight1',0.95`
</div>

<!--GradWeight2-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'GradWeight2'</span> - Adaptive learning weight 2</h3></header>
{: #GradWeight2}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--GradientMax-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'GradientMax'</span> - Gradient clipping threshold</h3></header>
{: #GradientMax}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--InitMethod-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'InitMethod'</span> - Initialization method</h3></header>
{: #InitMethod}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--LBPlot-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'LBPlot'</span> - Flag to plot the lowerbound or not</h3></header>
{: #LBPlot}

#### Data Type: True | False

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--LearningRate-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'LearningRate'</span> - Fixed learning rate</h3></header>
{: #LearningRate}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--MaxIter-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'MaxIter'</span> - Maximum number of iterations</h3></header>
{: #MaxIter}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--MaxPatience-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'MaxPatience'</span> - Maximum patience for early stopping  </h3></header>
{: #MaxPatience}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--MeanInit-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'MeanInit'</span> - Initial values of varitional mean</h3></header>
{: #MeanInit}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--NumSample-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'NumSample'</span> - Monte Carlo samples to estimate the lowerbound</h3></header>
{: #NumSample}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--NumParams-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'NumParams'</span> - Number of model parameters </h3></header>
{: #NumParams}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--SaveParams-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'SaveParams'</span> - Flag to save training parameters or not </h3></header>
{: #SaveParams}

#### Data Type: True | False

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>


<!--Setting-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Setting'</span> - Additional setting for custom models </h3></header>
{: #Setting}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--SigInitScale-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'SigInitScale'</span> - Constant factor for initialization </h3></header>
{: #SigInitScale}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>


<!--StdForInit-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'StdForInit'</span> - Standard deviation of normal distribution for initialization</h3></header>
{: #StdForInit}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--StepAdaptive-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'StepAdaptive'</span> - Threshold to start reducing learning rates </h3></header>
{: #StepAdaptive}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--Validation-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Validation'</span> - Percentage of training data used for validation </h3></header>
{: #Validation}

#### Data Type: True | False

This option is only available for cross-sectional (tabular) data. 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--ValidationLoss-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'ValidationLoss'</span> - Validation loss computed during fitting phase </h3></header>
{: #ValidationLoss}

#### Data Type: True | False

This option is only available for cross-sectional (tabular) data. 

**Default:** `None`

**Example:** `'ValidationLoss','MSE'`
</div>

<!--Verbose-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Verbose'</span> - Flag to show real-time fitting information or not </h3></header>
{: #Verbose}

#### Data Type: True | False

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

<!--WindowSize-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'WindowSize'</span> - Rolling window size to smooth the lowerbound </h3></header>
{: #WindowSize}

#### Data Type: Cell Array 

**Default:** `{'Normal',[0,1]}`

**Example:** `'Prior',{'Normal',[0,10]}`
</div>

</div>

---

## Output Arguments
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">EstMdl</span> - Model Object </header>
{: #cgvb-object}

#### Data type: DeepGLM Object| RECH Object | LogisticRegression Object | Custom Object
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

<!--Post-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Post</span> - </header>
#### Data type: VBLab model object | function handler
<br>
The statistical models containing unknown parameters, specified as:
- [VBLab model object](/VBLabDocs/model/#vblab-model).
- or [function handler to compute the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms](/VBLabDocs/model/custom/#custom-handler).
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