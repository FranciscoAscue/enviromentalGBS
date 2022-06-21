```r
#install.packages("foreach")
#install.packages("RSpectra")
#install.packages("devtools")
#devtools::install_github("bcm-uga/lfmm")

library(lfmm)
library(RSpectra)
```

```r
snpbinary <- read.table("bbd.ped")
snpbinary <- snpbinary[,-1] #remove the firts column
snpmatrix <- t(snpbinary)
colnames(snpmatrix) <- snpmatrix[1,]
snp <- snpmatrix[-c(1,2,3,4,5),]
``` 

```r
snp <- as.data.frame(snp)
for( i in 1:40){
    snp[,i] <- as.integer(snp[,i])
}
```

```r
var <- c(rep(1,3),rep(2,5),rep(1,3),rep(2,29))
snp2 <- as.matrix(snp)
colnames(snp2) <- NULL
rownames(snp2) <- NULL
snp2
snp3 = t(snp2)
```

```r
mod.lfmm <- lfmm_ridge(Y = snp3[,1:40],
                       X = scale(as.numeric(as.matrix(var))),
                       K = 6)

```

```r
pv <- lfmm_test(Y = snp3[1:40,],
                X = scale(as.numeric(as.matrix(var))),
                lfmm = mod.lfmm,
                calibrate = "gif")

## Manhattan plot with true associations shown
pvalues <- pv$calibrated.pvalue
plot(-log10(pvalues),
     pch = 19,
     cex = .3,
     xlab = "Probe",
     col = "grey")

th <- which(-log10(pvalues) > 4)
points(th,
       -log10(pvalues)[th],
        col = "blue")
```

```r
library(maptools)
library(raster)
library(tiff)
library(ggplot2)
library(dplyr)
```

##### download data from ``https://www.worldclim.org/data/worldclim21.html``
```r
setwd(dir = "Maps/")
str_name<-'wc2.1_2.5m_elev.tif'
imported_raster=raster(str_name)
DSM_HARV_pts <- rasterToPoints(imported_raster, spatial = TRUE)
# Then to a 'conventional' dataframe
DSM_HARV_df  <- data.frame(DSM_HARV_pts)
head(DSM_HARV_df)
```

```r
DSM_HARV_df_peru <- DSM_HARV_df %>% dplyr::filter(x > -85 & x < -65) %>%
                                        dplyr::filter(y > -18 & y < 0)
                                        
ggplot() +
    geom_raster(data = DSM_HARV_df_peru , aes(x = x, y = y, fill = wc2.1_10m_elev)) + 
    coord_equal()
    
```

