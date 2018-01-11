---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- "What is Bioconductor and why would I use it?"
objectives:
- "Describe what kinds of resources Bioconductor provides and give an example of what they might be useful for."
- "Locate Bioconductor packages and learn their intended purpose."
- "Locate Bioconductor package reference manuals and vignettes."
- "Install Bioconductor packages using biocLite."
keypoints:
- "First key point."
---

### What is Bioconductor?

Bioconductor is a collection of open-source tools that use R. It contains over 1400 R packages that can be used to analyze many types of biological data. A major benefit of Bioconductor is that the packages are usually well-documented, and there is an active community of users and developers.

There are three types of Bioconductor packages:
- Software: packages of code that can be used to analyze data
- Annotations: packages that contain information like genome sequences or transcript coordinates
- Experiments: packages that contain experimental data, usually used to provide examples and test software

(maybe show an image here)

Bioconductor packages are available for many different steps in data analysis pipelines. Often, you can choose between several different Bioconductor packages for the same step in the pipeline or biological question. This allows you to try different approaches and pick which one is the best fit for your project or data.

### Install Bioconductor

There is a special method of installing Bioconductor packages, called biocLite, that is different from installing other R packages. To get started, first you need to get biocLite. To do so, run this command in RStudio:

~~~
source("https://bioconductor.org/biocLite.R")
biocLite()
~~~
{: .r}

Now you can use BiocLite to install Bioconductor packages.
