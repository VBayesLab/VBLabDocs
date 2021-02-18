---
layout: default
title: readData
parent: Utilities Reference
nav_order: 1
permalink: /utilities/readdata
---

# <samp>readData</samp> [Github code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/Utilities/readData.m){: .fs-4 .btn .btn-purple  .float-right}
{: .fs-8 }

Load built-in datasets
{: .fs-6 .fw-300 }

---

## Syntax

```matlab
data = readData(DataName,Name,Value)
```
---
## Description
`data = readData(DataName,Name,Value)` returns one of the built-in datasets in selected format. `Name` and `Value` specifies additional options using one or more name-value pair arguments. For example, users can specify if the data is stored in a 2D array or a table.

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
{: #input-arguments}
<!--DataName-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">DataName</span> - Name of the dataset</header>
#### Data type: string
<br>
Name of the built-in datasets of the VBLab package. See [list of VBLab built-in datasets]({{site.baseurl}}{%link Built-in-Datasets.md%}). 

|Data Name| Description | Task | Argument |
|:--------|:----------|:---------|
|[`'Abalon'`](/VBLabDocs/datasets/#abalon)|  Cross-sectional data | Regression|`'Intercept'`, `'Normalized'`,`'Type'`|
|[`'Cencus'`](/VBLabDocs/datasets/#cencus-data)| Cross-sectional data | Classification |`'Intercept'`, `'Normalized'`,`'Type'`|
|[`'DirectMarketing'`](/VBLabDocs/datasets/#direct-marketing)| Cross-sectional data | Regression |`'Intercept'`, `'Normalized'`,`'Type'`|
|[`'GermanCredit'`](/VBLabDocs/datasets/#german-credit)| Cross-sectional data | Classification |`'Intercept'`, `'Normalized'`,`'Type'`|
|[`'LabourForce'`](/VBLabDocs/datasets/#labour-force)| Cross-sectional data | Classification |`'Intercept'`, `'Normalized'`,`'Type'`|
|[`'RealizedLibrary'`](/VBLabDocs/datasets/#realized-library)| Time-series data | Regression |`'Index'`,`'Length'`, `'RealizedMeasure'`,`'Type'`|

</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<!--Name-Value Pairs-->
<span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Name-Value Pair Arguments </span>

Specify optional comma-separated pairs of `Name,Value` arguments. `Name` is the argument name and `Value` is the corresponding value. `Name` must appear inside quotes. You can specify several name and value pair arguments in any order as `Name1,Value1,...,NameN,ValueN`.

**Example:** `'Type','Matrix','Intercept',true` specifies that the output dataset is stored in a 2D array and a column of 1 will be added to the data matrix as intercepts. 

<!--Intercept-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Intercept'</span> - Adding intercept column</h3></header>
#### Data Type: true | false
<br>
Flag to add a column of $1$ to the data as intercepts. 

**Note:** Only available for cross-sectional (tabular) data

**Default:** `false`

**Example:** `'Intercept',true`
</div>

<!--Type-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Type'</span> - Data format</h3></header>

#### Data Type: string
<br>
Format of the output data. Can be specified as

- `'Matrix'` Data is stored in a 2D array, for cross-sectional data or multivariate time series data, or 1D array, for univaritate time series. For cross-sectional, the last data column contains the response values.  
- `'Table'` Data is stored in a table. For cross-sectional, the last data column contains the response values. 

**Default:** `Matrix`

**Example:** `'Type',Table`
</div>

<!--Normalized-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Normalized'</span> - Normalization flag</h3></header>

#### Data Type: true | false
<br>
Flag to normalize numerical variables. Neural network based models such as [DeepGLM]({{site.baseurl}}{%link Model-DeepGLM.md%}) work more efficient with normalized data.  

**Default:** `false`

**Example:** `'Normalized',true`
</div>

<!--Index-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Index'</span> - Normalization flag</h3></header>

#### Data Type: string | cell array of strings
<br>
Flag to normalize numerical variables. Neural network based models such as [DeepGLM]({{site.baseurl}}{%link Model-DeepGLM.md%}) work more efficient with normalized data.  

**Default:** `None`

**Example:** `'Index','SP500'`
</div>

<!--Length-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Length'</span> - Normalization flag</h3></header>

#### Data Type: Integer | Positive
<br>
Flag to normalize numerical variables. Neural network based models such as [DeepGLM]({{site.baseurl}}{%link Model-DeepGLM.md%}) work more efficient with normalized data.  

**Default:** `None`

**Example:** `'Length',1000`
</div>

<!--RealizedMeasure-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'RealizedMeasure'</span> - Normalization flag</h3></header>

#### Data Type: string | cell array of strings
<br>
Flag to normalize numerical variables. Neural network based models such as [DeepGLM]({{site.baseurl}}{%link Model-DeepGLM.md%}) work more efficient with normalized data.  

**Default:** `None`

**Example:** `'RealizedMeasure','rk_parzen'`
</div>

</div>

---

## Output Arguments
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">data </span> - Output dataset </header>
{: #output-dataset}
#### Data type: array | table
<br>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

--- 

## Examples
<!--Continuous response-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">

## <span style="font-weight:bold;font-size:20px">Fit a LogisticRegression model for binary response</span> [Github code](https://github.com/VBayesLab/Tutorial-on-VB){: .fs-4 .btn .btn-purple  .float-right}
{: #logistic-binary}
Fit a LogisticRegression model to [LabourForce](/VBLabDocs/datasets/#labour-force) data using [CGVB]({% link VB-CGVB.md %})

Load the LabourForce data using the [<span style="font-family:monospace">readdata()</span>]({% link Utilities-Read-Data.md%}) function. 
The data is a matrix with the last column is the response variable. Set the `'Intercept'` argument to be `true` to add a column of 1 to the data matrix as intercepts.  

```matlab
% Load the LabourForce dataset
labour = readdata('LabourForce',...
                  'Type','Matrix',...
                  'Intercept',true);
```

</div>

---

