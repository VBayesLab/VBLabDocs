---
layout: default
title: MGVB
parent: VB API Reference 
nav_order: 2
permalink: /gvb/mgvb
---

# **MGVB**
{: .fs-8 }

Run the Manifold Gaussian VB (MGVB) method .
{: .fs-6 .fw-300 }

[Tutorial](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [GitHub](https://github.com/VBayesLab/Tutorial-on-VB){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Syntax

```matlab
PostVB = MGVB(data,Name,Value)
```
---
## Description
`PostVB = MGVB(model,data,Name,Value)` returns the Bayesian approximation `PostVB` to given the `model` and `data`. `Name` and `Value` specifie additional options using one or more name-value pair arguments. For example, you can specify how many samples used to estimate the lower bound. 

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
### data

### model

### Name-Value Pair Arguments 

---

## Output Arguments
### PostVB

--- 

## Examples