---
layout: default
title: NAGVAC for DeepGLM
parent: Code examples
nav_order: 2
permalink: /example/nagvac-deepglm
---

# NAGVAC for DeepGLM - 

```m
% Prepare data
data = xlsread('../../Data/MROZ.xlsx'); % load the data
y = data(:,1);
X = data(:,2:end);
% X = [ones(size(X,1),1),data(:,2:end)];

% Train a deepglm model
nn = [2,2,2];
mdl = deepGLMfit(X,y,... 
                 'Distribution','binomial',...
                 'Network',nn,... 
                 'Lrate',0.001,...           
                 'Verbose',1,...              % Display training result each iteration
                 'BatchSize',50,...           % Use entire training data as mini-batch
                 'MaxEpoch',50,...
                 'Patience',100,...           % Higher patience values could lead to overfitting
                 'Seed',100);
% 
             
% Plot shrinkage coefficients
% figure
deepGLMplot('Shrinkage',mdl.out.shrinkage,...
            'Title','Shrinkage Coefficients',...
            'Xlabel','Iterations',...
            'LineWidth',2);
                
% Fit GLM model
[mu_mle,~,stats] = glmfit(X,y,'binomial','constant','off'); 
```