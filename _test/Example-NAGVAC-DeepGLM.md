---
layout: default
title: NAGVAC for DeepGLM
parent: Code examples
nav_order: 2
permalink: /example/nagvac-deepglm
---

# **NAGVAC for DeepGLM**  
This example replicates the application with the Abalon data discussed in the [DeepGLM paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747) of Tran et al. (2020). 

---

We model the ages of abalons using a deepGLM model. First, load the [Abalon dataset](/VBLabDocs/datasets/#abalon) using the <samp>readData()</samp> function. 

```m
% Load the LabourForce dataset
abalon = readData('Abalon',...
                  'Type','Matrix',...  % Format data as a matrix
                  'Intercept',true,... % Add a column of 1 as intecepts
                  'Normalized',true);  % Normalize numerial input features 
```
Then, define a [DeepGLM]({{site.baseurl}}{%link Model-DeepGLM.md%}) model object. 
```m
% Number of input features
n_features = size(abalon,2)-1;
% Neural network structure with 2 hidden layers with 5 hidden nodes for each
network = [n_features,5,5]
% Define a deepGLM model object
Mdl = DeepGLM(network);
```
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

Given the test input, we can make prediction for the output using <samp>vbayesPredict</samp> method of the DeepGLM class object. 
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
[1] Tran, M.-N., Nguyen, T.-N., Nott, D., and Kohn, R. (2020). Bayesian deep net GLM and GLMM. *Journal of Computational and Graphical Statistics*, 29(1):97-113. [Read the paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747)