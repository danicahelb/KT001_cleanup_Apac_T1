# KT001_cleanup_Apac_T1

```r
rm(list=ls())
setwd("~/Desktop/Microarray Analyses/Kevin's array/151118 data/") #set working directory (folder where Apac array files are stored)
```

##########################################################################################
##Load array data:

```r
files <- sort(list.files("./Data/Processed Data")[grep("import_data.RData", list.files("./Data/Processed Data"))], decreasing=T)[1]
print(load(paste("./Data/Processed Data/", files, sep=""))) 
d1$spot[grep("std._", d1$spot)] <- gsub("std", "std0", d1$spot[grep("std._", d1$spot)])

temp.d1 <- d1[d1$study=="Apac_X1",]
```

##########################################################################################
##create matrix of raw MEAN intensity values

```r
temp <- matrix(NA, nrow=length(unique(d1$spot)), ncol=length(unique(temp.d1$sampleID)))
rownames(temp) <- sort(unique(temp.d1$spot))
colnames(temp) <- unique(temp.d1$sampleID)

for(id in colnames(temp)){
  for(ag in rownames(temp)){
    print(id)
    print(ag)
    temp[rownames(temp)==ag, colnames(temp)==id] <- temp.d1[temp.d1$spot==ag & temp.d1$sampleID==id, "Raw.Mean"]
  }
}
X1_raw_mean <- temp
```


##########################################################################################
##create matrix of raw Median intensity values

```r
temp <- matrix(NA, nrow=length(unique(d1$spot)), ncol=length(unique(temp.d1$sampleID)))
rownames(temp) <- sort(unique(temp.d1$spot))
colnames(temp) <- unique(temp.d1$sampleID)

for(id in colnames(temp)){
  for(ag in rownames(temp)){
    print(id)
    print(ag)
    temp[rownames(temp)==ag, colnames(temp)==id] <- temp.d1[temp.d1$spot==ag & temp.d1$sampleID==id, "Raw.Median"]
  }
}

X1_raw_median <- temp
```


##########################################################################################
##save data

```r
save(X1_raw_mean, X1_raw_median, file="./Data/Processed Data/0001.KTarray.Apac.cleaned_X1.RData")
```
