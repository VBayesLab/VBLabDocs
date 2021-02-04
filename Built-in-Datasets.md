---
layout: default
title: Built-in Datasets
description: "Description of built-in datasets "
nav_order: 7
permalink: /datasets/
---

# **Built-in datasets**
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

Given is the attribute name, attribute type, the measurement unit and a brief description. The dataset has $4177$ rows and $9$ columns.

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

### Information
{: .no_toc }

This data set contains weighted census data extracted from the 1994 and 1995 Current Population Surveys conducted by the U.S. Census Bureau. The data contains 41 demographic and employment related variables.

The instance weight indicates the number of people in the population that each record represents due to stratified sampling. To do real analysis and derive conclusions, this field must be used. This attribute should *not* be used in the classifiers.

One instance per line with comma delimited fields. There are 199523 instances in the data file and 99762 in the test file.

The data was split into train/test in approximately 2/3, 1/3 proportions.

For more information or to download the dataset, please visit the [dataset website](https://archive.ics.uci.edu/ml/datasets/Census-Income+(KDD)). 

### Attribute Information
{: .no_toc }

|Name           | Data Type   | Values | Description|
|:--------------|:------------|:-------|:-----------|
|age            | continuous  | 1-4    | Age |
|workclass      | categorical |        | Class of worker |
|fnlwgt         | categorical | 0-4    | Industry code |
|education      | categorical | 0-10   | Occupation code |
|marital-status | categorical | 0-5    | Adjusted gross income |
|occupation     | ordinal     | 1-5    | Education  |
|relationship   | ordinal     | in %   | Wage per hour |
|race           | categorical | 1-5    | Enrolled in edu inst last wk |
|sex            | categorical | 1-3    | Is there another debtor or a guarantor for the credit? |
|capital-gain   | ordinal     | 1-4    | Length of time (in years) the debtor lives in the present residence |
|capital-loss   | ordinal     | 1-4    | The debtor's most valuable property, i.e. the highest possible code is used |
|hours-per-week | integer     |        | age in years  |
|native-country | categorical | 1-3    | Installment plans from providers other than the credit-giving bank |

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
|Salary | continuous | in USD | Annual salary of customer |
|Children | integer | 0–3 | Number of childrens |
|History | categorical | low/medium/high/NA |  History of previous purchase volume. NA means that this customer has not yet purchased |
|Catalogs| continuous | in USD |  Number of catalogs sent  |
|AmountSpent | continuous | in USD |  Amount spent of customer  |

---

## GermanCredit data
{: #german-credit}

### Information
{: .no_toc }

This dataset classifies people described by a set of attributes as good or bad credit risks.

For more information or to download the dataset, please visit the [dataset website](https://archive.ics.uci.edu/ml/datasets/South+German+Credit+%28UPDATE%29). 

### Attribute Information
{: .no_toc }

Given is the attribute name, attribute type, values and a brief description. 

|Name                    | Data Type   | Values | Description|
|:-----------------------|:------------|:-------|:-----------|
|status                  | ordinal     | 1-4    | Status of the debtor's checking account with the bank |
|duration                | integer     |        | Credit duration in months |
|credit_history          | categorical | 0-4    | History of compliance with previous or concurrent credit contracts |
|purpose                 | categorical | 0-10   | Purpose for which the credit is needed |
|savings                 | categorical | 0-5    | Debtor's savings |
|employment_duration     | ordinal     | 1-5    | Present employment since |
|installment_rate        | ordinal     | in %   | Credit installments as a percentage of debtor's disposable income |
|personal_status_sex     | categorical | 1-5    | Combined information on sex and marital status |
|other_debtors           | categorical | 1-3    | Is there another debtor or a guarantor for the credit? |
|present_residence       | ordinal     | 1-4    | Length of time (in years) the debtor lives in the present residence |
|Property                | ordinal     | 1-4    |  The debtor's most valuable property, i.e. the highest possible code is used |
|age                     | integer     |        | age in years  |
|other_installment_plans | categorical | 1-3    | Installment plans from providers other than the credit-giving bank |
|housing                 | categorical | 1-3    | Type of housing the debtor lives in |
|number_credits          | integer     |        | Number of credits including the current one the debtor has (or had) at this bank |
|job                     | ordinal     | 1-4    | Quality of debtor's job |
|people_liable           | integer     |        | Number of persons who financially depend on the debtor (i.e., are entitled to maintenance)|
|telephone               | binary      | yes/no | Is there a telephone landline registered on the debtor's name|
|foreign_worker          | binary      | yes/no | Is the debtor a foreign worker?|
|credit_risk             | binary      | yes/no | Has the credit contract been complied with (good) or not (bad)?|

### Citation
{: .no_toc }

Grömping, U. (2019). South German Credit Data: Correcting a Widely Used Data Set. Report 4/2019, Reports in Mathematics, Physics and Chemistry, Department II, Beuth University of Applied Sciences Berlin.

**Or bibtex entry** :

```yaml
@Techreport{Grömping:2019,
title = {South German Credit Data: Correcting a Widely Used Data Set.},
author = {Grömping, U.},
year = {2019},
institution = {"Department II, Beuth University of Applied Sciences Berlin.}}
```

---

## LabourForce data
{: #labour-force}

---

## RealizedLibrary
{: #realized-library}

### Information
{: .no_toc }

The Oxford-Man Institute's "realised library" contains daily non-parametric measures of how volatility financial assets or indexes were in the past.

### Available Assets
{: .no_toc }

|Symbol	| Name	| Earliest Available | Latest Available




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

