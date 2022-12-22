# rk299792.github.io
---
title: "Homework 5"
author: "Radhakrishna Adhikari"
date: "2022-12-13"
output: pdf_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Problem 2

I implemented the SSVS algorithm below. I did not include any polynomial or interaction terms in the model. I chose 4000 as a burning period. After that, I counted the number of times each variable is selected. I chose the models with 9 variable that appeared highest number of times. 
```{r}
library(SSVS)
set.seed(24060)
model1=read.csv("~/Desktop/Bayesian Stat Materials/Model1_5444.txt",header = FALSE)

phi=2
V=function(X,var=32.433,psi=phi){
  X=data.matrix(X)
  V=solve(t(X)%*%X*(1/var)+diag(ncol(X))*(1/psi))
  return(V)
}

E <- function(X,Y1=y,psi=phi,var=32.433){
  X=data.matrix(X)
  V=V(X)
  E=V%*%t(X)%*%y/var
  return (E)
}



select_columns_matrix=function(indicator_array,columns1=columns){
  
newcol=c()
for (i in 1:50){
  if (indicator_array[i]==1){
    newcol=append(newcol,columns[i])
  }
  
}
return(model1[,newcol])
}


j_list=1:50

indicator_array=rep(1,50)
columns=names(model1)
columns=columns[2:length(columns)]
X_old=model1[,columns]
X_old$Intercept=rep(1,nrow(X_old))
y=model1[,"V1"]
indicator_matrix=rep(0,50)
for (i in 1:30000){
  j=sample(j_list,1,replace = TRUE)
  coin_filp=runif(1)
  
  if(coin_filp<=0.5){
    #We try to make the changes here
    E_old=E(X_old)
    V_old=V(X_old)
    
    indicator_array[j]=!indicator_array[j]
    X_new=select_columns_matrix(indicator_array)
    X_new$intercept=rep(1,nrow(X_new))
    
    E_new=E(X_new)
    V_new=V(X_new)
    
    num=(det(V_new)^(1/2))*(1/phi)^(ncol(X_new))*exp((1/2)*t(E_new)%*%solve(V_new)%*%E_new)
    denom=(det(V_old)^(1/2))*(1/phi)^(ncol(X_old))*exp((1/2)*t(E_old)%*%solve(V_old)%*%E_old)
    
    BF=num/denom
    
    p_star=(1+1/BF)^(-1)
    
    
    if(runif(1)<=p_star){
      indicator_array=indicator_array
      X_old=X_new
    }
    else {
      X_old=X_old
      indicator_array[j]=!indicator_array[j]
    }
  }
  if (i>=4000){
    indicator_matrix=rbind(indicator_matrix,indicator_array)
  }
  
    
  }

variable_counts=data.frame(cbind(columns,colSums(indicator_matrix)))
variable_counts <-variable_counts[order(variable_counts$V2,decreasing = TRUE),]

#variables that appeared highest number of times. 
head(variable_counts,8)
```
Next, I fitted the model with the selected variables and got the following coefficients. 

```{r}
selected_variables=variable_counts$columns[1:10]
sum_cols=paste(selected_variables, collapse = "+")
sum_cols
model1fit=lm(V1~V18+V29+V24+V34+V36+V5+V49+V21+V6+V35,data = model1)
summary(model1fit)
```

I also compared the result with the result obtained from the R-package named SSVSS. THere are several common variables selected. 
```{r}
results <- ssvs(data = model1, x = columns, y = "V1", continuous = TRUE,progress = FALSE)
summary1=summary(results)
summary1 <-summary1[order(summary1$MIP,decreasing = TRUE),]
head(summary1,10)

```

## Problem 3

I repeated the experiment with new data set. I didn't add polynomial and interaction variables here either. 

```{r}
library(SSVS)
set.seed(24060)
model1=read.csv("~/Desktop/Bayesian Stat Materials/Model2_5444.txt",header = FALSE)
j_list=1:50

indicator_array=rep(1,50)
columns=names(model1)
columns=columns[2:length(columns)]
X_old=model1[,columns]
X_old$Intercept=rep(1,nrow(X_old))
y=model1[,"V1"]
indicator_matrix=rep(0,50)
for (i in 1:30000){
  j=sample(j_list,1,replace = TRUE)
  coin_filp=runif(1)
  
  if(coin_filp<=0.5){
    #We try to make the changes here
    E_old=E(X_old)
    V_old=V(X_old)
    
    indicator_array[j]=!indicator_array[j]
    X_new=select_columns_matrix(indicator_array)
    X_new$intercept=rep(1,nrow(X_new))
    
    E_new=E(X_new)
    V_new=V(X_new)
    
    num=(det(V_new)^(1/2))*(1/phi)^(ncol(X_new))*exp((1/2)*t(E_new)%*%solve(V_new)%*%E_new)
    denom=(det(V_old)^(1/2))*(1/phi)^(ncol(X_old))*exp((1/2)*t(E_old)%*%solve(V_old)%*%E_old)
    
    BF=num/denom
    
    p_star=(1+1/BF)^(-1)
    
    
    if(runif(1)<=p_star){
      indicator_array=indicator_array
      X_old=X_new
    }
    else {
      X_old=X_old
      indicator_array[j]=!indicator_array[j]
    }
  }
  if (i>=4000){
    indicator_matrix=rbind(indicator_matrix,indicator_array)
  }
  
    
  }

variable_counts=data.frame(cbind(columns,colSums(indicator_matrix)))
variable_counts <-variable_counts[order(variable_counts$V2,decreasing = TRUE),]

#variables that appeared highest number of times. 
head(variable_counts,8)

```

Similar to problem 2. I selected variables from the table above. I then fitted the model with those variables and got the coefficients again. 

```{r}
selected_variables=variable_counts$columns[1:10]
sum_cols=paste(selected_variables, collapse = "+")
sum_cols
model1fit=lm(V1~V36+V14+V35+V29+V44+V16+V12+V38+V9+V43,data = model1)
summary(model1fit)
```

I compared the model with the the model obtained from R package. 

```{r}
results <- ssvs(data = model1, x = columns, y = "V1", continuous = TRUE,progress = FALSE)
summary1=summary(results)
summary1 <-summary1[order(summary1$MIP,decreasing = TRUE),]
head(summary1,10)
```



## Problem 4

I repeated exact same experiment again with this dataset. 
```{r}
library(SSVS)
set.seed(24060)
model1=read.csv("~/Desktop/Bayesian Stat Materials/Model3_5444.txt",header = FALSE)
j_list=1:50

indicator_array=rep(1,50)
columns=names(model1)
columns=columns[2:length(columns)]
X_old=model1[,columns]
X_old$Intercept=rep(1,nrow(X_old))
y=model1[,"V1"]
indicator_matrix=rep(0,50)
for (i in 1:30000){
  j=sample(j_list,1,replace = TRUE)
  coin_filp=runif(1)
  
  if(coin_filp<=0.5){
    #We try to make the changes here
    E_old=E(X_old)
    V_old=V(X_old)
    
    indicator_array[j]=!indicator_array[j]
    X_new=select_columns_matrix(indicator_array)
    X_new$intercept=rep(1,nrow(X_new))
    
    E_new=E(X_new)
    V_new=V(X_new)
    
    num=(det(V_new)^(1/2))*(1/phi)^(ncol(X_new))*exp((1/2)*t(E_new)%*%solve(V_new)%*%E_new)
    denom=(det(V_old)^(1/2))*(1/phi)^(ncol(X_old))*exp((1/2)*t(E_old)%*%solve(V_old)%*%E_old)
    
    BF=num/denom
    
    p_star=(1+1/BF)^(-1)
    
    
    if(runif(1)<=p_star){
      indicator_array=indicator_array
      X_old=X_new
    }
    else {
      X_old=X_old
      indicator_array[j]=!indicator_array[j]
    }
  }
  if (i>=4000){
    indicator_matrix=rbind(indicator_matrix,indicator_array)
  }
  
    
  }

variable_counts=data.frame(cbind(columns,colSums(indicator_matrix)))
variable_counts <-variable_counts[order(variable_counts$V2,decreasing = TRUE),]

#variables that appeared highest number of times. 
head(variable_counts,8)

```



```{r}
selected_variables=variable_counts$columns[1:10]
sum_cols=paste(selected_variables, collapse = "+")
sum_cols
model1fit=lm(V1~V18+V21+V5+V36+V6+V16+V10+V39+V11+V34,data = model1)
summary(model1fit)
```


```{r}
results <- ssvs(data = model1, x = columns, y = "V1", continuous = TRUE,progress = FALSE)
summary1=summary(results)
summary1 <-summary1[order(summary1$MIP,decreasing = TRUE),]
head(summary1,10)
```
