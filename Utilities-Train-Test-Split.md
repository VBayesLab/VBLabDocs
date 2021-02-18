---
layout: default
title: trainTestSplit
parent: Utilities Reference
nav_order: 2
permalink: /utilities/traintestsplit
---

# <samp>trainTestSplit</samp> [Github code](https://github.com/VBayesLab/VBLab/blob/main/VBLab/Utilities/trainTestSplit.m){: .fs-4 .btn .btn-purple .float-right}

Split data to subsets for training and testing
{: .fs-6 .fw-300}

---

## Syntax

```matlab
[dataTrain, dataTest] = trainTestSplit(data,ratio,Name,Value)
```
---
## Description
`[dataTrain, dataTest] = trainTestSplit(data,ratio,Name,Value)` Split arrays or tables `data` into train and test subsets given the split ratio `ratio`. If `data` is a struct, then split data stored in struct fields with the same ratio. `Name` and `Value` specifies additional options using one or more name-value pair arguments. For example, users can specify if data is randomly splitted or not. 

See: [Input Arguments](#input-arguments), [Output Argument](#output-arguments), [Examples](#examples)

---

## Input Arguments
{: #input-arguments}
<!--data-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">data</span> - Data to be splitted</header>
#### Data type: array | table | struct
<br>
The data to be splitted. 

</div>

<!--ratio-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;color:Tomato">ratio</span> - Split ratio</header>
#### Data type: double | between 0 and 1
<br>
Name of the built-in datasets of the VBLab package. See [list of VBLab built-in datasets]({{site.baseurl}}{%link Built-in-Datasets.md%}). 

</div>

<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<!--Name-Value Pairs-->
<span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">Name-Value Pair Arguments </span>

Specify optional comma-separated pairs of `Name,Value` arguments. `Name` is the argument name and `Value` is the corresponding value. `Name` must appear inside quotes. You can specify several name and value pair arguments in any order as `Name1,Value1,...,NameN,ValueN`.

**Example:** `'Type','Matrix','Intercept',true` specifies that the output dataset is stored in a 2D array and a column of 1 will be added to the data matrix as intercepts. 

<!--Shuffle-->
<div class="code-example" markdown="1" style="background-color:{{page.block_color}};padding:20px;">
<header><h3><span style="color:#A020F0;font-weight:bold;font-family:monospace">'Shuffle'</span> - Adding intercept column</h3></header>
#### Data Type: true | false
<br>
Flag to shuffle the observations when split data. 

**Note:** Should be set to `false` for time series data.

**Default:** `true`

**Example:** `'Shuffle',false`
</div>

---

## Output Arguments

<!--dataTrain-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">dataTrain </span> - Output training data </header>
{: #output-dataTrain}
#### Data type: array | table | struct
<br>
The training data. 
</div>

<!--dataTest-->
<div class="code-example" markdown="1" style="background-color:White;padding:20px;">
<header style="font-weight:bold;font-size:20px"><span style="font-family:monospace;font-size:20px;font-weight:bold;color:Tomato">dataTest </span> - Output test data </header>
{: #output-dataTest}
#### Data type: array | table | struct
<br>
The test data.
</div>