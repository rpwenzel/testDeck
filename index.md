---
title       : DDP Project
subtitle    : Summary of Clients' Stoplights
author      : Bob Wenzel
job         :
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      #
widgets     : [bootstrap, quiz, shiny, interactive]      # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides


---  plot #summary of clients' stoplights
## Summary of Clients' Stoplights
A client may have some or all of the following categories:

 - Alertattr, Categorycoverage, Catstat, Cmrattr, Cmrhealth, Doubleorders
 - Extremeprofile, Feederror, Feedmissing, Imagecheck, Optoutaging, Orderconsolidation
 - Orders, Promostaging, Sitevisits, Unrecproducts, Webattr, Wrdown

Stoplights are from over 400 clients, and can be Red, Yellow or Green,
They are created daily and depict the client's current status for each
of their categories.

"Summary of Clients' Spotlights" is a Shiny Application which summarizes the data for 
a date range specified by a user, giving the Total Records and percentage of each color.
It can be launched from a Browser using the following URL:

https://bwenzel.shinyapps.io/shinyapps/

On the following slides are the R code and its results that produces a sample graph using a date range: 
From: 2014-05-12  To: 2014-05-18

---  plot #pie chart

## A Pie Chart ##



```r
  require(ggplot2)
  library(MASS);  library(ggm);  library(RCurl)
  mainDir <- "c:/tmpDDP"
  if (!file.exists(mainDir)){ dir.create(file.path(mainDir))  }
  download.file("https://raw.githubusercontent.com/bwenzel/DDP2/master/input.txt",
  destfile = "c:/tmpDDP/input.txt", method = "curl")
  data=read.delim('c:/tmpDDP/input.txt',sep="\t")
  allstoplights=data.frame(data)
  stoplights=subset(allstoplights,((as.Date(data$date) > as.Date("2014-05-12")) &
  (as.Date(data$date) < as.Date("2014-05-18"))))
  count=nrow(stoplights)
  datacolor.freq=table(stoplights$color)
  colors=c('green','red','yellow')
  pie_labels <- round(datacolor.freq/sum(datacolor.freq) * 100, 1)
  pie_labels=paste(pie_labels,"%",sep="")
  main1="Summary of Clients' Stoplights\nTotal Records="
  main2=paste(main1,count)
```

---  plot #pie chart

## A Pie Chart Continued##



```r
  require(ggplot2)
  pie_pix=(pie(datacolor.freq,col=colors,main=main2,labels=pie_labels,cex=0.6))
  legend("topleft", cex=0.7, c("Good = (green rec. count / total rec.)%","Bad = (red rec. count / total records)%","Caution = (yellow rec. count / total records)%"), fill=colors)
```

<img src="assets/fig/pie-chart.png" title="plot of chunk pie-chart" alt="plot of chunk pie-chart" style="display: block; margin: auto;" />

---
## In Summary##
The data is currently available for the full month of May, and partial months of April and June, 2014.

If an invalid Range is specified, the following message will be displayed:
"Warning: No Data Found for the above Dates"

This Shiny application provides only an overview of all of the Stoplights' data.

More applcations hopefully will be developped to investigate the Stoplights data by Client and by Category.

Try the Shiny application changing from the default dates to your choice and click on the "Submit" button afterwards.

## See the Results and Enjoy!
