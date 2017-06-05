---
layout: post
title: Special Cases of Turn
author: Bonnie Buyuklieva
date: 5 June 2017
catergory: Migration
tags: England&Wales 2011 SMS
---

The first step understanding migration is getting a sense of what is 'normal'.
To do this I am looking at several measures, including turn (after [Dennett & Stillwell, 2008](https://www.researchonline.org.uk/sds/search/download.do%3Bjsessionid=B8F5D490363B64052BC2400530827BDB?ref=A20797)).

Defintions:
Outliers are defined as any values that lie beyond 1.3 of the IQR 
Turn = In+Out/(Total Pop) Churn = In+Out+Within/(Total Pop)

Summary Stats
-------------

Turn

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.0390  0.0700  0.0870  0.2779  0.1090 54.1600

![](images/HistoT-1.png)

Outliers
--------

Outliers (ie, values further than 1.3 the IQR)

    ##                      Name   turn medi  churn netMigRate
    ## 1                 Hackney  0.171    1  0.234     0.0042
    ## 2                Haringey  0.184    1  0.244    -0.0047
    ## 3                  Oxford  0.189    1  0.305     0.0109
    ## 4               Southwark  0.192    1  0.257     0.0011
    ## 5               Cambridge  0.200    1  0.290     0.0002
    ## 6           Tower Hamlets  0.209    1  0.278     0.0035
    ## 7                 Lambeth  0.211    1  0.276     0.0038
    ## 8  Kensington and Chelsea  0.215    1  0.275    -0.0029
    ## 9              Wandsworth  0.223    1  0.296     0.0039
    ## 10               Cornwall  0.224    1  0.305     0.0075
    ## 11                 Camden  0.240    1  0.302    -0.0063
    ## 12 Hammersmith and Fulham  0.245    1  0.307     0.0030
    ## 13            Westminster  0.345    1  0.407    -0.0093
    ## 14         City of London 10.251    1 12.119    -0.2759
    ## 15        Isles of Scilly 54.157    1 73.795     1.8112

Map
---

England and Wales

![](/images/mapDRAW-1.png)

London

![](/images/mapDRAW_lnd-1.png)
