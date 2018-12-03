---
title: "Assign4"
author: "Yue Wang"
date: '2018-12-02'
output:
  html_document:
    keep_md: yes
    number_sections: yes
    toc: yes
---



#2

```r
diploids_selection = function(p0=0.5,W_AA=0.25,W_Aa=0.5,W_aa=0.25,n=100){
  p=rep(NA,n)
  w_bar=rep(NA,n)
  p[1]=p0
  w_bar[1]=p[1]^2*W_AA+2*p[1]*(1-p[1])*W_Aa+(1-p[1])^2*W_aa
  for(i in 2:n){
    p[i]=p[i-1]^2*W_AA/w_bar[i-1]+p[i-1]*(1-p[i-1])*W_Aa/w_bar[i-1]
    w_bar[i]=p[i]^2*W_AA+2*p[i]*(1-p[i])*W_Aa+(1-p[i])^2*W_aa
  }
  
  return(p)
}
p<-diploids_selection(p0=0.001,W_AA=0.4,W_Aa=0.3,W_aa=0.2,n=40)
generations <- 1 :length(p)
plot(p~generations,pch=20,
     ylab = "allele frequency",
     xlab = "generation")
```

![](Assign4_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

#3

```r
gene_drift=function(p_a=0.5,p_A=0.5,size=40,n=10,type=1){#type=1:"haploid"
  freq_a=rep(NA,n)
  freq_A=rep(NA,n)
  freq=rep(NA,2)
  freq_a[1]=p_a
  freq_A[1]=p_A
  for(i in 2:n){
    if(freq_a[i-1]==1){
      freq_A[(i-1):n]=rep(0,(n-i+2))
      freq_a[i:n]=rep(1,(n-i+1))
      break 
    }
    if(freq_A[i-1]==1){
      freq_A[i:n]=rep(1,(n-i+1))
      freq_a[(i-1):n]=rep(0,(n-i+2))
      break 
    }
      freq = as.vector(table(sample(c("a","A"),size*type,replace = T,prob=c(freq_a[i-1],freq_A[i-1])))/(size*type))
      print(freq)
      freq_a[i] = freq[1]
      freq_A[i] = freq[2]
  }
  if(any(freq_a>0.9999)){
    fixation = min(which.max(freq_a>0.9999))
    cat("fixation for a occurs approximately at generation:", fixation)
  }else {
    maxAlleleFreq <- max(freq_a)
    cat("fixation for a does not occur, max. allele frequency is", print(maxAlleleFreq,digits =2))
  }
  if(any(freq_A>0.9999)){
    fixation = min(which.max(freq_A>0.9999))
    cat("fixation for A occurs approximately at generation:", fixation)
  }else {
    maxAlleleFreq <- max(freq_A)
    cat("fixation for A does not occur, max. allele frequency is", print(maxAlleleFreq,digits =2))
  }
  generations <- 1 :length(freq_a)
  plot(freq_a~generations,pch=20,type="o",col="red",ylim = c(min(min(freq_A,freq_a)),max(freq_A,freq_a)),
       ylab = "allele frequency",
       xlab = "generation")
  lines(freq_A~generations,pch=17,type="o",lty=2,col="blue")
} 
gene_drift(p_a=0.5,p_A=0.5,size=40,n=100,type=2)
```

```
## [1] 0.5375 0.4625
## [1] 0.45 0.55
## [1] 0.475 0.525
## [1] 0.4875 0.5125
## [1] 0.5 0.5
## [1] 0.475 0.525
## [1] 0.3375 0.6625
## [1] 0.35 0.65
## [1] 0.35 0.65
## [1] 0.4 0.6
## [1] 0.4125 0.5875
## [1] 0.3375 0.6625
## [1] 0.4125 0.5875
## [1] 0.425 0.575
## [1] 0.4125 0.5875
## [1] 0.5 0.5
## [1] 0.5125 0.4875
## [1] 0.4875 0.5125
## [1] 0.4875 0.5125
## [1] 0.5625 0.4375
## [1] 0.525 0.475
## [1] 0.625 0.375
## [1] 0.65 0.35
## [1] 0.6375 0.3625
## [1] 0.575 0.425
## [1] 0.4875 0.5125
## [1] 0.4875 0.5125
## [1] 0.4 0.6
## [1] 0.4125 0.5875
## [1] 0.525 0.475
## [1] 0.575 0.425
## [1] 0.65 0.35
## [1] 0.5375 0.4625
## [1] 0.5875 0.4125
## [1] 0.6125 0.3875
## [1] 0.625 0.375
## [1] 0.7125 0.2875
## [1] 0.6875 0.3125
## [1] 0.6125 0.3875
## [1] 0.5125 0.4875
## [1] 0.65 0.35
## [1] 0.625 0.375
## [1] 0.625 0.375
## [1] 0.625 0.375
## [1] 0.6125 0.3875
## [1] 0.6 0.4
## [1] 0.6375 0.3625
## [1] 0.625 0.375
## [1] 0.6 0.4
## [1] 0.5875 0.4125
## [1] 0.575 0.425
## [1] 0.5875 0.4125
## [1] 0.5375 0.4625
## [1] 0.5125 0.4875
## [1] 0.4625 0.5375
## [1] 0.45 0.55
## [1] 0.5125 0.4875
## [1] 0.6 0.4
## [1] 0.6375 0.3625
## [1] 0.65 0.35
## [1] 0.6625 0.3375
## [1] 0.6375 0.3625
## [1] 0.6125 0.3875
## [1] 0.5375 0.4625
## [1] 0.575 0.425
## [1] 0.6625 0.3375
## [1] 0.6375 0.3625
## [1] 0.5625 0.4375
## [1] 0.5 0.5
## [1] 0.45 0.55
## [1] 0.4125 0.5875
## [1] 0.4 0.6
## [1] 0.3875 0.6125
## [1] 0.475 0.525
## [1] 0.4625 0.5375
## [1] 0.4875 0.5125
## [1] 0.45 0.55
## [1] 0.3625 0.6375
## [1] 0.325 0.675
## [1] 0.3375 0.6625
## [1] 0.3375 0.6625
## [1] 0.375 0.625
## [1] 0.4125 0.5875
## [1] 0.425 0.575
## [1] 0.45 0.55
## [1] 0.45 0.55
## [1] 0.5125 0.4875
## [1] 0.575 0.425
## [1] 0.5375 0.4625
## [1] 0.5625 0.4375
## [1] 0.55 0.45
## [1] 0.6 0.4
## [1] 0.575 0.425
## [1] 0.5875 0.4125
## [1] 0.6375 0.3625
## [1] 0.65 0.35
## [1] 0.7 0.3
## [1] 0.7125 0.2875
## [1] 0.75 0.25
## [1] 0.75
## fixation for a does not occur, max. allele frequency is 0.75[1] 0.68
## fixation for A does not occur, max. allele frequency is 0.675
```

![](Assign4_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

#4

```r
gene_drift_freq=function(p_a=1-p_A,p_A=0.1,size=50,n=100,type=1,permutation=1000){#type=1:"haploid"
pro = 0
  for(m in 1:permutation) {
  freq_a=rep(NA,n)
  freq_A=rep(NA,n)
  freq_a[1]=p_a
  freq_A[1]=p_A
  for(i in 2:n){
    if(freq_a[i-1]==1){
      freq_A[(i-1):n]=rep(0,(n-i+2))
      pro = pro+1
      freq_a[i:n]=rep(1,(n-i+1))
      break 
    }
    if(freq_A[i-1]==1){
      freq_A[i:n]=rep(1,(n-i+1))
      freq_a[(i-1):n]=rep(0,(n-i+2))
      break 
    }
      freq = as.vector(table(sample(c("a","A"),size*type,replace = T, prob=c(freq_a[i-1],freq_A[i-1])))/(size*type))
      freq_a[i] = freq[1]
      freq_A[i] = freq[2]
  }
} 
return(pro/permutation)
}
gene_drift_freq(p_A=0.5,size =200, type=2)
```

```
## [1] 0.012
```

```r
gene_drift_freq(p_A=0.25,size =200, type=2)
```

```
## [1] 0.102
```

```r
gene_drift_freq(p_A=0.1,size =200, type=2)
```

```
## [1] 0.433
```

#5

```r
gene_drift_allele_trajectories=function(p_a=0.5,p_A=0.5,size=400,n=100,type=1,permutation = 100){#type=1:"haploid"
  freq_a=rep(NA,n)
  freq_A=rep(NA,n)
  freq_a[1]=p_a
  freq_A[1]=p_A
  generations <- 1 :length(freq_A)
  plot(freq_A~generations,pch=20,type="n",col="red",ylim = c(0,1),
       ylab = "allele frequency",
       xlab = "generation")
  
  for(m in 1:permutation){
  for(i in 2:n){
    if(freq_a[i-1]==1){
      freq_A[(i-1):n]=rep(0,(n-i+2))
      freq_a[i:n]=rep(1,(n-i+1))
      break 
    }
    if(freq_A[i-1]==1){
      freq_A[i:n]=rep(1,(n-i+1))
      freq_a[(i-1):n]=rep(0,(n-i+2))
      break 
    }
      freq = as.vector(table(sample(c("a","A"),size*type,replace = T, prob=c(freq_a[i-1],freq_A[i-1])))/(size*type))
      freq_a[i] = freq[1]
      freq_A[i] = freq[2]
  
  }
  #generations <- 1 :length(freq_A)
  lines(freq_A~generations,lty=1,col=rainbow(permutation)[m])
} 
}
gene_drift_allele_trajectories()
```

![](Assign4_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

#6.1
##using set.seed(1234) to generate same random numbers every time

```r
get_p_value = function(slope=0.1,intercept = 0.5,size = 20, e = 2){
x <- seq(from =1, to = 10, length.out = size) # length.out is how many observations we will have a <- 0.5 
y_deterministic <- intercept+ slope*x
y_simulated <- rnorm(length(x), mean = y_deterministic, sd = e)
mod_sim <- lm(y_simulated ~ x)
p_val_slope <- summary(mod_sim)$coef[2,4] # extracts the p-value p_val_slope
summary(mod_sim)
return (p_val_slope)
}
get_p_value()
```

```
## [1] 0.3757218
```

#6.2

```r
permutation = 1000
p_array=rep(NA,permutation)
p_array=replicate(permutation,get_p_value())
hist(p_array,freq = F)
```

![](Assign4_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
p_pvalue_0.05 = as.vector(table(p_array<0.05)/permutation)[2]
p_pvalue_0.05
```

```
## [1] 0.082
```

#6.3

```r
permutation = 1000
p_array=rep(NA,permutation)
p_array=replicate(permutation,get_p_value(slope=0))
hist(p_array,freq = F)
```

![](Assign4_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
p_pvalue_0.05 = as.vector(table(p_array<0.05)/permutation)[2]
p_pvalue_0.05
```

```
## [1] 0.053
```

#6.4

```r
size_array = seq(10,100,by=5)
permutation =100
for(i in size_array){
  p_array=replicate(permutation,get_p_value(slope=0.1,intercept = 0.5,e=1.5))
  #hist(p_array,freq = F)
  p_pvalue_0.05[i] = as.vector(table(p_array<0.05)/permutation)[2]
}
plot(p_pvalue_0.05[size_array]~size_array)
abline(lm(p_pvalue_0.05[size_array]~size_array))
```

![](Assign4_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

