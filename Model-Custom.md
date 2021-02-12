---
layout: default
title: VBLab Custom Models
parent: Models API Reference
nav_order: 4
permalink: /model/custom/
---

# **VBLab Custom Model**
{: .fs-8}

VBLab provides several ways for custom-built models that work with VB algorithm classes such as [CGVB]({{site.baseurl}}{% link VB-CGVB.md %}),
[VAFC]({{site.baseurl}}{% link VB-VAFC.md %}), [MGVB]({{site.baseurl}}{% link VB-MGVB.md%}) and [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}).

---

## Define custom models as function handlers
{: #custom-handler}
The supported VB algorithms take statistical models and training data as inputs in order to compute the $\Delta_\theta h(\theta)$ and $h(\theta)$ terms. These model- and data-specific information can be provided to the VB algorithms as a function handler with some rules. The following template is suggested when users define their statistical models as functions to compute the $\Delta_\theta h(\theta)$ and $h(\theta)$ terms.

```m
function [h_func_grad,h_func] = grad_h_func(data,theta,setting)
end
```
It is not required to use to same name for function and input/output arguments but the input and outputs arguments have to be defined exactly in the same order. Descriptions for input and output arguments are as follow

| Input | Description|
|:-----|:------|
|`data`   | &bull; The training data. Denote as $y$ in VB algorithms. <br> &bull; This is also the `data` arguments when calling VB classes such as [CGVB]({{site.baseurl}}{% link VB-CGVB.md %}), [VAFC]({{site.baseurl}}{% link VB-VAFC.md %}), [MGVB]({{site.baseurl}}{% link VB-MGVB.md%}) and [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}).|
|`theta`  | &bull; A column vector of model parameters in each VB iteration. Denote as $\theta$ in VB algorithms. <br> &bull; For GVB, $\theta$ is generated from a multivariate normal distribution. Then if $\theta$ is restricted, e.g. prior distribution is not normal, then a transformation of $\theta$ to original scale, together with the jacobian, have to be provided. |
|`setting`| &bull; Additional settings of the custom models. <br> &bull; For example, prior distribution or some constant terms to define the models.|
|`h_func_grad`|  &bull; Gradient vector $\Delta_\theta h(\theta) = \Delta_\theta \text{log}p(y \mid \theta) + \Delta_\theta \text{log} p(\theta)$.<br> &bull; Must be a column vector $1 \times D$ with $D$ the number of model parameters.|
|`h_func`| &bull; $h(\theta) = \text{log} p(y \mid \theta) + \text{log} p(\theta)$. <br> &bull; Must be a scalar. |

**Note:** 
- If the statistical models are defined as function handlers, users have to specify the number of model parameters explicitly using the `'NumParams'` argument of VB classes.
- This approach is more suitable for simple models. For complicated models, it is advisable to define custom models as Matlab classes, which will be discussed in the next section.  

### Example: Define a Logistic Regression model as a function handler. [Github code](https://github.com/VBayesLab/VBLab/tree/main/VBLab){: .fs-4 .btn .btn-purple  .float-right}
{: #example-handler}

This example shows how to define a Logistics Regression model as function handlers to with the VB algorithm supported by the VBLab package. See mathematical derivation of the $\Delta_\theta h(\theta)$ and $h(\theta)$ terms of Logistics Regression model in [the tutorial example 3.4](/VBLabDocs/tutorial/example#example3-4). 

First, load the [LabourForce](/VBLabDocs/datasets/#labour-force) data as a matrix. The last column is the response variable. 
```m
% Load the LabourForce dataset
labour = readData('LabourForce',...
                  'Type','Matrix',...
                  'Intercept',true);
```
Prepare some variables needed to run the VB algorithm and to define the custom model.
```m
% Number of input features
n_features = size(labour,2)-1;
setting.Prior = [0,50];
```
In this example, we use the CGVB technique to fit the custom model on the <samp>labour</samp> data. To run the CGVB technique with the custom model, we need to specify the handler of the function defining the custom model as the first input argument. We also need to explicitly provide the number of model parameters for the `'NumParams'` argument. Finally, we need to pass additional information, e.g. priors, stored in the struct `setting` to the `'Setting'` argument. This `setting` variable then will be passed to the custom function as the input argument.

```m
% Run CGVB to obtain VB approximation of the posterior distribution
Post = CGVB(@grad_h_func_logistics,...  % Function handler to define the custom model
            labour,...                  % Training data    
            'NumParams',num_feature,... % Number of model parameters
            'Setting',setting,...       % Additional setting of the custom models
            'LearningRate',0.002,...    % Learning rate
            'NumSample',50,...          % Number of samples to estimate gradient of lowerbound
            'MaxPatience',20,...        % For Early stopping
            'MaxIter',5000,...          % Maximum number of iterations
            'InitMethod','Random',...   % Randomly initialize variational mean
            'GradWeight1',0.9,...       % Momentum weight 1
            'GradWeight2',0.9,...       % Momentum weight 2
            'WindowSize',10,...         % Smoothing window for lowerbound
            'GradientMax',10,...        % For gradient clipping
            'LBPlot',true);             % Plot the lowerbound when finish

```
The output `Post` is a struct storing information about estimation results and the VB algorithm. See the output of [CGVB]({{site.baseurl}}{% link VB-CGVB.md %}),
[VAFC]({{site.baseurl}}{% link VB-VAFC.md %}), [MGVB]({{site.baseurl}}{% link VB-MGVB.md%}) and [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}) for more details.  

We then define the function `grad_h_func_logistics` to compute the $\Delta_\theta h(\theta)$ and $h(\theta)$ terms using their mathematical derivation shown in [the tutorial example 3.4](/VBLabDocs/tutorial/example#example3-4).

```m
%% Define gradient of h function for Logistic regression 
% theta: Dx1 array
% h_func: scalar
% h_func_grad: Dx1 array
function [h_func_grad,h_func] = grad_h_func_logistics(data,theta,setting)

    % Extract additional settings
    d = length(theta);
    sigma2 = setting.Prior(2);
    
    % Extract data
    X = data(:,1:end-1);
    y = data(:,end));
    
    % Compute log likelihood
    aux = X*theta;
    llh = y.*aux-log(1+exp(aux));
    llh = sum(llh);  
    
    % Compute gradient of log likelihood
    ppi       = 1./(1+exp(-aux));
    llh_grad  = X'*(y-ppi);

    % Compute log prior
    log_prior =-d/2*log(2*pi)-d/2*log(sigma2)-theta'*theta/sigma2/2;
    
    % Compute gradient of log prior
    log_prior_grad = -theta/sigma2;
    
    % Compute h(theta) = log p(y|theta) + log p(theta). Must be a scalar
    h_func = llh + log_prior;
    
    % Compute gradient of the h(theta)
    h_func_grad = llh_grad + log_prior_grad;

    % h_func_grad must be a Dx1 column
    h_func_grad = reshape(h_func_grad,length(h_func_grad),1);
end    
```
**Note:** 
- The <samp>grad_h_func_logistics()</samp> function can be defined in a separated Matlab script named *grad_h_func_logistics.m* or defined in the same script running the example. For the latter case, the function has to be defined **at the end of the script**. 
- In some cases, deriving the gradient $\Delta_\theta h(\theta)$ can be troublesome. We can compute it using Matlab's Automatic Differentiation facility, which is a technique for evaluating derivatives numerically and automatically. See [Matlab Automatic Differentiation tutorials](https://mathworks.com/help/deeplearning/ug/deep-learning-with-automatic-differentiation-in-matlab.html).

---

## Define custom models as Matlab Class Objects
{: #class-model}

For complicated models, it is more efficient to define the models as Matlab classes. The model-specific information can be stored in classes' properties and methods. The following template is suggested to define the custom models as classes: 

```m
classdef CustomModel
    
    % Define model-specific properties
    properties
        ModelName      % Model name 
        NumParam       % Number of parameters
        Post           % Struct to store training results (maybe not used)   
    end
    
    % Define model-specific methods
    methods
        % Constructor. This will be automatically called when users create a CustomModel object
        function obj = CustomModel(inputArg1,inputArg2)
        end
        
        % Function to compute gradient of h_theta and h_theta
        function [h_func_grad, h_func] = hFunctionGrad(obj,data,theta)
        end  
    end
end
```

### Example: Define a VAR(1) model as a Matlab class. [Github code](https://github.com/VBayesLab/VBLab/tree/main/VBLab){: .fs-4 .btn .btn-purple .float-right}
{: #example-class}

---