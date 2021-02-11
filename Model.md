---
layout: page
title: Models API Reference
description: "Model supported by VBLab"
nav_order: 3
has_children: true
permalink: /model/
img_yes: /VBLabDocs/assets/images/yes.png
img_no: /VBLabDocs/assets/images/no_2.jpg
style: "height: 25px; width:25px;"
style_no: "height: 20px; width:20px;"
has_toc: false
---

# **VBLab Statistical Models**
{: .fs-8}

VBLab provides statistical models that can be used with the supported VB techniques. Users can also defined their custom models. 

---

## VBLab models
{: #vblab-model}
- VBLab provides some statistical models that can be used to quickly implement the Variational Bayes (VB) techniques supported. 
- These models are designed as Matlab class objects with predefined attributes and methods. 
- Available VBLab models: 
    - [DeepGLM]({{site.baseurl}}{% link Model-DeepGLM.md %}): Bayesian Deep Generalized Linear model
        - DeepGLM models [(Tran et al., 2020)](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747) are flexible versions of generalized linear models incorporating basis functions formed by Deep Feedforward Neural Networks (DFNN). 
    - [LogisticRegression]({{site.baseurl}}{% link Model-Logistic-Regression.md %}): Bayesian Logistic Regression model
    - [RECH]({{site.baseurl}}{% link Model-RECH.md %}): Recurrent Conditional Heteroskedasticity model
        - The RECH models are proposed by [Nguyen et al. (2020)](https://arxiv.org/abs/2010.13061)
        
---

## Custom models

There are two ways to define custome models:
- Define custom function to compute the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms then provide the handle of the function as the input model. See [how to define custom models as function handlers](/VBLabDocs/model/custom/#custom-handler).
- Define custom models as Matlab classes with methods to compute the $h(\theta)$ and $\Delta_\theta h(\theta)$ terms. See [how to define custom models as class objects](/VBLabDocs/model/custom/#class-model).

---

## Model compatibility

Theoretically, the provided VB methods can work with all VBLab supported models. However, due to model-specific properties, we recommend the following efficient combinations of VB methods and models.

|                      | CGVB | VAFC  | MGVB | NAGVAC |
|:---------------------|:-----|:------| :----|:-------|
| **DeepGLM**              | <img src="{{page.img_no}}" style="{{page.style_no}}"/> | <img src="{{page.img_yes}}" style="{{page.style}}"/>  |  <img src="{{page.img_no}}" style="{{page.style_no}}"/>    | <img src="{{page.img_yes}}" style="{{page.style}}"/>   |
| **Logistics Regression** | <img src="{{page.img_yes}}" style="{{page.style}}"/> | <img src="{{page.img_yes}}" style="{{page.style}}"/>  | <img src="{{page.img_yes}}" style="{{page.style}}"/>     | <img src="{{page.img_yes}}" style="{{page.style}}"/>   |
| **RECH**                 | <img src="{{page.img_no}}" style="{{page.style_no}}"/> | <img src="{{page.img_no}}" style="{{page.style_no}}"/>   |  <img src="{{page.img_yes}}" style="{{page.style}}"/>     | <img src="{{page.img_no}}" style="{{page.style_no}}"/>   |
| **Custom Models**               | <img src="{{page.img_yes}}" style="{{page.style}}"/> | <img src="{{page.img_yes}}" style="{{page.style}}"/>  |  <img src="{{page.img_yes}}" style="{{page.style}}"/>  | <img src="{{page.img_yes}}" style="{{page.style}}"/>  |

For example, as the MGVB technique does not require the gradient of the log-likelihood function, it is suitable for the RECH models as deriving the RECH models are highly flexible in terms of model specification. 

---

## References

[1] Nguyen, T.-N., Tran, M.-N., and Kohn, R. (2020). Recurrent conditional heteroskedasticity. arXiv:2010.13061. [Read the paper](https://arxiv.org/abs/2010.13061)

[2] Tran, M.-N., Nguyen, T.-N., Nott, D., and Kohn, R. (2020). Bayesian deep net GLM and GLMM. *Journal of Computational and Graphical Statistics*, 29(1):97-113. [Read the paper](https://www.tandfonline.com/doi/abs/10.1080/10618600.2019.1637747)

