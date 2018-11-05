---
title: "Assign2"
author: "Yue Wang"
date: '2018-11-02'
output: 
  html_document:
    keep_md: true
---

**Q2**

```r
Data = read.csv("/Users/yuewang/Desktop/eXpress_dm_counts.csv",header = T, row.names = 1,sep=',')
rna_counts = function(x, y = TRUE ){  #y==FALSE means using log2
  if (y == TRUE) {
    a <- mean(Data[,x])
  } else {
    Data = Data+0.000001
    a <- mean(log2(Data[,x]))
  }
  return(a)
}
rna_counts('F101_lg_female_hdhorn')
```

```
## [1] 1978.847
```
**Q3**


```r
sample <- colnames(Data)
elements <- c()
for(i in sample){
  elements <- c(elements, rna_counts(i))
}
names(elements)=sample
```


**Q4**

```r
system.time(sapply(sample,rna_counts))
```

```
##    user  system elapsed 
##   0.001   0.000   0.002
```

```r
system.time(for(i in sample){ elements <- c(elements, rna_counts(i))})
```

```
##    user  system elapsed 
##   0.003   0.000   0.003
```
**Q5**

```r
sapply(Data,mean)
```

```
##  F101_lg_female_hdhorn F101_lg_female_thxhorn   F101_lg_female_wings 
##               1978.847               1983.250               1583.904 
##  F105_lg_female_hdhorn F105_lg_female_thxhorn   F105_lg_female_wings 
##               2105.712               1433.749               1869.962 
##  F131_lg_female_hdhorn F131_lg_female_thxhorn   F131_lg_female_wings 
##               2117.847               2307.529               2272.692 
##   F135_sm_female_wings  F135_sm_female_hdhorn F135_sm_female_thxhorn 
##               1728.483               1452.913               1776.309 
##  F136_sm_female_hdhorn F136_sm_female_thxhorn   F136_sm_female_wings 
##               2065.780               1777.769               1988.882 
##  F196_sm_female_hdhorn F196_sm_female_thxhorn   F196_sm_female_wings 
##               1348.898               1025.301               3067.287 
##  F197_sm_female_hdhorn F197_sm_female_thxhorn   F197_sm_female_wings 
##               2639.152               2047.151               2081.889 
##  F218_lg_female_hdhorn F218_lg_female_thxhorn   F218_lg_female_wings 
##               2329.563               1950.561               2074.992 
## M120_sm_male_genitalia    M120_sm_male_hdhorn   M120_sm_male_thxhorn 
##               1832.780               2105.145               2101.163 
##     M120_sm_male_wings M125_lg_male_genitalia    M125_lg_male_hdhorn 
##               2536.920               2088.092               2372.259 
##     M125_lg_male_wings M160_lg_male_genitalia    M160_lg_male_hdhorn 
##               2559.085               1727.538               2111.337 
##   M160_lg_male_thxhorn     M160_lg_male_wings M171_sm_male_genitalia 
##               2087.583               2184.076               2035.093 
##    M171_sm_male_hdhorn   M171_sm_male_thxhorn     M171_sm_male_wings 
##               1598.190               1621.659               1825.344 
## M172_sm_male_genitalia    M172_sm_male_hdhorn   M172_sm_male_thxhorn 
##               2196.101               1713.119               1344.019 
##     M172_sm_male_wings M180_lg_male_genitalia    M180_lg_male_hdhorn 
##               2602.351               1922.634               2670.498 
##   M180_lg_male_thxhorn     M180_lg_male_wings M200_sm_male_genitalia 
##               2003.293               3216.476               2412.038 
##    M200_sm_male_hdhorn   M200_sm_male_thxhorn     M200_sm_male_wings 
##               2032.085               2820.495               2203.813 
## M257_lg_male_genitalia    M257_lg_male_hdhorn   M257_lg_male_thxhorn 
##               2170.258               2361.912               2749.767 
##     M257_lg_male_wings 
##               1325.684
```

**Q6**

```r
row_mean = function(x, y = TRUE ){  #y==FALSE means using log2
  if (y == TRUE) {
    a <- rowMeans(Data[x,])
  } else {
    Data = Data+0.000001
    a <- rowMeans(log2(Data[x,]))
  }
  return(a)
}
row_mean('FBpp0087248',TRUE)
```

```
## FBpp0087248 
##    23.45455
```

```r
row_mean_con=sapply(rownames(Data),row_mean)
```

**Q7**

```r
row_mean_male = function(x, y = TRUE,z ){  #y==FALSE means using log2
  if (y == TRUE) {
    a <- rowMeans(z[x,])
  } else {
    z = z+0.000001
    a <- rowMeans(log2(z[x,]))
  }
  return(a)
}
male_hdhorn_lg_data = Data[,grep("M.*lg_male_hdhorn",sample,value = T)]
male_hdhorn_sm_data = Data[,grep("M.*sm_male_hdhorn",sample,value = T)]
lg_mean <- sapply(rownames(Data), z=male_hdhorn_lg_data,row_mean_male)
sm_mean <- sapply(rownames(Data),z=male_hdhorn_sm_data, row_mean_male)
mean_differece = lg_mean-sm_mean
```
**Q8**

```r
mean = sapply(rownames(Data),row_mean)
log_mean = sapply(rownames(Data),y=FALSE,row_mean)
library(ggplot2)
ggplot(Data,aes(x=mean,y=mean_differece))+geom_point()
```

![](Assign3_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
ggplot(Data,aes(x=log_mean,y=mean_differece))+geom_point()
```

![](Assign3_files/figure-html/unnamed-chunk-7-2.png)<!-- -->



