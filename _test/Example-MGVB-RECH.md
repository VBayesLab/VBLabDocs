---
layout: default
title: MGVB for RECH
parent: Code examples
nav_order: 3
permalink: /example/mgvb-rech
---

# **MGVB for RECH**   [Github code](https://github.com/VBayesLab/VBLab/blob/main/Example/MGVB_RECH.m){: .fs-4 .btn .btn-purple .float-right}
This example showing how to fit a RECH model of Nguyen et al. (2020) on SP500 return data using the MGVB technique of Tran et al. (2020).

---

Let $y=\{y_t,\ t=1,...,T\}$ be a time series of financial asset returns and $\mathcal F_t$ be the $\sigma$-field of the information up to time $t$. 
Volatility, defined as the conditional variance $\sigma_t^2:=\text{Var}(y_t|\mathcal F_{t-1})$, is of high interest in the financial sector.

The conditional variance in the RECH models is the sum of two components: the recurrent component modeled by an RNN, and the garch component modeled by a GARCH-type structure. 
For example, by using the Simple Recurrent Network (SRN) for the recurrent component $\omega_t$ and the standard GARCH(1,1) for the garch component, they obtain the SRN-GARCH specification of the RECH models as:

$$\begin{eqnarray}
	y_t &=& \sigma_t\epsilon_t,\;\;\epsilon_t\stackrel{iid}{\sim}\mathcal{N}(0,1),\;\;t=1,2,...,T  \\
	\sigma_t^2 &=& \omega_t+ \alpha y_{t-1}^2 + \beta \sigma_{t-1}^2,\;\;t=2,...,T,\;\; \sigma^2_1=\sigma^2_0  \\
	\omega_t &=& \beta_0+\beta_1 h_t,\;\; t=2,...,T,\\
	h_t &=& \phi(v x_t  + wh_{t-1}+b),\;\;t=2,...,T,\;\; \text{with} \;\; h_1 \equiv 0;
\end{eqnarray}$$

Nguyen et al. (2020) suggest $x_t=(\omega_{t-1},y_{t-1},\sigma^2_{t-1})^\top$. 
The SRN-GARCH model has 7 parameters: $\theta = (\alpha,\beta,\beta_0,\beta_1,v,w,b)$. 

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

Split the return series to in-sample data for model fitting and out-of-sample data for forecasting. Then fit the RECH model `Mdl` to in-sample data using MGVB technique. 
```m
% Train/Test split
sp500_return = sp500.y;
[sp500_in, sp500_out] = trainTestSplit(sp500_return,0.5,...
                                       'Shuffle',false);
% Run MGVB to obtain VB approximation of the posterior distribution
[~,Estmdl] = MGVB(mdl,sp500_in,...
                  'NumSample',100,...
                  'LearningRate',0.01,...
                  'GradWeight',0.4,...
                  'MaxPatience',50,...
                  'MaxIter',2500,...
                  'GradientMax',100,...
                  'WindowSize',30,...
                  'LBPlot',false);         % We will plot LB manual together with variational distribution
```
Once the model `Mdl` is fitted, we can manually plot the variational distribution together with the lowerbound. The convergence of the lowerbound shows that the MGVB works properly. 
```m
figure
% Extract variation mean and variance
mu_vb = Estmdl.Post.mu;
sigma2_vb = Estmdl.Post.sigma2;
param_name = Estmdl.ParamName;
% Plot the variational distribution of each parameter
for i=1:length(mu_vb)
    subplot(3,3,i)
    vbayesPlot('Density',...
               'Distribution',{'Normal',[mu_vb(i),sigma2_vb(i)]})
    grid on
    title(param_name{i})
    set(gca,'FontSize',15)
end
```
<img src="/VBLabDocs/assets/images/Example-RECH-distribution.jpg" class="center"/>

We can use the variational mean to make the one-step-ahead forecast of volatility. 
```m
sp500_vol_forecast = vbayesPredict(Estmdl,...
                                   'Ytest',sp500_out);
```
If realized measure of the volatiliy is provided, we can compute predictive scores in different metrics. In this example, we use the Realized Kernel Variance `rk_parzen`.  
```m
% Extract realized measure of volatility. 
sp500_vol = sp500.rk_parzen;
[~, sp500_vol_out] = trainTestSplit(sp500_vol,0.5,...
                                    'Shuffle',false);
sp500_vol_forecast = vbayesPredict(Estmdl,...
                                   'Ytest',sp500_out,...
                                   'Voltest',sp500_vol_out,...
                                   'Score',{'MSE','MAE'});
```
Finall, we can plot the volatility forecast and realized volatility.
```m
% Compute constant term
c = mean((sp500_out-mean(sp500_out)).^2)/mean(sp500_vol_out);
% Plot volatility forecast and realized volatility
figure
hold on
grid on
plot(sp500_vol_forecast,'-r','LineWidth',2)
p1 = plot(sp500_vol_out,'--g','LineWidth',1);
p1.Color(4) = 0.4;
xlabel('Time')
ylabel('Conditional Variance')
legend({'\sigma^2_{t,SRN-GARCH}','\sigma^2_{t,realized}'},'FontSize',16)
set(gca,'FontSize',16);
ylim([0,6])
``` 

<img src="/VBLabDocs/assets/images/Example-RECH-forecast.JPG" class="center"/>

---

## Reference
[1] Nguyen, T.-N., Tran, M.-N., and Kohn, R. (2020). Recurrent conditional heteroskedasticity. arXiv:2010.13061. [Read the paper](https://arxiv.org/abs/2010.13061)

[2] Tran, M.-N., Nguyen, D. H., and Nguyen, D. (2020). Variational Bayes on manifolds. Technical report. arXiv:1908.03097.[Read the paper](https://arxiv.org/abs/1908.03097)