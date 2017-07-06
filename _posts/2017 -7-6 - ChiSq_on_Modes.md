------------------------------------------------------------------------

<span style="color:blue">Mode Vector</span>
-------------------------------------------

<center>
![](images/WhyMode.png)
</center>
<p>
Mode gives the most common type category per move. Hence I assyme it is representative.
</p>
<p>
From the Detailed Mode Vector that contains the I take only the modes for every category.
</p>
<p>
Below you can see how complete these mode vectors are:
</p>
![](images/Sparcity%20of%20Mode%20vector%20data.png)
<p>
**NB!** Age sex table is split into most common age group for males and females.
</p>

------------------------------------------------------------------------

<span style="color:blue">Chi-square test for independence </span>
-----------------------------------------------------------------

H<sub>0</sub> : Family and tenure ~~and gender~~ are independent.
Significance level: *α* = 0.05 = 5%

**1. Make contigency table with all attributes**

<p>
With the three topics we will build 3 x 2-way contingency tables.
</p>
<p>
2-way because Tenure will be agaist Family type.
</p>
<p>
~~3 tables because gender can be male, female or both.~~
</p>
**Family**

``` r
vector_Family <-  as.numeric(mode_Vector[,3])
vector_Family[vector_Family<3] <- "!Family" ##-- Cells: 1 & 2
vector_Family[vector_Family == 3] <- "Couple" 
vector_Family[vector_Family == 9] <- "!Household"
vector_Family[(vector_Family >= 4) & (vector_Family <= 8) ] <- "Parent"
```

**Tenure**

``` r
vector_Tenure <- c("R","O","RF")[interaction(mode_Vector[,5] <= 4, mode_Vector[,5] >= 6)]
```

\*\* ~~Gender~~ \*\* THIS NEEDS FIXING

``` r
vector_Gender <- c("B","M","F")[interaction(is.na(mode_Vector[,7]),
                                            is.na(mode_Vector[,6]))]
#Gender <- factor(vector_Gender, levels = c("B","M","F"))
```

[**Contigency Table - Observed Values**](http://onlinestatbook.com/2/chi_square/contingency.html)

``` r
Family <- factor(vector_Family, levels = c("!Family","Couple","Parent","!Household"))
Tenure <- factor(vector_Tenure, levels = c("R","O","RF"))
myTable <- table(Family,Tenure)
myTable
```

    ##             Tenure
    ## Family           R     O    RF
    ##   !Family    23779 10454   541
    ##   Couple     26376 22805   801
    ##   Parent      5797 11653   242
    ##   !Household  8562  4042   161

<p>
**2. Calculate the expected values per cell**
</p>
[**Expected Values**](http://onlinestatbook.com/2/chi_square/contingency.html)
$$E\_{ij} =  \\frac{P(Row)\_{i}}{All} \* \\frac{P(Column)\_{j}}{All} = \\frac{(Row \\ Total  \* Column \\ Total  )\_{ij} }{Grand \\ Total} $$

``` r
library(vcd)
```

    ## Loading required package: grid

``` r
chisq.test(myTable)$expected
```

    ##             Tenure
    ## Family               R         O       RF
    ##   !Family    19471.846 14775.471 526.6821
    ##   Couple     27987.629 21237.350 757.0204
    ##   Parent      9906.709  7517.330 267.9606
    ##   !Household  7147.815  5423.848 193.3369

<p>
**3. Calculate *χ*<sup>2</sup>**
</p>
[**Chi-square Contribution**](http://www.ucl.ac.uk/english-usage/resources/ftfs/experiment2.htm)
$$Chi-Square\\ Contribution =  \\frac{(O - E)^2}{E}$$

``` r
library(gmodels)
myTable <- xtabs(Freq ~ Tenure + Family, data=myTable)
CrossTable(myTable ,prop.t=FALSE,prop.r=FALSE,prop.c=FALSE)
```

    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  115213 
    ## 
    ##  
    ##              | Family 
    ##       Tenure |    !Family |     Couple |     Parent | !Household |  Row Total | 
    ## -------------|------------|------------|------------|------------|------------|
    ##            R |      23779 |      26376 |       5797 |       8562 |      64514 | 
    ##              |    952.738 |     92.803 |   1704.876 |    279.794 |            | 
    ## -------------|------------|------------|------------|------------|------------|
    ##            O |      10454 |      22805 |      11653 |       4042 |      48954 | 
    ##              |   1263.927 |    115.717 |   2275.245 |    352.057 |            | 
    ## -------------|------------|------------|------------|------------|------------|
    ##           RF |        541 |        801 |        242 |        161 |       1745 | 
    ##              |      0.389 |      2.555 |      2.515 |      5.409 |            | 
    ## -------------|------------|------------|------------|------------|------------|
    ## Column Total |      34774 |      49982 |      17692 |      12765 |     115213 | 
    ## -------------|------------|------------|------------|------------|------------|
    ## 
    ## 

[**Pearson's cumulative test statistic:**](http://www.statisticshowto.com/probability-and-statistics/chi-square/)
$$ Pearson's \\ Statistic= \\chi^2 = \\sum\_{i=1}^{n}\\left( \\frac{(O\_i - E\_i)^2}{E\_i } \\right)$$

``` r
chisq.test(myTable)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  myTable
    ## X-squared = 7048, df = 6, p-value < 2.2e-16

<center>
**NB! Double checking the zero p-val: **
</center>
``` r
SanityCheck_ChiStat <- sum((abs(chisq.test(myTable)$observed - chisq.test(myTable)$expected))^2 / chisq.test(myTable)$expected)
SanityCheck_ChiStat
```

    ## [1] 7048.026

``` r
SanityCheck_pVal  <- chisq.test(myTable)$p.value
SanityCheck_pVal
```

    ## [1] 0

![](170706_SupMeet_files/figure-markdown_github/density%20graph-1.png)

------------------------------------------------------------------------

<span style="color:blue">Data</span>
------------------------------------

<p>
**Data**: Using the ONS 2011 SMS dataset at LA level.
</p>
<https://wicid.ukdataservice.ac.uk/cider/about/data_int.php?type=1>.

<p>
**Tables**: MU02UK (Family Status), MU06UK (Tenure) and MM01AUK (Age by Sex). Hope to include MU05BUK (NS-SeC (grouped))
</p>
<p>
First, I do a bulk download, then sort through the csv to recognise the begining index of a new OD-table. Themes are organised by tables, names by their code. Every table has unique cell numbers. These cells are subtables that specify some group by theme category and are what I match by in the mode vectors.
</p>
<center>
<h3>
Mode Vector
</h3>
</center>
![](images/mode_Vect_str.png)

<center>
<h3>
Family Status
</h3>
</center>
![](images/Family.png)
<center>
<h3>
Tenure
</h3>
</center>
![](images/Tenure.png)
<center>
<h3>
Age divided by Gender
</h3>
</center>
![](images/Capture.png)

------------------------------------------------------------------------
