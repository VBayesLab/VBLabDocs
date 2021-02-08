---
layout: default
title: CGVB for Bayesian Logistic Regression
parent: Code examples
nav_order: 1
permalink: /example/cgvb-logistic-3-4
---

# **CGVB for Bayesian Logistic Regression model**
This example implements the [example 3.4](/VBLabDocs/tutorial/example#example3-4) shown in the VB tutorial paper.  

Load the LabourForce data as a matrix. The last column is the response variable.

```m
% Load the LabourForce dataset
labour = readdata('LabourForce',...
                  'Type','Matrix',...
                  'Intercept',true);
```
Create a LogisticRegression model object by specifying the number of parameters as the input argument. Use the default priors for model coefficients. 
```m
% Number of input features
n_features = size(labour,2)-1;
% Define a LogisticRegression model object
Mdl = LogisticRegression(n_features);
```
Run CGVB to obtain VB approximation of the posterior distribution of model parameters. 
```m
% Run CGVB to obtain VB approximation of the posterior distribution
Estmdl = CGVB(Mdl,labour_train,...
              'LearningRate',0.002,...  % Learning rate
              'NumSample',50,...        % Number of samples to estimate gradient of lowerbound
              'MaxPatience',20,...      % For Early stopping
              'MaxIter',5000,...        % Maximum number of iterations
              'InitMethod','MLE',...    % Use estimation from MLE as initial parameters
              'GradWeight1',0.9,...     % Momentum weight 1
              'GradWeight2',0.9,...     % Momentum weight 2
              'WindowSize',10,...       % Smoothing window for lowerbound
              'GradientMax',10,...      % For gradient clipping
              'LBPlot',true);           % Plot the lowerbound when finish
```
The plot of lowerbound shows that the CGVB algorithm converges well. 

<img src="/VBLabDocs/assets/images/example3-4-lowerbound.jpg" class="center"/>

We can check how close of the variational distribution to the true posterior densities of model paramters. We run MCMC to obtain samples of model parameters from theirs posterior distribution. 

```m
% Run MCMC
Post_MCMC = MCMC(Mdl,...
                 'NumMCMC',50000);    % Number of samples (MCMC iterations)
```
```m  
% Compare VB and MCMC
% Get posterior mean and trace plot for a parameter to check the mixing 
[mcmc_mean,mcmc_std,mcmc_chain] = Post_MCMC.getParamsMean('BurnInRate',0.4,...
                                                          'PlotTrace',1);

% Plot density
fontsize  = 20;
numparams = Mdl.ParamNum;

% Extract variation mean and variance
mu_vb     = Estmdl.Post.mu;
sigma2_vb = Estmdl.Post.sigma2;

% Compute MLE estimation
X = [ones(size(X,1),1),data(:,2:end)];
[mu_mle,~,stats] = glmfit(X,y,'binomial','constant','off'); 

figure
for i = 1:numparams
    subplot(3,3,i)
    xx = mcmc_mean(i)-4*mcmc_std(i):0.002:mcmc_mean(i)+4*mcmc_std(i);
    yy_mcmc = ksdensity(mcmc_chain(:,i),xx);    
    yy_vb = normpdf(xx,mu_vb(i),sqrt(sigma2_vb(i)));    
    plot(xx,yy_mcmc,'-k',xx,yy_vb,'--b','LineWidth',1.5)
    line([mu_mle(i) mu_mle(i)],ylim,'LineWidth',1.5,'Color','r')    
    str = ['\theta_', num2str(i)];   
    title(str,'FontSize', fontsize)
    legend('MCMC','VB')
end
subplot(3,3,9)
plot(Estmdl.Post.LB_smooth,'LineWidth',1.5)
title('Lower bound','FontSize', fontsize)
```
<img src="/VBLabDocs/assets/images/Example3-4-code.JPG" class="center"/>
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     