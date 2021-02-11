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
|`theta`  | &bull; Model parameters in each VB iteration. Denote as $\theta$ in VB algorithms. <br> &bull; For GVB, $\theta$ is generated from a multivariate normal distribution. Then if $\theta$ is restricted, e.g. prior distribution is not normal, then a transformation of $\theta$ to original scale, together with the jacobian, have to be provided. |
|`setting`| &bull; Additional settings of the custom models. <br> &bull; For example, prior distribution or some constant terms to define the models.|
|`h_func_grad`|  &bull; Gradient vector $\Delta_\theta h(\theta) = \Delta_\theta \text{log}p(y \mid \theta) + \Delta_\theta \text{log} p(\theta)$.<br> &bull; Must be a column vector $1 \times D$ with $D$ the number of model parameters.|
|`h_func`| &bull; $h(\theta) = \text{log} p(y \mid \theta) + \text{log} p(\theta)$. <br> &bull; Must be a scalar. |

**Note:** If the statistical models are defined as function handlers, users have to specify the number of model parameters explicitly using the `'NumParams'` argument of VB classes

### Example: Define a Logistic Regression model as a function handler.
{: #example-handler}



---

## Define custom models as Matlab Class Objects
{: #class-model}

This approach is more complicated 

### Example: Define a Linear Regression model as a Matlab class.
{: #example-class}

---