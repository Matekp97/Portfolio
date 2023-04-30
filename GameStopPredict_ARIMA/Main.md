

```R
library(ggplot2)
library(TSA)
library(forecast)

GME=read.csv("GME.csv",sep=",");
GME=data.frame(GME$Date,GME$Adj.Close)
dt=as.Date(GME$GME.Date, format="%Y-%m-%d");
X=GME$GME.Adj.Close
```


```R
GMEg= ggplot(GME, aes(x = dt, y = X)) + geom_line() +
  labs(title="Adjusted Closure of GME in the last year", x="Months", y="Value") +
  scale_x_date(breaks = pretty(dt,n=10),date_labels = "%m");
GMEg
```


![png](output_1_0.png)



```R
T = length(dt);
r = ((X[2:T]-X[1:(T-1)])/X[1:(T-1)])*100
plot(r)
```


![png](output_2_0.png)



```R
x = diff(log(X))*100
plot(x)
```


![png](output_3_0.png)



```R
library(fBasics)
hist(r)
```


![png](output_4_0.png)



```R
basicStats(r)
s1 = skewness(r)[1]; s1
t1 = s1/sqrt(6/length(r))
pv = 2*(1-pnorm(t1))
pv
t.test(x)
normalTest(x,method="jb")
```


<table class="dataframe">
<caption>A data.frame: 16 × 1</caption>
<thead>
	<tr><th></th><th scope=col>r</th></tr>
	<tr><th></th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>nobs</th><td>250.000000</td></tr>
	<tr><th scope=row>NAs</th><td>  0.000000</td></tr>
	<tr><th scope=row>Minimum</th><td>-60.000000</td></tr>
	<tr><th scope=row>Maximum</th><td>134.835802</td></tr>
	<tr><th scope=row>1. Quartile</th><td> -3.347393</td></tr>
	<tr><th scope=row>3. Quartile</th><td>  4.757696</td></tr>
	<tr><th scope=row>Mean</th><td>  2.832840</td></tr>
	<tr><th scope=row>Median</th><td>  0.195741</td></tr>
	<tr><th scope=row>Sum</th><td>708.209952</td></tr>
	<tr><th scope=row>SE Mean</th><td>  1.108859</td></tr>
	<tr><th scope=row>LCL Mean</th><td>  0.648901</td></tr>
	<tr><th scope=row>UCL Mean</th><td>  5.016779</td></tr>
	<tr><th scope=row>Variance</th><td>307.392192</td></tr>
	<tr><th scope=row>Stdev</th><td> 17.532604</td></tr>
	<tr><th scope=row>Skewness</th><td>  3.234629</td></tr>
	<tr><th scope=row>Kurtosis</th><td> 20.206332</td></tr>
</tbody>
</table>




3.2346288400622



0



    
    	One Sample t-test
    
    data:  x
    t = 1.6369, df = 249, p-value = 0.1029
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
     -0.3215156  3.4854378
    sample estimates:
    mean of x 
     1.581961 
    



    
    Title:
     Jarque - Bera Normalality Test
    
    Test Results:
      STATISTIC:
        X-squared: 1734.6067
      P VALUE:
        Asymptotic p Value: < 2.2e-16 
    
    Description:
     Tue Jan 25 00:13:12 2022 by user: Mattia
    



```R
acf(r);
```


![png](output_6_0.png)



```R
pacf(r)
```


![png](output_7_0.png)



```R
Box.test(r,lag = log(length(r)),type="Ljung-Box")
```


    
    	Box-Ljung test
    
    data:  r
    X-squared = 33.084, df = 5.5215, p-value = 6.269e-06
    



```R
ar(r)$aic
```


<style>
.dl-inline {width: auto; margin:0; padding: 0}
.dl-inline>dt, .dl-inline>dd {float: none; width: auto; display: inline-block}
.dl-inline>dt::after {content: ":\0020"; padding-right: .5ex}
.dl-inline>dt:not(:first-of-type) {padding-left: .5ex}
</style><dl class=dl-inline><dt>0</dt><dd>32.1232690673944</dd><dt>1</dt><dd>32.743087818983</dd><dt>2</dt><dd>28.187781179015</dd><dt>3</dt><dd>22.0529702244726</dd><dt>4</dt><dd>0.0647341602300457</dd><dt>5</dt><dd>2.02691160555992</dd><dt>6</dt><dd>3.73155885533311</dd><dt>7</dt><dd>5.46344913088046</dd><dt>8</dt><dd>4.63953766485861</dd><dt>9</dt><dd>4.05553831230645</dd><dt>10</dt><dd>0</dd><dt>11</dt><dd>1.10013775465586</dd><dt>12</dt><dd>3.10012213427353</dd><dt>13</dt><dd>3.83966095178653</dd><dt>14</dt><dd>5.21920531331602</dd><dt>15</dt><dd>6.08313074590455</dd><dt>16</dt><dd>4.8417141506236</dd><dt>17</dt><dd>5.35484336443506</dd><dt>18</dt><dd>7.12759306421594</dd><dt>19</dt><dd>6.91401615364998</dd><dt>20</dt><dd>6.50799639904699</dd><dt>21</dt><dd>6.80794438832572</dd><dt>22</dt><dd>8.45961564506365</dd><dt>23</dt><dd>10.3852452670158</dd></dl>




```R
M=Arima(r,order = c(10,0,0))
Arima(r,order = c(4,0,0))

roots = polyroot(c(1,-M$coef[1:10])) 
croots = 1/roots
Mod(croots)
```


    Series: r 
    ARIMA(4,0,0) with non-zero mean 
    
    Coefficients:
             ar1     ar2     ar3      ar4    mean
          0.0897  0.1973  0.1898  -0.3033  2.8572
    s.e.  0.0601  0.0593  0.0592   0.0602  1.2349
    
    sigma^2 estimated as 265.3:  log likelihood=-1050.09
    AIC=2112.17   AICc=2112.52   BIC=2133.3



<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>0.860536475476584</li><li>0.894471581701726</li><li>0.894471581701726</li><li>0.860536475476584</li><li>0.841992591467478</li><li>0.841098786829885</li><li>0.841992591467478</li><li>0.760171346936879</li><li>0.76017134693688</li><li>0.841098786829885</li></ol>




```R
Arima(r,order = c(10,0,0))
```


    Series: r 
    ARIMA(10,0,0) with non-zero mean 
    
    Coefficients:
             ar1     ar2     ar3      ar4     ar5      ar6     ar7     ar8     ar9
          0.0951  0.2029  0.2146  -0.2838  0.0138  -0.1284  0.0459  0.1255  0.1216
    s.e.  0.0622  0.0623  0.0632   0.0646  0.0670   0.0667  0.0646  0.0631  0.0635
             ar10    mean
          -0.1717  2.8900
    s.e.   0.0641  1.2987
    
    sigma^2 estimated as 257.7:  log likelihood=-1043.62
    AIC=2111.24   AICc=2112.55   BIC=2153.49



```R
Arima(r,order = c(10,0,0), fixed=c(0,NA,NA,NA,0,0,0,NA,0,0,0))
```


    Series: r 
    ARIMA(10,0,0) with non-zero mean 
    
    Coefficients:
          ar1     ar2     ar3      ar4  ar5  ar6  ar7     ar8  ar9  ar10  mean
            0  0.2200  0.2111  -0.2442    0    0    0  0.1119    0     0     0
    s.e.    0  0.0594  0.0587   0.0606    0    0    0  0.0595    0     0     0
    
    sigma^2 estimated as 268.8:  log likelihood=-1052.27
    AIC=2114.54   AICc=2114.79   BIC=2132.15



```R
M=Arima(r,order = c(4,0,0),fixed=c(0,NA,NA,NA,NA))
roots = polyroot(c(1,-M$coef[1:4])) 
croots = 1/roots
Mod(croots)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>0.673277056426473</li><li>0.795040188963044</li><li>0.795040188963044</li><li>0.673277056426473</li></ol>




```R
res = M$residuals
plot(res)
```


![png](output_14_0.png)



```R
acf(res)
```


![png](output_15_0.png)



```R
Box.test(res,lag = 25, fitdf=4)
tsdiag(M)
normalTest(res,method="jb")
```


    
    	Box-Pierce test
    
    data:  res
    X-squared = 31.149, df = 21, p-value = 0.07122
    



    
    Title:
     Jarque - Bera Normalality Test
    
    Test Results:
      STATISTIC:
        X-squared: 4117.8796
      P VALUE:
        Asymptotic p Value: < 2.2e-16 
    
    Description:
     Tue Jan 25 00:14:22 2022 by user: Mattia
    



![png](output_16_2.png)



```R
T=length(r)
t=T-10
Fr=r[1:t]
FM=Arima(Fr,order = c(4,0,0),fixed=c(0,NA,NA,NA,NA))
P = predict(FM,10)                           
ls.str(P)                                      
plot(c(Fr,P$pred))
points(c(rep(NA,240),P$pred),col="red")
```


    pred :  Time-Series [1:10] from 241 to 250: 2.977 0.784 2.69 5.382 2.393 ...
    se :  Time-Series [1:10] from 241 to 250: 16.1 16.1 16.4 16.8 17.3 ...



![png](output_17_1.png)



```R
MR=max(r[t:T])
mR=min(r[t:T])

k = 30;
plot(r[(T-k):T],ylim=c(mR-1,MR+1))
points(c(rep(NA,k-10+1),P$pred),col="red")
lines(c(rep(NA,k-10),x[T],P$pred+1.96*P$se),col="red",lwd=2,lty=2)
lines(c(rep(NA,k-10),x[T],P$pred-1.96*P$se),col="red",lwd=2,lty=2)
```


![png](output_18_0.png)



```R

```
