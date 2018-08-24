---
title: "R基本功之读取数据"
author: "Yuanzhen Lin"
date: '2018-08-24'
tags:
- R
- AAFMM
- readxl
- read files
---

数据是所有统计分析和图形绘制的基础，正如原料与菜谱之关系。数据读取正确与否，直接关系后续的统计分析结果的准确性，这好比原料新鲜与否，直接关系到后续菜肴的可口性。对于R初学者，尤其要重视数据的读取。本人主要从事林业统计分析，尤其是遗传评估，大部分涉及的变量多为因子型，但R语言并非如此智能，能直接读懂我们脑子里所想。在接触的一些网友问题里，我发现许多网友对数据是否正确读取没有什么概念，导致后续的许多分析结果都是错的。**<span style="color:red">请记住，只要给R语言任意一份看似合乎情理的数据，R似乎都能给出一些结果，但结果可能是错的！</span>**

<!--more-->

因此，本人在自编程序包`'AAFMM'`里加入了读取数据的函数read.file()和数据转换函数fdata()，前者用于直接读取文件，包括.csv文件，.txt文件，.dat和.asd等文件。对于excel文件，建议使用`'readxl'`包来读取,
再利用fdata()进行格式转换。

## 0 程序包安装

``` r
devtools::install_github(c('yzhlinscau/AAFMM'))
```

## 1 AAFMM涉及函数

  - read.example()
  - read.file()
  - fdata()

## 2 数据读取

### 2.1 读取.csv文件

包`'AAFMM'`的外置数据如下:

``` r
read.example(package = "AAFMM")
## [1] "barley.asd"  "fm.csv"    "mmex.txt"   "ZINC.DAT"
```

现在以文件"fm.csv"为例。

``` r
# set working directory
read.example(package = "AAFMM", setpath = TRUE)
# for user, if file 'fm.csv' under folder 'd:/Rdata', should use:
# setwd("d:/Rdata")

df<-AAFMM::read.file(file="fm.csv", header=TRUE)
df1<-read.csv(file="fm.csv", header=TRUE)
```
数据读取结果如下：
``` r
names(df)
##  [1] "TreeID"  "Spacing" "Rep"     "Fam"     "Plot"    "dj"      "dm"     
##  [8] "wd"      "h1"      "h2"      "h3"      "h4"      "h5"
names(df1)
##  [1] "TreeID"  "Spacing" "Rep"     "Fam"     "Plot"    "dj"      "dm"     
##  [8] "wd"      "h1"      "h2"      "h3"      "h4"      "h5"
all.equal(names(df),names(df1))
## [1] TRUE

head(df,5)
##   TreeID Spacing Rep   Fam Plot    dj    dm    wd h1  h2  h3  h4  h5
## 1  80001       3   1 70048    1 0.334 0.405 0.358 29 130 239 420 630
## 2  80002       3   1 70048    2 0.348 0.393 0.365 24 107 242 410 600
## 3  80004       3   1 70048    4 0.354 0.429 0.379 19  82 180 300 500
## 4  80005       3   1 70017    1 0.335 0.408 0.363 46 168 301 510 700
## 5  80008       3   1 70017    4 0.322 0.372 0.332 33 135 271 470 670
head(df1,5)
##   TreeID Spacing Rep   Fam Plot    dj    dm    wd h1  h2  h3  h4  h5
## 1  80001       3   1 70048    1 0.334 0.405 0.358 29 130 239 420 630
## 2  80002       3   1 70048    2 0.348 0.393 0.365 24 107 242 410 600
## 3  80004       3   1 70048    4 0.354 0.429 0.379 19  82 180 300 500
## 4  80005       3   1 70017    1 0.335 0.408 0.363 46 168 301 510 700
## 5  80008       3   1 70017    4 0.322 0.372 0.332 33 135 271 470 670
```

虽然读取的数据框df和df1里的变量名称和数据内容一样，但却是真假李逵之别。这可通过函数str()来区别。

``` r
str(df)
## 'data.frame':    827 obs. of  13 variables:
##  $ TreeID : Factor w/ 827 levels "80001","80002",..: 1 2 3 4 5 6 7 8...
##  $ Spacing: Factor w/ 2 levels "2","3": 2 2 2 2 2 2 2 2 2 2 ...
##  $ Rep    : Factor w/ 5 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Fam    : Factor w/ 55 levels "70001","70002",..: 44 44 44 15 15 2 2  ...
##  $ Plot   : Factor w/ 4 levels "1","2","3","4": 1 2 4 1 4 2 4 1 2 3 ...
##  $ dj     : num  0.334 0.348 0.354 0.335 0.322 0.359 0.368 0.358  ...
##  $ dm     : num  0.405 0.393 0.429 0.408 0.372 0.45 0.509 0.381  ...
##  $ wd     : num  0.358 0.365 0.379 0.363 0.332 0.392 0.388 0.369  ...
##  $ h1     : int  29 24 19 46 33 30 37 32 34 28 ...
##  $ h2     : int  130 107 82 168 135 132 124 126 153 127 ...
##  $ h3     : int  239 242 180 301 271 258 238 290 251 243 ...
##  $ h4     : int  420 410 300 510 470 390 380 460 430 410 ...
##  $ h5     : int  630 600 500 700 670 570 530 660 600 630 ...

str(df1)
## 'data.frame':    827 obs. of  13 variables:
##  $ TreeID : int  80001 80002 80004 80005 80008 80026 80028  ...
##  $ Spacing: int  3 3 3 3 3 3 3 3 3 3 ...
##  $ Rep    : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ Fam    : int  70048 70048 70048 70017 70017 70002 70002  ...
##  $ Plot   : int  1 2 4 1 4 2 4 1 2 3 ...
##  $ dj     : num  0.334 0.348 0.354 0.335 0.322 0.359 0.368 0.358  ...
##  $ dm     : num  0.405 0.393 0.429 0.408 0.372 0.45 0.509 0.381  ...
##  $ wd     : num  0.358 0.365 0.379 0.363 0.332 0.392 0.388 0.369  ...
##  $ h1     : int  29 24 19 46 33 30 37 32 34 28 ...
##  $ h2     : int  130 107 82 168 135 132 124 126 153 127 ...
##  $ h3     : int  239 242 180 301 271 258 238 290 251 243 ...
##  $ h4     : int  420 410 300 510 470 390 380 460 430 410 ...
##  $ h5     : int  630 600 500 700 670 570 530 660 600 630 ...
```

现在可以看到两个数据框的区别在于前5个变量，在df里为因子型，而在df1为数值型。它们的区别是什么呢？打个比方，差异就如伟人的特型演员与伟人自身的区别，即便外型上再高度相似，特型演员都不会是伟人！如果读者用心留意，会发现read.file()将以大写字母为词首的变量全部设为因子，是的，它就是这么干的！

回到R语言里，举个简单的方差分析为例，来看看两种数据的具体区别：

``` r
fit<-aov(h5~Rep+Fam,data=df)
fit1<-aov(h5~Rep+Fam,data=df1)
```

现在来看看方差分析的结果：

``` r
summary(fit)
##              Df  Sum Sq Mean Sq F value Pr(>F)    
## Rep           4  718227  179557    22.7 <2e-16 ***
## Fam          54  640573   11862     1.5 0.0134 *  
## Residuals   765 6049952    7908                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 3 observations deleted due to missingness

summary(fit1)
##              Df  Sum Sq Mean Sq F value Pr(>F)  
## Rep           1       1       1   0.000 0.9909  
## Fam           1   48242   48242   5.381 0.0206 *
## Residuals   821 7360509    8965                 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 3 observations deleted due to missingness
```

两个方差分析结果相去甚远！由结果可知，fit结果是正确的，而fit1结果是错误的！因为后者Rep和Fam的自由度df均为1，但事实上应分别为4和54。

两份数据集变量名一样，数据内容一样，但分析结果是一正一错！这是R语言的问题吗？不是，是我们自己的问题，R语言它不会读取我们的内心所想，需要我们告诉它数据的具体类型。

到此，我们是否应该加强对数据读取是否正确的重视呢？答案当然是肯定的。

### 2.2 读取其它文本文件

除了csv文件外，read.file()还可以读取其它文本文件，示例如下：

``` r
df2<-AAFMM::read.file(file="barley.asd",header=TRUE, sep=',')
df3<-AAFMM::read.file(file="mmex.txt",header=TRUE, sep='\t')
df4<-AAFMM::read.file(file="ZINC.DAT",header=TRUE, sep=' ')
```
通过参数sep来设定数据间的分隔符。

### 2.3 读取excel文件

对于excel文件，我本人一般将它存为csv文件，再读入R中。如果读者喜欢用excel文件，那就使用`readxl`包来读取excel文件。

``` r
read.example(package='readxl')
##  [1] "clippy.xls"    "clippy.xlsx"   "datasets.xls"  "datasets.xlsx"
##  [5] "deaths.xls"    "deaths.xlsx"   "geometry.xls"  "geometry.xlsx"
##  [9] "type-me.xls"   "type-me.xlsx"
```

不论是那种格式的excel文件，`readxl`包的read\_excel()都能读取：

``` r
read.example(package='readxl', setpath=TRUE) 
df5<-readxl::read_excel("datasets.xls", sheet=1)
df6<-readxl::read_excel("datasets.xlsx", sheet=1)
```

文件“datasets.xls”里的工作表如下：

``` r
readxl::excel_sheets("datasets.xls")
## [1] "iris"   "mtcars"   "chickwts"  "quakes"
```

fdata可以进行数据框的格式转换，示例如下：

``` r
df5b<-AAFMM::fdata(data=df5)
df6b<-AAFMM::fdata(data=df6, faS=5)
```

请注意，"datasets.xls"和"datasets.xlsx"事实上是一样的，只是excel版本的差异而已。但为演示，我将"datasets.xls"的第一个工作表里非因子型变量名词首改为小写，这样fdata(data=df5)就可完成格式，否则通过faS参数来指定因子的位置。转换结果如下：

``` r
str(df5)
## Classes 'tbl_df', 'tbl' and 'data.frame':    150 obs. of  5 variables:
##  $ sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : chr  "setosa" "setosa" "setosa" "setosa" ...

str(df5b)
## 'data.frame':    150 obs. of  5 variables:
##  $ sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 ...

str(df6b)
## 'data.frame':    150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 ...
```

## 3 结语

  - read.example()主要用于获取程序包的外置数据集，即原始数据，非.rda或.R内置数据。
  - read.file()主要用于读取非excel文件，将变量名以大写字母为词首的全部设为因子。
  - fdata()主要用于读取excel文件后的数据格式转换。

对于初学者，一句忠告：首先要知道变量的具体类型，然后将因子型的以大写字母作为词首命名，其它的交给read.file()或者fdata()来处理。此外，要多用函数str()来查看数据结构。
