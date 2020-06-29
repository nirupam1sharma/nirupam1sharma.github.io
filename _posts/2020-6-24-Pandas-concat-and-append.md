---
layout: post
title: Pandas Concat and Append
published: true
tags: Python
---
# Python 102: Pandas Concat and Append   
In this blog post, 2 important **pandas** functions **pd.concat** and **pd.append** will be discussed with the help of examples.  
## Why Concat and Append
Data Science projects typically involve working with multiple tables and analysis may often require us to combine these multiple files together. This file join process may require simple horizontal or vertical concatenation or may require complex SQL type joins. 
>For example: appending new rows to a table, pulling additional columns in, or in more complex cases, merging distinct tables on a common key  

This blog post will only cover Concat and Append operation and for advanced Merge and Join operations you refer to separate post that I'll write soon.  
## Import important libraries
The first task is to import the important libraries which will be required. In our case, we require a single library **pandas** which we will import.  

`import pandas as pd`

## Create a synthetic dataset
To make it easier for people to understand and replicate this on their system, we create synthetic data using the following code.  

#### Create series objects

**`my_series1 = pd.Series(['Apple', 'Banana', 'Chocolate', 'Donuts', 'Eclairs'], index=[1, 2, 3, 4, 5])`**  

**`my_series2 = pd.Series(['Fan', 'Gatorade', 'Hersheys', 'Ice cream', 'Jam'], index=[6, 7, 8, 9, 10])`**  


`print(my_series1)`


    1        Apple
    2       Banana
    3    Chocolate
    4       Donuts
    5      Eclairs
    dtype: object
    



`print(my_series2)`


    1           Fan
    2      Gatorade
    3      Hersheys
    4     Ice cream
    5          Jam
    dtype: object

### Create dataframe objects

`
df1 = pd.DataFrame([['Adam', 3.79], ['Betty', 3.4]],
                    columns=['Name', 'GPA'])`  

`df2 = pd.DataFrame([['Mac', 3.9], ['Nancy', 4]],
                  columns=['Name', 'GPA'])`  

`df3 = pd.DataFrame([['Computer Science','UCLA'],['Business Analytics','University of Cincinnati'],
                    ['Psychology','NJIT'],['Electronics','NYU']],
                  columns = ['Major','School'])`


Let us look at each of the data frames  

`df1`

		Name	GPA
	0	Adam	3.79
	1	Betty	3.40

`df2`

		Name	GPA
	0	Mac		3.9
	1	Nancy	4.0

`df3`

		Major				School
	0	Computer Science	UCLA
	1	Business Analytics	University of Cincinnati
	2	Psychology			NJIT
	3	Electronics			NYU

We have created all the necessary pandas series and data frames to explain the concepts in next section

## Concat function pd.concat()

The **Concat()** function helps in concatenating i.e. joining two different pandas objects on different axes.

_Syntax_  
	`pandas.concat(objs,axis,ignore_index)`

 - _objs_ : Series or Dataframe objects – This parameter takes the series or dataframe objects inside a list for performing concatenation operation.

 - _axis_ : {‘0′ for Index,’1’ for Columns} – By providing axis, we tell whether concatenation is to be performed over index or columns. The default is 0.

 - _ignore\_index_ : bool – This parameter either helps in removing the index or in adding index to dataframes. The default value is false.

You can have a look at the detailed description of all the parameters [here](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html)

>When you concatenate series, then a series object is returned. When multiple objects consisting of at least 1 dataframe are there, then resulting object is a dataframe

## Running pd.concat() on pandas series with different combination of parameters 
### 1. Default parameters

`
pd.concat([my_series1,my_series2])
`

	1         Apple
	2        Banana
	3     Chocolate
	4        Donuts
	5       Eclairs
	1           Fan
	2      Gatorade
	3      Hersheys
	4     Ice cream
	5          Jam
	dtype: object

We can see in the above output, by default concatenation happens over index i.e. vertically. The index of the series are not modified and hence we see repeating index. 

### 2. Using ignore_index parameter
To solve the issue of repeating index we set `ignore_index` as True as shown in the below example

`
pd.concat([my_series1,my_series2],ignore_index=True)
`


	0        Apple
	1       Banana
	2    Chocolate
	3       Donuts
	4      Eclairs
	5          Fan
	6     Gatorade
	7     Hersheys
	8    Ice cream
	9          Jam
	dtype: object

We can see that the index is reassigned starting with `0` 
 
### 3. Using axis parameter
We can combine the 2 series horizontally by setting the axis parameter as 1 as shown in below example  

`
pd.concat([my_series1,my_series2],axis=1)
`  

		0			1
	1	Apple		Fan
	2	Banana		Gatorade
	3	Chocolate	Hersheys
	4	Donuts		Ice cream
	5	Eclairs		Jam

## Running pd.concat() on pandas data frames with different combination of parameters 
### 1. Default parameters

`
pd.concat([df1,df2])
`

		Name	GPA
	0	Adam	3.79
	1	Betty	3.40
	0	Mac		3.90
	1	Nancy	4.00

We can see in the above output, by default concatenation happens over index i.e. vertically. The index of the data frames are not modified and hence we see repeating index. 

### 2. Using ignore_index parameter
To solve the issue of repeating index we set `ignore_index` as True as shown in the below example

`
pd.concat([df1,df2],ignore_index=True)
`

		Name	GPA
	0	Adam	3.79
	1	Betty	3.40
	2	Mac		3.90
	3	Nancy	4.00

We can see that the index is reassigned starting with `0` 
 
### 3. Using axis parameter
We can combine the 2 data frames horizontally by setting the axis parameter as 1 as shown in below example when we try to add the major and school information of the students  

`
comb_df = pd.concat([df1,df2],ignore_index=True)  
print(comb_df)
`  

		Name	GPA
	0	Adam	3.79
	1	Betty	3.40
	2	Mac		3.90
	3	Nancy	4.00

`
result_df = pd.concat([comb_df,df3],axis=1)  
print(result_df)
`  

    		Name   	GPA               	Major                    				School
	0   	Adam  	3.79    			Computer Science                      	UCLA
	1  		Betty  	3.40  				Business Analytics  					University of Cincinnati
	2    	Mac  	3.90          		Psychology                      		NJIT
	3  		Nancy  	4.00         		Electronics                       		NYU

## Append function DataFrame.append()
Series and DataFrame objects have an append method that can accomplish the same thing as concat does in fewer lines. For example, rather than calling `pd.concat([df1, df2])`, you can simply call `df1.append(df2)`. With the help of pandas `append()` function, we can append rows of one object to the rows of the calling object. With the help of `append()`, columns can also be appended

_Syntax_  
`pandas.DataFrame.append(other,ignore_index=False,sort=False)`

>Append rows of `other` to the end of caller, returning a new object.  
Columns in `other` that are not in the caller are added as new columns

 - other : DataFrame or Series/dict-like object, or list of these – This consists the other object which gets appended to main object.

 - ignore_index : bool – This parameter either helps in removing the index or in adding index to dataframes. The default value is false.

 - sort : bool – If required, we can sort the columns of self and other if they are not aligned.

This append() function returns a dataframe as an output

## Running DataFrame.append() on pandas data frames with different combination of parameters 
### 1. Default parameters

`
df1.append([df2])
`

		Name	GPA
	0	Adam	3.79
	1	Betty	3.40
	0	Mac		3.90
	1	Nancy	4.00

We see that values in df2 got appended to tail of df1 vertically. The index of the data frames are not modified and hence we see repeating index. 

### 2. Using ignore_index parameter
To solve the issue of repeating index we set `ignore_index` as True as shown in the below example

`
df1.append([df2],ignore_index=True)
`

		Name	GPA
	0	Adam	3.79
	1	Betty	3.40
	2	Mac		3.90
	3	Nancy	4.00

We can see that the index is reassigned starting with `0` 
