---
layout: default
title: MGVB for RECH
parent: Code examples
nav_order: 3
permalink: /example/mgvb-rech
---

# **MGVB for RECH**  
This example showing how to fit a RECH model of Nguyen et al. (2020) on SP500 return data using the MGVB technique of Tran et al. (2020).

---

We model the financial volatility using the SRN-GARCH specification of RECH models. First, load the SP500 daily returns together with realized measures from the [RealizedLibrary](/VBLabDocs/datasets/#realized-library) data using the <samp>readData()</samp> function. To access the model performance, we use the realized measure `rk_parzen` as the proxy to the volatility. See the [list of available realized measures](/VBLabDocs/datasets/#list-assets).

```m
% Load the SP500 daily return data
sp500 = readData('RealizedLibrary',...
                 'Index','SP500',...
                 'RealizedMeasure','rk_parzen',...
                 'Length',4000);
```
Create a RECH model object and specify priors following Nguyen et al. (2020). 
{%raw%}
```m
% Define priors for model parameters using 2D cell array
% Parameter names must be specified correctly
prior = {{'v','w','b'},'Normal',[0,0.1];...
         {'beta0','beta1'},'Uniform',[0,0.5];...
         {'alpha','beta'},'Uniform',[0,1]};
% Define a RECH model with SRN-GARCH specification		 
Mdl = RECH('SRN-GARCH',...
           'Prior', prior);
```
{%endraw%}

Split `abalon` to training and test data then fit the DeepGLM model `Mdl` to training set using NAGVAC technique. 
```m
% Train/Test split
[abalon_train, abalon_test] = trainTestSplit(abalon,0.15);
% Run VAFC to obtain VB approximation of the posterior distribution
EstMdl = NAGVAC(Mdl,abalon_train,...
                'LearningRate',0.01,...     % Use a small learning rate
                'MaxPatience',100,...       % Maximum patience
                'WindowSize',100,...        % Rolling window size to smooth the lowerbound
                'MaxIter',5000,...          % Maximum number of iterations    
                'InitMethod','Random',...   % Initialization method
                'GradientMax',100,...       % Gradient clipping threshold
                'LBPlot',true);             % Plot the lowerbound when finish
```
Once the model `Mdl` is fitted, we can see the convergence of the lowerbound, showing that the NAGVAC algorithm works properly. 
<img src="/VBLabDocs/assets/images/Example-NAGVAC-DeepGLM.jpg" class="center"/>

We can plot the shrinkage coefficients over iterations to explore the significance of the covariates in terms of explaining the
response $y$, which is the age (in years) of the rings. The following plot shows that the $2^{nd}$ (Sex) and $8^{th}$ (Viscera weight) covariates are less important than the others and can be removed from data.  
```m             
% Plot shrinkage coefficients
figure
vbayesPlot('Shrinkage',EstMdl.Post.shrinkage,...
           'Title','Shrinkage Coefficients',...
           'Xlabel','Iterations',...
           'LineWidth',2);
```
<img src="/VBLabDocs/assets/images/Example-NAGVAC-DeepGLM-Shrinkage.jpg" class="center"/>

Given the test input, we can make prediction for the output using [<samp>vbayesPredict</samp>]({{site.baseurl}}{% link Model-Predict.md%}) method of the DeepGLM class object. 
```m
% Make prediction (point estimation) on a test set
X_test = abalon_test(:,1:end-1);
y_test = abalon_test(:,end);
Pred1 = vbayesPredict(EstMdl,X_test);
```
I can compute predictive scores if true responses are provided.
```m
% If y_test is specified (for model evaluation purpose)
% then we can check PPS and MSE on test set
Pred2 = vbayesPredict(EstMdl,X_test,'Ytest',y_test);
disp(['PPS on test set using deepGLM is: ',num2str(Pred2.pps)])
disp(['MSE on test set using deepGLM is: ',num2str(Pred2.mse)])
```
```yml
PPS on test set using deepGLM is: 1.3042
MSE on test set using deepGLM is: 4.962
```
Or we can make the interval estimation for test observation and compute the accuracy.
```m
% Estimate prediction interval for entire test data
Pred3 = vbayesPredict(mdl,X_test,...
                      'Ytest',y_test,...
                      'Interval',1,...
                      'Nsample',1000);                       
y_pred = mean(Pred3.yhatMatrix)';
mse2 = mean((y_test-y_pred).^2);
accuracy = (y_test<Pred3.interval(:,2) & y_test>Pred3.interval(:,1));
disp(['Prediction Interval accuracy: ',num2str(sum(accuracy)/length(accuracy))]);
```
```yml
Prediction Interval accuracy: 0.76794
```
It is also useful to plot the prediction interval together with the true test responses. 
```m
figure
vbayesPlot('Interval',Pred3,...
           'Ytest',y_test,...
           'Title','Prediction Interval for Test Data',...
           'Xlabel','Observations',...
           'Ylabel','Age(years)',...
           'Nsample',40);           
```

<img src="/VBLabDocs/assets/images/Example-DeepGLM-Abalon.jpg" class="center"/>

--- 

## Reference
[1] Nguyen, T.-N., Tran, M.-N., and Kohn, R. (2020). Recurrent conditional heteroskedasticity. arXiv:2010.13061. [Read the paper](https://arxiv.org/abs/2010.13061)
