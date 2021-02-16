---
layout: default
title: DeepGLM Class
parent: Models API Reference
nav_order: 1
permalink: /model/deepglm/
block_color: GhostWhite
---

# <samp>DeepGLM</samp> class [Github code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/Models/DeepGLM.m){: .fs-4 .btn .btn-purple .float-right}
Create DeepGLM model object 
{: .fs-6 .fw-300 }

---

## Syntax

```matlab
Mdl = DeepGLM(Network,Name,Value)
```
---
## Description
`Mdl = DeepGLM(Network,Name,Value)` returns [DeepGLM model object](#deepglm-object) `Mdl` given neural network structure `Network`. `Name,Value` specifies additional options using one or more name-value pair arguments. For example, users can specify the activation function or distribution of the output. 

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
<!--Network-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">Network</span> - Neuron Network structure of the deepGLM model </header>
#### Data type: Array of positive integer
<br>
Neuron Network structure of the deepGLM model. `[NumFeatures, L1,...,LM]`:
<dl>
<dt><code>Numfeatures</code></dt> <dd>Number of features (columns) of training data.</dd>
<dt><code>L1,...,LM</code></dt> <dd>Number of hidden nodes in each hidden layer. For example, <code>L1</code> is the number of hidden nodes in the first hidden layer and so on. </dd>
</dl>
**Note:** The output layer has only 1 node.
</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<!--Name-Value Pairs-->
<span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Name-Value Pair Arguments </span>

Specify optional comma-separated pairs of `Name,Value` arguments. `Name` is the argument name and `Value` is the corresponding value. `Name` must appear inside quotes. You can specify several name and value pair arguments in any order as `Name1,Value1,...,NameN,ValueN`.

**Example:** `'Distribution','Normal','Activation','relu'` specifies that the distribution of the response is normal, and the activation function of hidden layers is the Rectified Linear Unit (ReLU) function.

<!--Distibution-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Distribution'</span> - Distribution of the response variable</h3></header>

#### Data Type: String 
<br>
Distribution of the response variable, specified as the comma-separated pair consisting of <samp>'Distribution'</samp> and one of the following:

| `'Normal'`   | Normal distribution (**default**)     | 
| `'Binomial'` | Binomial distribution                 | 
| `'Poisson'`  | Poisson distribution                  | 

**Example:** <code style="color:#A020F0">'Distribution','Normal'</code>
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

<!--Description-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Description'</span> - Model description</h3></header>

#### Data Type: string
<br>
Model description, specified as a string scalar or character vector. Provide additional information about the model. 

**Default:** Empty string

**Example:** `'Description','DeepGLM with binary output'`
</div>

</div>

---

## Output Arguments
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Mdl</span> - DeepGLM Object </header>
{: #deepglm-object}

#### Data type: DeepGLM Object
<br>
<samp>DeepGLM</samp> is an object of the DeepGLM class with pre-defined properties and functions.

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

### Object Properties
The DeepGLM object properties include information about model-specific information, coefficient estimates and fitting method. 

|Properties|Data type |Description{: .text-center}|
|:-------------|:------------------|:------|
|`ModelName`    |string (r)| Name of the model, which is <samp>'DeepGLM'</samp>|
|`NumParams`    |integer (+) | Number of model parameters|
|`Network`      |array | Neural network structure of DeepGLM models|
|`Distribution` |string | Neural network structure of DeepGLM models|
|`Activation`   |string | Neural network structure of DeepGLM models|
|`Post` *       |struct  | &bull; Information about the fittedd method used to estimate model paramters <br> &bull; The <samp>DeepGLM</samp> model can only be fitted by [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}) and [VAFC]({{site.baseurl}}{% link VB-VAFC.md %}) techniques|
|`Coefficient` * |cell array| &bull; Estimated Mean of weights of Deep Neural Network <br> &bull; Used to doing point estimation for new test data|
|`CoefficientVar` * |cell array (r)| Variance of coefficient estimates|
|`Shrinkage` *    |array| Array storing estimated values of group Lasso coefficients|
|`LogLikelihood` * |double (r)| Loglikelihood of the fitted model. |
|`FittedValue` * |array (r)| &bull; Fitted (predicted) values based on the input data. <br> &bull; For binary response, these are fitted probability|

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

## <span style="font-weight:bold;font-size:20px">Fit a DeepGLM model for continuous response</span> [Github code](https://github.com/VBayesLab/Tutorial-on-VB){: .fs-4 .btn .btn-purple  .float-right}
{: #deepglm-continuous}
Fit a DeepGLM model to [DirectMarketing](/VBLabDocs/datasets/#direct-marketing) data using [VAFC]({{site.baseurl}}{%link VB-VAFC.md%})

Load the DirectMarketing data using the [<span style="font-family:monospace">reaDdata()</span>]({{site.baseurl}}{% link Utilities-Read-Data.md%}) function. 
The data is a matrix with the last column is the response variable. Set the `'Intercept'` argument to be `true` to add a column of 1 to the data matrix as intercepts.  
```matlab
% Load the DirectMarketing dataset
marketing = readData('DirectMarketing',...
                     'Type','Matrix',...
                     'Intercept',true,...
                     'Normalized',true);
```
Create a default DeepGLM model object by passing only neural network structure as the input argument. By default, the response is defined as a normal variable. 
```matlab
% Number of input features
n_features = size(marketing,2)-1;
% Neural network structure with 2 hidden layers with 5 hidden nodes for each
network = [n_features,5,5]
% Define a deepGLM model object
Mdl = DeepGLM(network);
```
Run VAFC to obtain VB approximation of the posterior distribution of model parameters. Use [<span style="font-family:monospace">trainTestSplit()</span>]({{site.baseurl}}{% link Utilities-Read-Data.md%}) function to split `marketing` to training and test data. 
```matlab
% Train/Test split
[marketing_train, markerting_test] = trainTestSplit(marketing,0.2);
% Run VAFC to obtain VB approximation of the posterior distribution
EstMdl = VAFC(Mdl,marketing_train,...
              'LearningRate',0.002,...   % Use a small learning rate
              'NumFactor',4,...          % Use only 4 factors 
              'Validation',0.15,...      % Use 15% number of observations fpr validation    
              'Loss','PPS');             % Use PPS as the predictive metric
```
Given the fitted DeepGLM model `EstMdl`, we can make prediction with new data. Set `'YTest'` to `true` to indicate that the last column of test data contains true responses. Use `'Loss'` argument to specify predictive scores we want to compute.     
```matlab
% Make prediction with new data and compute prediction scores in PPS and MSE.
[Pred,Error] = vbayesPredict(EstMdl,markerting_test,...
                             'YTest',true,...            % There are true response
                             'Loss',{'PPS','MSE'});      % Compute predictive scores in PPS and MSE
```
</div>

<!--Binary response-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

## <span style="font-weight:bold;font-size:20px">Fit a DeepGLM model for binary response</span> [Github code](https://github.com/VBayesLab/Tutorial-on-VB){: .fs-4 .btn .btn-purple  .float-right}
{: #deepglm-binary}
Fit a DeepGLM model to [GermanCredit](/VBLabDocs/datasets/#german-credit) data using [NAGVAC]({{site.baseurl}}{%link VB-NAGVAC.md%}). 

Load the GermanCredit data using the [<span style="font-family:monospace">readdata()</span>]({{site.baseurl}}{% link Utilities-Read-Data.md%}) function. Use the same arguments as in the previous example.  
```matlab
% Load the DirectMarketing dataset
credit = readdata('GermanCredit',...
                  'Type','Matrix',...
                  'Intercept',true,...
                  'Normalized',true);
```
Create a DeepGLM model with binary output and run NAGVAC to obtain VB approximation of the posterior distribution of model parameters. As we create the DeepGLM model with big network, NAGVAC is more efficient than VAFC. 
```matlab
% Number of input features
n_features = size(credit,2)-1;
network = [n_features,100,100]
% Define a deepGLM model object
Mdl = DeepGLM(network,...
              'Distribution','Binomial');   % For binary output
% Train/Test split
[credit_train, credit_test] = trainTestSplit(credit,0.2);
% Run NAGVAC to obtain VB approximation of the posterior distribution
[~,EstMdl] = NAGVAC(Mdl,credit_train,...
                    'Validation',0.15,...      % Use 15% number of observations fpr validation    
                    'Loss','PPS');             % Use PPS as the predictive metric
```
Make prediction with the test data. Given the true labels, we can compute the miss-classification rate (MCR) as the predictive score.  
```matlab
% Make prediction with new data and compute prediction scores in PPS and MCR
[Pred,MCR] = vbayesPredict(EstMdl,credit_test,...
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
[LogisticRegression]({{site.baseurl}}{% link Model-Logistic-Regression.md %}) $\mid$ [RECH]({{site.baseurl}}{% link Model-RECH.md %}) $\mid$ [Custom model]({{site.baseurl}}{% link Model-Custom.md%}) $\mid$ [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}) $\mid$ [VAFC]({{site.baseurl}}{%link VB-VAFC.md%})