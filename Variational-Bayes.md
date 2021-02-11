---
layout: default
title: VB API Reference 
description: "Gaussian Variational Bayes methods supported by VBLab"
nav_order: 4
has_children: true
permalink: /gvb/
has_toc: false
---
# **VB classes**

---
VBLab provides several classes to implement VB algorithms discussed in the tutorial paper. 
- [CGVB]({{site.baseurl}}{% link VB-CGVB.md %}): Gaussian Variational Bayes with Cholesky decomposed covariance method
- [VAFC]({{site.baseurl}}{% link VB-VAFC.md %}): Gaussian Variational Bayes with factor decomposed covariance method
- [NAGVAC]({{site.baseurl}}{% link VB-NAGVAC.md %}): Natural gradient Gaussian Variational Approximation with factor Covariance method 
- [MGVB]({{site.baseurl}}{% link VB-MGVB.md %}): Manifold Gaussian Variational Bayes method