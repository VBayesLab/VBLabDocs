---
layout: default
title: CGVB
parent: VB API Reference 
nav_order: 1
permalink: /gvb/cgvb
block_color: GhostWhite
---

# **CGVB**
{: .fs-8 }

Fit VBLab supported or custom models using the CGVB algorithm
{: .fs-6 .fw-300 }

[Tutorial]({{site.baseurl}}{% link Tutorial-FFVB-CGVB.md%}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [GitHub](https://github.com/VBayesLab/Tutorial-on-VB){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Syntax

```matlab
EstMdl = CGVB(Mdl,data,Name,Value)
```
```matlab
Post = CGVB(Func,data,Name,Value)
```
---
## Description
`EstMdl = CGVB(Mdl,data,Name,Value)` run the CGVB algorithm to return the fitted model `EstMdl` given the model `Mdl` and data `data`. The model `Mdl` can be a VBLab supported or user-defined model object. `Name` and `Value` specifies additional options using one or more name-value pair arguments. For example, you can specify how many samples used to estimate the lower bound. 

`Post = CGVB(Mdl,data,Name,Value)` run the CGVB algorithm to return the Bayesian approximation `Post` given the model `Func`, specified as a function handler, and data `data`.

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

**Example:** `'LearningRate',0.001,'LBPlot',true` specifies that the learning rate of the CGVB algorithm is set to be $0.001$ and the plot of the covergence of the lowerbound is shown at the end of the algorithm.  



|Name   | Default Value |Notation|Description |
|:------|:------------|:------------|:------------|
|[`'BatchSize'`](#BatchSize)|`None`|  | Mini-batch size for stochastic gradient descent|
|[`'GradWeight1'`](#GradWeight1)|`0.9`| $\beta_1$ | Adaptive learning weight 1 |
|[`'GradWeight2'`](#GradWeight2)|`0.9`| $\beta_1$ | Adaptive learning weight 2 |
|[`'GradientMax'`](#GradientMax)| `10` | $\ell_\text{threshold}$ | Gradient clipping threshold|
|[`'InitMethod'`](#InitMethod)|`'Random'`| |Initialization method |
|[`'InitValue'`](#InitValue)|`None`| | Initial values of varitional mean |
|[`'LBPlot'`](#LBPlot)|`true`| | Flag to plot the lowerbound or not |
|[`'LearningRate'`](#LearningRate)|`0.01`| $\epsilon_0$  | Fixed learning rate|
|[`'MaxIter'`](#MaxIter)|`1000`| | Maximum number of iterations |
|[`'MaxEpoch'`](#MaxEpoch)|`None`| | Maximum number of epochs |
|[`'MaxPatience'`](#MaxPatience)|`20` | $P$ | Maximum patience for early stopping |
|[`'NumSample'`](#NumSample)|`50`| $S$ | Monte Carlo samples to estimate the lowerbound |
|[`'NumParams'`](#NumParams)|`None`| | Number of model parameters |
|[`'SaveParams'`](#SaveParams)|`false`| | Flag to save training parameters or not |
|[`'Setting'`](#Setting)|`None`| | Additional setting for custom models |
|[`'SigInitScale'`](#SigInitScale)|`0.1`| | Constant factor for initialization |
|[`'StdForInit'`](#StdForInit)|`0.01`| | Standard deviation of normal distribution for initialization |
|[`'StepAdaptive'`](#StepAdaptive)|`'MaxIter'/2`| $\tau$ | Threshold to start reducing learning rates |
|[`'TrainingLoss'`](#TrainingLoss)|`PPS`| | Training loss over VB iterations |
|[`'Validation'`](#Validation)|`0.1`| | Percentage of training data used for validation |
|[`'ValidationLoss'`](#ValidationLoss)|`PPS`| | Validation loss over VB iterations |
|[`'Verbose'`](#Verbose)|`true`| | Flag to show real-time fitting information or not |
|[`'WindowSize'`](#WindowSize)|`50`| | Rolling window size to smooth the lowerbound |

<!--BatchSize-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'BatchSize'</span> - Mini-batch size</h3></header>
{: #BatchSize}

#### Data Type: Integer | positive  
<br>
The size of the mini-batch used in each VB iteration. For numerical stabability of the VB algorithm, `'BatchSize'` should be a large number (e.g. 1000, 5000)
compared to the mini-batch size in deep learning literature (e.g. 32, 128, 256).

Must be a positive integer equal or smaller than number of observations of training data. 

By default, `'BatchSize'` is set to `None` indicating that all training data is used in each VB iteration. 

**Note:** This option is only available for cross-sectional data. 

**Default:** `None`

**Example:** `'BatchSize',1000`
</div>

<!--GradWeight1-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'GradWeight1'</span> - Adaptive learning weight 1</h3></header>
{: #GradWeight1}

#### Data Type: Double
<br>
The adaptive learning rate $\beta_1$ associated with the $\bar{g}$ component to update variational parameters in each VB iteration in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb), 

Must be a number between $0$ and $1$.

**Default:** `0.9`

**Example:** `'GradWeight1',0.95`
</div>

<!--GradWeight2-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'GradWeight2'</span> - Adaptive learning weight 2</h3></header>
{: #GradWeight2}

#### Data Type: Double
<br>
The adaptive learning rate $\beta_2$ associated with the $\bar{v}$ component to update variational parameters in each VB iteration in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb).

Must be a number between $0$ and $1$.

**Default:** `0.9`

**Example:** `'GradWeight2',0.95`
</div>

<!--GradientMax-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'GradientMax'</span> - Gradient clipping threshold</h3></header>
{: #GradientMax}

#### Data Type: Double | Positive
<br>
The maximum value of $\bar{g}$ in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb) to prevent the exploding gradient problem occurs when the gradient gets too large, thus making the optimization for the model parameters (e.g., using gradient descent) highly unstable.

**Default:** `100`

**Example:** `'GradientMax',10`
</div>

<!--InitMethod-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'InitMethod'</span> - Initialization method</h3></header>
{: #InitMethod}

#### Data Type: string
<br>
Intialization method of variational parameters, can be specified as following options:
- `'Random'`: Initialize variational parameters from a normal distribution with zero mean and a scaled diagnal covariance matrix $\mathcal{N}(0_D, cI_D)$ with $D$ number of variational parameters, $c$ a constant term and $I_D$ an indentity matrix size $D$. The constant $c$ can be specified using the [`'SigInitScale'`](#SigInitScale) argument. 
- `'Custom'`: Initalize variational parameters using custom method provided by the model object. The model object has to have a method named `initParams()` to inialize variational parameter. 
- `'Value'`: Initialize variational parameters using values specified by the [`'InitValue'`](#InitValue) argument. 

**Default:** `'Random'`

**Example:** `'InitMethod','Custom'`
</div>

<!--InitValue-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'InitValue'</span> - Initial values of varitional mean</h3></header>
{: #InitValue}

#### Data Type: Column vector 
<br>
The column vector of initial values of variational parameters. For example, we can use the point estimation of model parameters from MLE to initialize the VB techniques. 

**Default:** `None`

**Example:** `'InitValue',zeros(D,1)`
</div>

<!--LBPlot-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'LBPlot'</span> - Flag to plot the lowerbound or not</h3></header>
{: #LBPlot}

#### Data Type: True | False
<br>
Flag to plot the smoothed lowerbound over iterations to quicly check the convergence of the VB algorithm.  

**Default:** `true`

**Example:** `'LBPlot',false`
</div>

<!--LearningRate-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'LearningRate'</span> - Fixed learning rate</h3></header>
{: #LearningRate}

#### Data Type: Double | Between 0 and 1
<br>
The fixed learning rate $\epsilon_0$ to update the variational parameters in each VB iteration in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb).

Must be a number between $0$ and $1$.

**Default:** `0.01`

**Example:** `'LearningRate',0.001`
</div>

<!--MaxIter-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'MaxIter'</span> - Maximum number of iterations</h3></header>
{: #MaxIter}

#### Data Type: Integer | Positive
<br>
Maximum number of VB iterations for early stopping. If the [`'BatchSize'`](#BatchSize) argument is specified, users have to use the [`'MaxEpoch'`](#MaxEpoch) argument to specify the maximum number of iterations instead. 

**Default:** `1000`

**Example:** `'MaxIter',1000`
</div>

<!--MaxEpoch-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'MaxEpoch'</span> - Maximum number of epochs</h3></header>
{: #MaxEpoch}

#### Data Type: Integer | Positive
<br>
The maximum number of epochs that will be used for training. An epoch is defined as the number of iterations needed for optimization
algorithm to scan entire training data.

**Note:** This option is only available for cross-sectional data. 

**Default:** `None`

**Example:** `'MaxEpoch',100`
</div>

<!--MaxPatience-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'MaxPatience'</span> - Maximum patience for early stopping  </h3></header>
{: #MaxPatience}

#### Data Type: Integer | Positive
<br>


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

<!--TrainingLoss-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'TrainingLoss'</span> - Training loss over VB iterations </h3></header>
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

## Reference
[1] Tran, M.-N., Nguyen, T.-N., Nott, D., and Kohn, R. (2020). Bayesian deep net GLM and GLMM. *Journal of Computational and Graphical Statistics*, 29(1):97-113. [Read the paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747)

---

## See Also
{: #see-also}
[VAFC]({{site.baseurl}}{%link VB-VAFC.md%}) $\mid$ [NAGVAC]({{site.baseurl}}{%link VB-NAGVAC.md%}) $\mid$ [MGVB]({{site.baseurl}}{%link VB-MGVB.md%})