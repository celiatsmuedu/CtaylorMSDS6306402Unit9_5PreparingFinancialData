# CTaylorMSDS6306402Unit9_5Stocks_1
Celia Taylor  
July 21, 2016  



#######  This R Markdown document is based on Dr. Monnie McGee's instruction in SMU MSDS6306 Unit 9.5.

######  Assigned NYSE: PSXP, Phillips 66 Partners LP

```r
## Financial Series Unit 9.5 Preparing Financial Data
## Download the data
## Calculate log returns
## Calculate volatility measure
## Calculate a volatitily measure with a continuous lookback window
## Plot the results with a volatility curve overlay.
## Deliverable 
## * Post a link to R markdown document on GitHub
## * Include code for 
## ** entering data, 
## ** calculating log returns
## ** calculating volatility measure
## ** calculating volatility for the entire series using three different decay factors.
## * Include code for plot (and the plot) with volatility estimates for various values of d overlaid.
## ** 
install.packages("downloader", repos="http://cran.rstudio.com/")
```

```
## Installing package into 'C:/Users/Celia Taylor/Documents/R/win-library/3.2'
## (as 'lib' is unspecified)
```

```
## package 'downloader' successfully unpacked and MD5 sums checked
## 
## The downloaded binary packages are in
## 	C:\Users\Celia Taylor\AppData\Local\Temp\RtmpKaOabA\downloaded_packages
```

```r
library(downloader)
install.packages("tseries", repos="http://cran.rstudio.com/")
```

```
## Installing package into 'C:/Users/Celia Taylor/Documents/R/win-library/3.2'
## (as 'lib' is unspecified)
```

```
## package 'tseries' successfully unpacked and MD5 sums checked
## 
## The downloaded binary packages are in
## 	C:\Users\Celia Taylor\AppData\Local\Temp\RtmpKaOabA\downloaded_packages
```

```r
library(tseries) 
```
###### Download the data for NYSE: PSXP, Phillips 66 Partners LP

```r
SNPdata <- get.hist.quote(instrument="PSXP",quote = "Close", provider = "yahoo")
```

```
## Warning in download.file(url, destfile, method = method, quiet = quiet):
## downloaded length 47703 != reported length 200
```

```
## time series starts 2013-07-23
```

```r
length(SNPdata) 
```

```
## [1] 755
```
###### Calculate the log returns

```r
## ** Calculating log returns
SNPret <- log(lag(SNPdata)) - log(SNPdata)
length(SNPdata)
```

```
## [1] 755
```
###### Standard Deviation of SNP returns times 250 trading days in a year multiply by 100 to make it a percentage
###### Calculate the Volatility Measure

```r
###### Calculate the Volatility Measure
SNPvol <- sd(SNPret) * sqrt(250) * 100
# 
SNPvol
```

```
## [1] 37.308
```

```r
## Volatility function, Vol, gets volatility in a continuous lookback window way - not just one number but various numbers that represent volatility at particular periods of time.  Function named Vol and is a function of d and the log returns.  D is the number of values we go back in time.  Initialize the variables variance and lambda.  Varlist is a vector.  Loop goes through the log returns, the data that Dr. McGee has.  Lam is the exponential value that is the weight that is multiplied to each return.  D has the same sort of function as s.  D is a value that forms our weight function.  Function creates the weight of (.9) or 1/d.  Then calculate the variance for the particular one and take the square root.  Function returns the variance list and the variance itself.
Vol <- function(d, logrets){
        var = 0
        lam = 0
        varlist <- c()
        for (r in logrets) {
               lam = lam*(1-1/d) + 1
        var = (1-1/lam)*var + (1/lam)*r^2
                varlist <- c(varlist, var)
        }
          sqrt(varlist)}
```
###### Calculate the Volatility Measure for the entire series using three different decay factors at 10, 30, and 100.

```r
## ** Calculating volatility for the entire series using three different decay factors.
#Recreate Figure 6.12 in the text on page 155
volest <- Vol(10,SNPret)
volest2 <- Vol(30,SNPret)
volest3 <- Vol(100,SNPret)
```
###### Plot the volatility estimates for various values of d overlaid.  The plot shows the Volatility Measure for the entire series using three different decay factors at 10 (black), 30 (red), and 100 (blue).

```r
## ** Calculating volatility for the entire series using three different decay factors.
## * Include code for plot (and the plot) with volatility estimates for various values of d overlaid.
plot(volest, type="l", main = "NYSE: PSXP, Phillips 66 Partners LP")
lines(volest2,type="l", col="red")
lines(volest3, type = "l", col="blue")
```

![](CTaylorMSDS6306402Unit9_5PSXP_7_files/figure-html/PlotVolatility1-1.png)<!-- -->
