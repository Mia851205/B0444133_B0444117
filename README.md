台灣旅遊情形
================

分析議題：台灣旅遊地點跟來台遊客的國家是否有關連
================================================

載入使用資料們
--------------

資料來源：政府資料開放平台 - 歷年來台旅客居住地統計：<http://data.gov.tw/node/7323> - 來台交通工具：<http://data.gov.tw/node/8359> - 歷年中華民國國民出國目的地人數統計：<http://data.gov.tw/node/7325> - 歷年來台旅客統計：<http://data.gov.tw/node/7322>

``` r
library(readxl)
transport <- read_excel("C:/Users/Umetsu/Desktop/R語言/CGUIM_BigData_HW6-sui-bian/來台交通工具.xlsx")
destination <- read_excel("C:/Users/Umetsu/Desktop/R語言/CGUIM_BigData_HW6-sui-bian/歷年中華民國國民出國目的地人數統計.xlsx")
country <- read_excel("C:/Users/Umetsu/Desktop/R語言/CGUIM_BigData_HW6-sui-bian/歷年來台旅客居住地統計.xlsx")
number <- read_excel("C:/Users/Umetsu/Desktop/R語言/CGUIM_BigData_HW6-sui-bian/歷年來台旅客統計.xlsx")
```

資料處理與清洗
--------------

1.將資料類別做轉換，轉換成正確的型態 2.將歷年來台旅客居住地統計依據101年合計跟100年合計做由排序，取前6筆資料

``` r
destination[[2]]= as.numeric(destination[[2]])
```

    ## Warning: NAs introduced by coercion

``` r
destination[[3]]= as.numeric(destination[[3]])
```

    ## Warning: NAs introduced by coercion

``` r
destination[[4]]= as.numeric(destination[[4]])
```

    ## Warning: NAs introduced by coercion

``` r
destination[[5]]= as.numeric(destination[[5]])
```

    ## Warning: NAs introduced by coercion

``` r
destination[[6]]= as.numeric(destination[[6]])
```

    ## Warning: NAs introduced by coercion

``` r
destination[[7]]= as.numeric(destination[[7]])
```

    ## Warning: NAs introduced by coercion

``` r
destination[[8]]= as.numeric(destination[[8]])
```

    ## Warning: NAs introduced by coercion

``` r
destination[[9]]= as.numeric(destination[[9]])
```

    ## Warning: NAs introduced by coercion

``` r
number[[3]]= as.numeric(number[[3]])
```

    ## Warning: NAs introduced by coercion

``` r
number[[5]]= as.numeric(number[[5]])
```

    ## Warning: NAs introduced by coercion

``` r
number[[8]]= as.numeric(number[[8]])
```

    ## Warning: NAs introduced by coercion

``` r
country[[9]]= as.numeric(country[[9]])
```

    ## Warning: NAs introduced by coercion

``` r
country1 = head(country[order(country[[2]],decreasing = T),])
country2 = head(country[order(country[[5]],decreasing = T),])
```

假設一：105年台人出國旅遊的目的地是否與距離台灣遠近有關
-------------------------------------------------------

``` r
library(ggplot2)
qplot(destination$`105年` ,destination$國家, data = destination,color=`105年`,aes(size=20))
```

    ## Warning: Ignoring unknown parameters: NA

![](README_files/figure-markdown_github/unnamed-chunk-3-1.png)

假設二：101年台人出國旅遊的目的地是否與旅客來台的居住地有正相關
---------------------------------------------------------------

``` r
a = merge(destination,country,by="國家")
a2=a[,1]
a3=a[,12]
a6=a[,17]
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
a4 = cbind(a2,a3)
a5=data.frame(a4)
a7 = cbind(a5,a6)
colnames(a7) <- c("國家","101年台灣人出國目的地","101年來台旅客居住地")
#str(a7)
library(ggplot2)
qplot(a7$`101年來台旅客居住地`,as.numeric(as.character(a7$`101年台灣人出國目的地`)), data = a7,geom = c("point", "smooth"),color=國家,aes(size=10))
```

    ## Warning: Ignoring unknown parameters: NA

    ## Warning: Ignoring unknown parameters: NA

    ## `geom_smooth()` using method = 'loess'

![](README_files/figure-markdown_github/unnamed-chunk-4-1.png)

假設三：桃園機場是否為台灣出入境主要的地點(以101年資料作探討)
-------------------------------------------------------------

``` r
library(ggplot2)
ggplot(transport,aes(x=transport$`桃園(飛機)`,y=transport$`小計(飛機)`))+geom_line()
```

![](README_files/figure-markdown_github/unnamed-chunk-5-1.png)

假設四：101年旅客來台搭乘的交通工具為輪船的是否大多都為海島國家
---------------------------------------------------------------

``` r
library(ggplot2)
qplot(transport$`小計(輪船)`,transport$國家, data = transport,aes(size=50))
```

    ## Warning: Ignoring unknown parameters: NA

![](README_files/figure-markdown_github/unnamed-chunk-6-1.png)

分析結果
--------

來台旅客的國家主要跟地理關係還有交通便利性有關，大多都是亞洲國家來台旅遊，台灣人有大多都往亞洲國家旅行，並且大多採取搭飛機的模式做為轉承的交通工具，其中桃園機場為最重要的台灣對外交通樞紐，在此搭乘飛機的數量與搭飛機來台的人總人數呈現密切的正相關。

分析結果可能解決的問題
----------------------

可以增加南臺灣的交通便利性，尤其是高雄機場的部分，藉此或許能夠吸引南亞洲的國家從南台灣登陸來遊玩

組員名單與分工
--------------

B0444117 林育弘：資料的初步整理、做投影片、修改圖的細處(顏色、大小)、按投影片 B0444133 陳美樺：抓資料、打程式碼、匯入資料、做圖、上台報告
