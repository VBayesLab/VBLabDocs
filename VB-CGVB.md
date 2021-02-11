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

[Tutorial]({{site.baseurl}}{% link Tutorial-FFVB-CGVB.md%}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [GitHub code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/VB/CGVB.m){: .btn .fs-5 .mb-4 .mb-md-0 }

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
|[`'Validation'`](#Validation)|`0.1`| | Subset of training data used for validation |
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
Intialization method of variational mean, can be specified as following options:

|:---|:---|
|`'Random'`| Initialize variational mean from a normal distribution with zero mean and a diagnal covariance matrix $\mathcal{N}(0_D, \sigma I_D)$ with $D$ number of variational parameters, $\sigma$ a constant term and $I_D$ an indentity matrix size $D$. The constant $\sigma$ can be specified using the [`'StdForInit'`](#StdForInit) argument|
|`'Custom'`| Initalize variational parameters using custom method provided by the model object. The model object has to have a method named `initParams()` to inialize variational parameter|
|`'Value'`|Initialize variational parameters using values specified by the [`'InitValue'`](#InitValue) argument|

**Note:** The the variational parameters associated with the lower triangular matrix $L$ is set as a diagnal matrix $c I_D$ with $c$ is another contant term which can be specified using the [`'SigInitScale'`](#SigInitScale) argument.  

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
Number of consecutive times that the validation loss, or lowerbound, is allowed to be larger than or equal to the previously smallest loss, or lowerbound, before the training is stopped, used as an early stopping criterion. This is denoted as $P$ in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb). 

**Default:** `20`

**Example:** `'MaxPatience',100`
</div>

<!--NumSample-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'NumSample'</span> - Monte Carlo samples to estimate the lowerbound</h3></header>
{: #NumSample}

#### Data Type: Integer | Positive
<br>
Number of Monte Carlo samples needed to estimate the gradient of the lower bound. This is denoted as $S$ in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb). 

**Default:** `50`

**Example:** `'NumSample',100`
</div>

<!--NumParams-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'NumParams'</span> - Number of model parameters </h3></header>
{: #NumParams}

#### Data Type: Integer | Positive
<br>
Number of model parameters. 
- If the handle of the function calculating the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms is provided, users have to specify a value for this argument. 
- If a model object is specified, users have to set the number of parameters using the `NumParams` property of the model class. See [how to define a custom model as a Maltab class object](/VBLabDocs/model/custom/#class-model).

**Default:** `None`

**Example:** `'NumParams',{'Normal',[0,10]}`
</div>

<!--SaveParams-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'SaveParams'</span> - Flag to save training parameters or not </h3></header>
{: #SaveParams}

#### Data Type: true | false
<br>
Flag to save variational parameters in each VB iteration. 

**Default:** `false`

**Example:** `'SaveParams',true`
</div>


<!--Setting-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Setting'</span> - Additional setting for custom models </h3></header>
{: #Setting}

#### Data Type: struct
<br>
Additional settings that could be use to define custom models as function handlers. 

The most efficient way to define these additinal as a struct. This struct then will be passed to the function handlers as an input. See [how to define custom model as function handler](/VBLabDocs/model/custom/#custom-handlers).

**Default:** `None`

**Example:** `'Setting',prior` with `prior` is a struct whose fields are prior distribution name and parameters, e.g. `prior.name = 'Normal'` and `prior.params = [0,1]`. 
</div>

<!--SigInitScale-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'SigInitScale'</span> - Constant factor for initialization </h3></header>
{: #SigInitScale}

#### Data Type: double
<br>
The constant factor $c$ to scale the initial values for the lower triangular matrix $L$.  

**Default:** `0.1`

**Example:** `'SigInitScale',0.5`
</div>


<!--StdForInit-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'StdForInit'</span> - Standard deviation of normal distribution for initialization</h3></header>
{: #StdForInit}

#### Data Type: double
<br>
The constant factor $\sigma$ to scale the convariance matrix of the normal distribution used to initialize the variational mean. 
 
Only specify this argument when the argument [`'InitMethod'`](#InitMethod) is set to `'Random'`.

**Default:** `0.01`

**Example:** `'StdForInit',0.04`
</div>

<!--StepAdaptive-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'StepAdaptive'</span> - Threshold to start reducing learning rates </h3></header>
{: #StepAdaptive}

#### Data Type: Integer | Positive 
<br>
The iteration to start reducing learning rate, which is denote as $\tau$ in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb). 

By default, this is set as `'MaxIter'/2` or `'MaxEpoch'/2`. 

Must be smaller than `'MaxIter'` or `'MaxEpoch'`. 

**Default:** `'MaxIter'/2` or `'MaxEpoch'/2`

**Example:** `'StepAdaptive',300`
</div>

<!--TrainingLoss-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'TrainingLoss'</span> - Training loss over VB iterations </h3></header>
{: #StepAdaptive}

#### Data Type: string | cell array of strings
<br>
The VB algorithm uses lowerbound to access the convergence of the training phase. 

However, users can also calculate the predictive scores evaluated on the training data over VB iterations. Users can specify a single metric, defined as a string, or multiple metrics, defined as a cell array of strings. 

Available score metrics:

|:---|:----|
|PPS | Partial Predictive Score|
|MSE | Mean Squared Errors (for continuos output)|
|MAE | Mean Absoluted Errors (for continuos output)|
|CR  | Classification rate (for binary output)|

For the PPS:
- If the models are specified as function handlers, users have to also specify function handlers to the argument `'LogLikFunc'` to compute the log-likelihood of the custom models. 
- If the models are specified as class objects, users have to define a method named `logLik()` to compute the log-likelihood of the custom models. 

**Default:** `None`

**Example:** `'TrainingLoss',{'PPS','MSE'}`
</div>

<!--Validation-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Validation'</span> - Subset of training data used for validation </h3></header>
{: #Validation}

#### Data Type: double between 0 and 1 | Integer
<br>
Number of observations of training data are used as validation data. The number of observations can be specified as a percentage (a number between 0 and 1) of training data or an integer smnaller than the number of training observations.

**Note:** This option is only available for cross-sectional (tabular) data. 

**Default:** `None`

**Example:** `'Prior',0.1` or `'Prior',1000`
</div>

<!--ValidationLoss-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'ValidationLoss'</span> - Validation loss computed during fitting phase </h3></header>
{: #ValidationLoss}

#### Data Type: string | cell array of strings

Calculate the predictive scores evaluated on the validation data over VB iterations. Users can specify a single metric, defined as a string, or multiple metrics, defined as a cell array of strings. 

Available score metrics:

|:---|:----|
|PPS | Partial Predictive Score|
|MSE | Mean Squared Errors (for continuos output)|
|MAE | Mean Absoluted Errors (for continuos output)|
|CR  | Classification rate (for binary output)|

For the PPS:
- If the models are specified as function handlers, users have to also specify function handlers to the argument `'LogLikFunc'` to compute the log-likelihood of the custom models. 
- If the models are specified as class objects, users have to define a method named `logLik()` to compute the log-likelihood of the custom models. 

**Note:** This option is only available for cross-sectional (tabular) data. 

**Default:** `None`

**Example:** `'ValidationLoss','MSE'`
</div>

<!--Verbose-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Verbose'</span> - Flag to show real-time fitting information or not </h3></header>
{: #Verbose}

#### Data Type: True | False
<br>
By default, the index of the current iteration and lowerbound are shown in every iteration. Set `'Verbose'` to be `false` to turn off these messages. 

**Default:** `true`

**Example:** `'Verbose',false`
</div>

<!--WindowSize-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'WindowSize'</span> - Rolling window size to smooth the lowerbound </h3></header>
{: #WindowSize}

#### Data Type: Integer | Positive
<br>
Size of moving average window that used to smooth the lowerbound. Denoted as $t_W$ in [Algorithm 7](/VBLabDocs/tutorial/ffvb/cgvb#algorithm-7-cholesky-gvb). 

**Default:** `50`

**Example:** `'WindowSize',100`
</div>

</div>

---

## Output Arguments

<!--Post-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Post</span> - Estimation results</header>
#### Data type: struct
<br>
The statistical models containing unknown parameters, specified as:
- [VBLab model object](/VBLabDocs/model/#vblab-model).
- or [function handler to compute the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms](/VBLabDocs/model/custom/#custom-handler).
</div>

<!--EstMdl-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">EstMdl</span> - Model Object </header>
{: #cgvb-object}

#### Data type: VBLab model object| Custom model Object
<br>


</div>

--- 

## Examples

1. [CGVB for Logistic Regression model defined as a LogisticRegression object]({{site.baseurl}}{% link Example-CGVB-Bayesian-Logistics-Regression.md%}) 
2. [CGVB for Logistic Regression model defined as a function handler](/VBLabDocs/model/custom/#example-handler)

---

## Reference
[1] Tran, M.-N., Nguyen, T.-N., Nott, D., and Kohn, R. (2020). Bayesian deep net GLM and GLMM. *Journal of Computational and Graphical Statistics*, 29(1):97-113. [Read the paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747)

---

## See Also
{: #see-also}
[VAFC]({{site.baseurl}}{%link VB-VAFC.md%}) $\mid$ [NAGVAC]({{site.baseurl}}{%link VB-NAGVAC.md%}) $\mid$ [MGVB]({{site.baseurl}}{%link VB-MGVB.md%})