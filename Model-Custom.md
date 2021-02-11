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

## Define custom models as function handler
{: #custom-handler}



The input and outputs arguments have to be defined in the following order

```m
function [h_func_grad,h_func] = grad_h_func_logistic(data,theta,setting)
end
```

|:-----|:------|
|`data`   | &bull; The input data. |
|`theta`  | &bull; Model parameters in each VB iteration. <br> &bull; |
|`setting`| Additional settings of the custom models |
|`h_func_grad`|  &bull; Gradient vector $\Delta_\theta h(\theta) = \Delta_\theta \text{log}p(y \mid \theta) + \Delta_\theta \text{log} p(\theta)$ where $y$ the input data stored in `data` and $\theta$ the model parameters stored in `theta`.<br> &bull; Must be a column vector $1 \times D$ with $D$ the number of model parameters.|
|`h_func`| $h(\theta)$ and |

### Example: Define a Logistic Regression model as a function handler.
{: #example-handler}

---

## Define custom models as Matlab Class Objects
{: #class-model}

This approach is more complicated 

### Example: Define a Linear Regression model as a Matlab class.
{: #example-class}

---