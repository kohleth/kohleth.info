---
title: Things I learnt developing production grade R projects
author: Kohleth Chia
date: '2017-10-20'
categories:
  - r
slug: things-i-learnt-developing-production-grade-r-projects
draft: yes
---

In the past 1.5 years I have been working on a project which develops web application to facilitate certain medical diagnosis. In the core of this application is a bunch of R scripts which perform some prediction.

This was really my first experience writing production grade R scripts which is embedded in another system, and which will be deployed on another computer. This was very different to the usual interactive R programming that I do daily. 

So, below are the four things that I have learnt from this experience.

# 1. Version control your R libraries early
When your script depends on a bunch of R packages (which seems to be the case these days as R packages become more specialized), you will need to make sure those packages are available on the new computer that will run those scripts. R Studio has `packrat`[\\]
And Microsoft has `checkpoint`[\\]. But I did not know about any of these when I started the project. 

So when I learnt about this at the end of the project, trying then to version control the project dependencies becomes almost mission impossible. I could never get `pactrak` to work. So I ended up just installing a new clean version of R and the associated base library, then run everything scripts which loads a library and install all the reported missing ones. Once I have built this library I just ship the entire folder today with my scripts.

The downside of this approach is that you will be shipping a large R library folder (mine being ~300MB), when all this could actually be installed from CRAN/github. Furthermore, you will be shipping all the base R libraries that you don't actually need (e.g. forecast in my case).

However, the benefit of this is, firstly, it actually works (compared to `packrat`). Secondly, there is no need for the destination R to re-install anything. Everything should work out of box. This is particular useful when you depend on non-CRAN packages. It just makes the whole process smoother.

# 2. Version control your R

# 3. Pandoc does not come with R (or rmarkdown or knitr)!

# 4. How to invoke R with argurment from terminal

# 5. Global log switch

# 6. Trim the fat from your model!

