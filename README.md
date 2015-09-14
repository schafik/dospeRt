---
title: "dospert"
author: "Geun Hae Ahn"
date: "September 4, 2015"
output: 
  html_document: 
    theme: cosmo
---

dospert is a pacakge used to aid the analysis of DOSPERT data. It includes three functions:

- **d_clean**: d_clean function returns raw DOSPERT data in a panel format. 
- **d_sum**: d_sum function returns sum of DOSPERT responses by domain and scales.
- **d_score**: d_score returns DOSPERT scores attached to the original input dataframe.


# DOSPERT domains and scales
DOSPERT has five risk domains and three risk types. These are taken as arguments for d_sum, and are used for variable names. 

- Risk domains are abbreviated as follows:
    + financial: *fin*
    + health/safety: *hea* or *saf*
    + recreational: *rec*
    + ethical: *eth*
    + social: *soc*
    
- Risk types are:
    + risk taking: *RT*
    + risk benefit: *RB*
    + risk perception: *RP*
    

# Data
Data for analysis is raw data downloaded from qualtrics. File formats supported are .csv and .xml, with .csv as default. The main difference is that when downloaded, .csv files include first row of column names/questions but .xml files do not. There is no need to manipulate the variable names before using any of the functions. However, for the responses of the dospert questions, variable names should be in "(domain)(risk type)_Question number" (e. g. finRT_1) format. 



```
## Error in setwd("/git_repositories/betterment_project/data/raw_data"): cannot change working directory
```



```r
dcsv <- read.csv("pilot_data.csv", header = TRUE) # [d]ospert in [csv]
```

```
## Warning in file(file, "rt"): cannot open file 'pilot_data.csv': No such
## file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

```r
head(dcsv[, 1:10])
```

```
## Error in head(dcsv[, 1:10]): object 'dcsv' not found
```


```r
dxml <- xmlToDataFrame("Betterment_Pilot_8_7.xml", stringsAsFactors = F) %>% filter(uid != "")  
```

```
## Error in xmlToDataFrame(xmlParse(doc), colClasses, homogeneous, collectNames, : error in evaluating the argument 'doc' in selecting a method for function 'xmlToDataFrame': Error: XML content does not seem to be XML: 'Betterment_Pilot_8_7.xml'
```

```r
# [d]ospert in [xml]
# unique identifying variable for this dataset is 'uid'
# removed observations without uid variable value
head(dxml[, 1:10])
```

```
## Error in head(dxml[, 1:10]): object 'dxml' not found
```

# **1) d_clean**

d_clean function takes three arguments: raw dataframe, unique identifying variable and file type and returns a panel format dataframe, which can be used, for example, for data inspection purposes.



In the sample .csv format dataframe, first column uniquely identifies the respondents. 


```r
csvdclean <- d_clean(dcsv, "V1", file_type = "csv")
```

```
## Error in is.data.frame(data): object 'dcsv' not found
```

```r
head(csvdclean)
```

```
## Error in head(csvdclean): object 'csvdclean' not found
```

For the .xml format dataframe, the usage is similar: unique identifying variable in this sample dataset is 'uid', and file type is xml.


```r
xmldclean <- d_clean(dxml, "uid", file_type = "xml")
```

```
## Error in is.data.frame(data): object 'dxml' not found
```

```r
head(xmldclean)
```

```
## Error in head(xmldclean): object 'xmldclean' not found
```


# **2) d_sum**

d_sum takes five arguments: raw dataframe, unique identifying variable, risk domain, risk type (or scale) and file type, and returns the sum of the respondents DOSPERT response of the designated risk domain and type by unique id.


```r
csvdsum <- d_sum(dcsv, "V1", "fin", "RT", file_type = "csv")
```

```
## Error in eval(expr, envir, enclos): object 'dcsv' not found
```

```r
head(csvdsum)
```

```
## Error in head(csvdsum): object 'csvdsum' not found
```


```r
xmldsum <- d_sum(dxml, "uid", "fin", "RB", file_type = "xml")
```

```
## Error in eval(expr, envir, enclos): object 'dxml' not found
```

```r
head(xmldsum)
```

```
## Error in head(xmldsum): object 'xmldsum' not found
```

# **3) d_score**

d_score takes same three arguments as d_clean, calculates the risk attitude coefficients and returns the results attached to the last columns of input dataframe. 

The risk attitude coefficients are named "(domain)_int'', "(domain)_RB'', "(domain)_RP''


```r
csvdscore <- d_score(dcsv, "V1", file_type = "csv")
```

```
## Error in eval(expr, envir, enclos): object 'dcsv' not found
```

```r
head(csvdscore %>% dplyr::select(unique_id, fin_int, fin_RB, fin_RP))
```

```
## Error in eval(expr, envir, enclos): object 'csvdscore' not found
```


```r
xmldscore <- d_score(dxml, "uid", file_type = "xml")
```

```
## Error in eval(expr, envir, enclos): object 'dxml' not found
```

```r
head(xmldscore %>% dplyr::select(unique_id, fin_int, fin_RB, fin_RP))
```

```
## Error in eval(expr, envir, enclos): object 'xmldscore' not found
```




