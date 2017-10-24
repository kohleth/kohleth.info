---
title: Things I learnt integrating R in production
author: Kohleth Chia
date: '2017-10-20'
categories:
  - r
slug: things-i-learnt-integrating-r-in-production
draft: false
---

In the past 1.5 years I have been working on a project which develops a web application to facilitate certain medical diagnosis. In the core of this application is a bunch of R scripts which perform some prediction.

This was really my first experience writing production grade R scripts which is embedded in another system, and which will be deployed on another computer. This was very different to the usual interactive R programming that I do daily. 

So, this is a (running) list of things that I have learnt from this experience.

### 1. Version control your R libraries early
When your script depends on a bunch of R packages (which seems to be the case these days as R packages become more specialized), you will need to make sure those packages are available on the new computer that will run those scripts. R Studio has [`packrat`](//rstudio.github.io/packrat/)
And Microsoft has [`checkpoint`](//mran.microsoft.com/package/checkpoint/). But I did not know about any of these when I started the project. 

So when I learnt about this at the end of the project, trying then to version control the project dependencies becomes almost mission impossible. I could never get `pactrak` to work. So I ended up just installing a new clean version of R and the associated base library, then run everything scripts which loads a library and install all the reported missing ones. Once I have built this library I just ship the entire folder today with my scripts.

The downside of this approach is that you will be shipping a large R library folder (mine being ~300MB), when all this could actually be installed from CRAN/github. Furthermore, you will be shipping all the base R libraries that you don't actually need (e.g. `forecast` in my case).

However, the benefit of this is, firstly, it actually works (compared to `packrat`). Secondly, there is no need for the destination R to re-install anything. Everything should work out of box. This is particular useful when you depend on non-CRAN packages. It just makes the whole process smoother.

### 2. Version control your R
If you are like me, you probably think the only version that matters is the version of R packages. Well, turns out, the library I built in the step above would only work with R >=3.4.0. I don't really know why but upgrading R on the destination machine solves some problems for me.

### 3. `Pandoc` does not come with R (or `rmarkdown` or `knitr`)!
After using RStudio for a while, you don't really remember what comes with base R and what is actually an add on from RStudio. [`pandoc`](//pandoc.org/MANUAL.html#options) turns out to be one of them. [`pandoc`](//pandoc.org/MANUAL.html#options) is the core engine behind [`rmarkdown`](//rmarkdown.rstudio.com) and `knitr`. Every time you 'knit' a markdown document, [`pandoc`](//pandoc.org/MANUAL.html#options)` is called. 

One functionality of my production script is to automatically generate report using [`rmarkdown`](//rmarkdown.rstudio.com), and such functionality will be called programmatically via `Rscript`. Since all this happens outside RStudio (the destination machine doesn't even have RStudio installed), the `knit` component wouldn't actually work because it wouldn't be able to find the [`pandoc`](//pandoc.org/MANUAL.html#options) engine. 

The solution is to manually copy the [`pandoc`](//pandoc.org/MANUAL.html#options) engine across, as outlined in [this] (//stackoverflow.com/questions/28432607/pandoc-version-1-12-3-or-higher-is-required-and-was-not-found-r-shiny) SO answer.


### 4. How to invoke R with argurment from the terminal
This is critical to know when your R script will be executed by another program via terminal using `Rscript`.

The solution is to use `args=commandArgs()` as outlined in [this](//www.vscentrum.be/cluster-doc/software/r-cla-in-scripts) page.

### 5. Trim the fat from your model!
R model objects can be quite big, especially when it contains the entire training dataset. For example, 

```
library(caret)
myglm=train(Species~.,data=droplevels(iris[1:100,]),method="glm")
print(object.size(myglm),unti="auto")`
# 398056 bytes
```
the above code produces a glm that is almost 400KB large, even though if all we need is prediction, all we need are 5 numbers `coef(myglm$finalModel)` or add another 26 if we need prediction interval `vcov(myglm$finalModel)`.

This is a real problem in production when things need to be loaded and executed fast and smooth.

Fortunately, you can remove most model component for production use, i.e. [trim the fat]
(//www.win-vector.com/blog/2014/05/trimming-the-fat-from-glm-models-in-r/). However, doing so would probably destroy some functionality of the model object, such as displaying a nice summary when `print`. But this shouldn't matter if the functionality is not called in production. Just make sure you thoroughly test the trimmed model object! (e.g. `all.equal(predict(trimed_model,newdata), predict(original_model,newdata))`)