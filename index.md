---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: VBLab Home
nav_order: 1
description: "Official documentation of VBLab"
permalink: /
---

# **About VBLab**
{: .fs-8 }

VBLab is a probabilistic programming software package, currently available in Matlab, allowing automatic variational Bayesian (VB) inference on many pre-defined common statistical models and also user-defined models. 

Key features:
- Providing various Fixed From VB (FFVB) methods and works efficiently for high dimensional and complex posterior distributions. 
- Users are not required to know the technicality behind the VB techniques provided; all they need to do is to supply their statistical models, which can be specified flexibly in various ways.

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [VBLab on GitHub](https://github.com/VBayesLab/VBLab){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Getting started

### Install VBLab package

1. Download or clone the VBLab package on [VBLab Github Page](https://github.com/VBayesLab/VBLab)
2. Add the VBLab package, with all subfolders, to the Matlab search path. See [How to add or remove folders to Matlab search path](https://au.mathworks.com/help/matlab/matlab_env/add-remove-or-reorder-folders-on-the-search-path.html)

### How to start

1. Read the [VB tutorial paper](https://www.researchgate.net/publication/340006729_A_practical_tutorial_on_Variational_Bayes) for the theoretical explanation of the VB methods supported by the VBLab package. See also [shorter version of the VB tutorial]({{site.baseurl}}{% link Tutorial-Paper.md %}) on this site.
2. Run [examples](https://github.com/VBayesLab/VBLab/tree/main/Example) showing how to use various VB methods to fit different VBLab and user-defined models. See detail explanation of the examples in the the [VB tutorial paper](https://www.researchgate.net/publication/340006729_A_practical_tutorial_on_Variational_Bayes) or in the [Example]({{site.baseurl}}{% link Example.md%}) section on this site.
3. Check API reference for [supported VB techniques]({{site.baseurl}}{% link Variational-Bayes.md%}), [statistical models]({{site.baseurl}}{% link Model.md%}) and [how to define custom models]({{site.baseurl}}{% link Model-Custom.md%}) for users' applications.  

---

## Authors

- **Trong-Nghia Nguyen**, PhD candidate, The University of Sydney Business School. ([Google scholar](https://scholar.google.com.vn/citations?user=4fEGoI8AAAAJ&hl=en), [Research gate](https://www.researchgate.net/profile/Nghia_Nguyen79), [LinkedIn](https://www.linkedin.com/in/nguyen-nghia-458b3097/))
- **Minh-Ngoc Tran**, Associate Professor, The University of Sydney Business School. ([Google scholar](https://scholar.google.com/citations?user=98A6Dq8AAAAJ&hl=en), [Research gate](https://www.researchgate.net/profile/Minh-Ngoc-Tran), [Home Page](https://sites.google.com/site/mntran26/home))
- **Viet-Hung Dao**, PhD Candidate, The University of New South Wales Business School. ([Google scholar](https://scholar.google.com/citations?user=D7SJexgAAAAJ&hl=en&authuser=1&fbclid=IwAR2kh8zpH5Qx2ihhuG8jyBJueoe6N8Te0nttHNAim1v0orC_bnddIcmcXLw), [Research gate](https://www.researchgate.net/profile/Hung-Dao-3), [Home Page](https://acems.org.au/our-people/hung-dao))

--- 

## Citing VBLab

If you use VBLab in a scientific publication, we would appreciate citations to the following paper:

M.-N. Tran, T.-N. Nguyen and V.-H. Dao (2021). [A practical tutorial on Variational Bayes](https://arxiv.org/abs/2103.01327). *arXiv2103.01327*.

**Or bibtex entry** :
```yaml
@misc{tran:2021,
      title={A practical tutorial on Variational Bayes}, 
      author={Minh-Ngoc Tran and Trong-Nghia Nguyen and Viet-Hung Dao},
      year={2021},
      eprint={2103.01327},
      archivePrefix={arXiv},
      primaryClass={stat.CO}
}
```
