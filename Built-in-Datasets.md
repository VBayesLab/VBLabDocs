---
layout: default
title: Built-in Datasets
description: "Description of built-in datasets "
nav_order: 7
permalink: /datasets/
---

# Built-in datasets
{: .no_toc }

VBLab provides provide a few built-in datasets (already-cleaned, in `*.mat` format) that can be used for debugging a model or creating simple code examples.
<br>
## List of datasets
{: .no_toc .text-delta }

1. TOC
{:toc}

--- 

## Abalon data
{: #abalon}

### Information
{: .no_toc }

Predicting the age of abalone from physical measurements. The age of abalone is determined by cutting the shell through the cone, staining it, and counting the number of rings through a microscope -- a boring and time-consuming task. Other measurements, which are easier to obtain, are used to predict the age. Further information, such as weather patterns and location (hence food availability) may be required to solve the problem.

For more information or to download the dataset, please visit the [dataset website](https://archive.ics.uci.edu/ml/datasets/Abalone). 

### Attribute Information
{: .no_toc }

Given is the attribute name, attribute type, the measurement unit and a brief description.

|Name | Data Type | Measurement Unit | Description|
|:----|:-------|:-------|:-------|
|Sex | categorical | -- | M, F, and I (infant)|
|Length | continuous | mm | Longest shell measurement |
|Diameter | continuous | mm | perpendicular to length |
|Height | continuous | mm | with meat in shell |
|Whole weight | continuous | grams | whole abalone |
|Shucked weight | continuous | grams | weight of meat |
|Viscera weight | continuous | grams | gut weight (after bleeding) |
|Shell weight | continuous | grams | after being dried |
|Rings | integer | -- | +1.5 gives the age in years |

### Citation
{: .no_toc }

Dua, D. and Graff, C. (2019). UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]. Irvine, CA: University of California, School of Information and Computer Science.

**Or bibtex entry** :

```yaml
@misc{Dua:2019 ,
author = "Dua, Dheeru and Graff, Casey",
year = "2017",
title = "{UCI} Machine Learning Repository",
url = "http://archive.ics.uci.edu/ml",
institution = "University of California, Irvine, School of Information and Computer Sciences" }
```

---

## Cencus data

---

---

## DirectMarketing data
{: #direct-marketing}

### Information
{: .no_toc }

The data set includes data from a direct marketer who sells his products only via direct mail. He sends catalogs with product characteristics to customers who then order directly from the catalogs. The marketer has developed customer records to learn what makes some customers spend more than others.

The objective is to explain AmountSpent in terms of the provided customer characteristics.

For more information or to download the dataset, please visit the [dataset on Kaggle website](https://www.kaggle.com/yoghurtpatil/direct-marketing). 

### Attribute Information
{: .no_toc }

Given is the attribute name, attribute type, values and a brief description. 

|Name | Data Type | Values | Description|
|:----|:-------|:-------|:-------|
|Age | categorical | old/middle/young | Age of customer|
|Gender | binary |  male/female | Customer gender |
|OwnHome | binary | yes/no |  Whether customer owns home |
|Married | binary | single/married  | Whether customer is married or single |
|Location | binary | far/close | Location in terms of distance to the nearest brick and mortar store that sells similar products |
|Salary | continuous | USD | Annual salary of customer |
|Children | integer | 0–3 | Number of childrens |
|History | categorical | low/medium/high/NA |  History of previous purchase volume. NA means that this customer has not yet purchased |
|Catalogs| continuous | USD |  Number of catalogs sent  |
|AmountSpent | continuous | USD |  Amount spent of customer  |

---

## GermanCredit data
{: #german-credit}

### Information
{: .no_toc }

This dataset classifies people described by a set of attributes as good or bad credit risks.

For more information or to download the dataset, please visit the [dataset website](https://archive.ics.uci.edu/ml/datasets/statlog+(german+credit+data)). 

### Attribute Information
{: .no_toc }

Given is the attribute name, attribute type, values and a brief description. 

|Name | Data Type | Values | Description|
|:----|:-------|:-------|:-------|
| | ordinal | 1-4 | Status of existing checking account|
| | integer |  | Duration in month |
|OwnHome | binary | yes/no |  Credit history |
|Purpose | categorical | single/married  | Whether customer is married or single |
|Location | binary | far/close | Location in terms of distance to the nearest brick and mortar store that sells similar products |
|Salary | continuous | USD | Annual salary of customer |
|Children | integer | 0–3 | Number of childrens |
|History | categorical | low/medium/high/NA |  History of previous purchase volume. NA means that this customer has not yet purchased |
|Catalogs| continuous | USD |  Number of catalogs sent  |
|AmountSpent | continuous | USD |  Amount spent of customer  |

### Citation
{: .no_toc }

Dua, D. and Graff, C. (2019). UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]. Irvine, CA: University of California, School of Information and Computer Science.

**Or bibtex entry** :

```yaml
@misc{Dua:2019 ,
author = "Dua, Dheeru and Graff, Casey",
year = "2017",
title = "{UCI} Machine Learning Repository",
url = "http://archive.ics.uci.edu/ml",
institution = "University of California, Irvine, School of Information and Computer Sciences" }
```

---

## LabourForce data
{: #labour-force}

---

## RealizedLibrary
{: #realized-library}

### Citation
{: .no_toc }

Heber, Gerd, Asger Lunde, Neil Shephard and Kevin Sheppard (2009) "Oxford-Man Institute's realized library", Oxford-Man Institute, University of Oxford

**Or bibtex entry** :

```yaml
@misc{Heber:2009,
author = "Heber, Gerd, Asger Lunde, Neil Shephard and Kevin Sheppard",
year = "2009",
title = "Oxford-Man Institute’s realized library",
url = "https://realized.oxford-man.ox.ac.uk/",
institution = "Oxford-Man Institute, University of Oxford" }
```

---

