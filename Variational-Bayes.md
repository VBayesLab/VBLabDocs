---
layout: default
title: VB API Reference 
description: "Gaussian Variational Bayes methods supported by VBLab"
nav_order: 4
has_children: true
permalink: /gvb/
---
# **VB classes**

[Tutorial](#tutorial){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [VBLab on GitHub](https://github.com/VBayesLab/Tutorial-on-VB){: .btn .fs-5 .mb-4 .mb-md-0 }

---
VBLab provides several classes to implement VB algorithms discussed in the tutorial paper. 
- [CGVB]({{site.baseurl}}{% link VB-CGVB.md %}): Gaussian Variational Bayes with Cholesky decomposed covariance method
- [VAFC]({{site.baseurl}}{% link VB-VAFC.md %}): Gaussian Variational Bayes with factor decomposed covariance method
- [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}): Natural gradient Gaussian Variational Approximation with factor Covariance method 
- [MGVB]({{site.baseurl}}{% link VB-MGVB.md %}): Manifold Gaussian Variational Bayes method