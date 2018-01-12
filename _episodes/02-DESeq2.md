---
title: "DESeq2: A Bioconductor Package"
teaching: 0
exercises: 0
questions:
- "How do I install and use a Bioconductor package?"
- "How can I find which genes are differentially expressed in RNA-Seq data?"
objectives:
- "Install the Bioconductor package DESeq2 using biocLite."
- "Follow a workflow in a Bioconductor package vignette."
- "Use DESeq2 to identify differentially expressed genes in sample RNA-Seq
data."
keypoints:
- "DESeq2 is one example of a well-documented Bioconductor package."
- "DESeq2 works as one step in a data analysis pipeline, detecting
differentially expressed genes from gene counts."
- "In addition to performing statistical calculations, DESeq2 has functions for
visualization and export of results."
---

### What is DESeq2?

DESeq2 is a popular Bioconductor package that is used to identify differentially
expressed genes in RNA-Seq data. It is well-documented and widely used: the
[paper describing it](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-014-0550-8)
 has been cited thousands of times.

### Installing DESeq2

We will use biocLite to install DESeq2 by using the following command in
RStudio:

~~~
biocLite("DESeq2")
~~~
{: .source}

For this lesson, we will be following a
 modified version of the DESeq2 vignette. The original vignette and reference
 manual have much more information.

 > ## Finding Reference Manuals and Vignettes
 >
 > As we saw earlier, the Reference Manual and Vignette for DESeq2 are available
on the
[biocViews page](https://bioconductor.org/packages/release/bioc/html/DESeq2.html).
 >
 > You can also find the Vignette by typing the following command in RStudio:
 `browseVignettes("DESeq2")`
 > This method is especially useful because it ensures that you are looking at
 the vignette that corresponds to the version of DESeq2 installed on your
 computer.
 {: .challenge}

### Input

To detect differentially expressed genes, you must first have a list of gene
counts.
...

### The DESeqDataSet

DESeq2 stores read counts and information from the statistical analysis in a
*DESeqDataSet* object.
...

### Pre-filtering

To use less memory and increase speed, we will pre-filter our dataset, to remove
genes with very low expression. Run the following commands to keep only the
genes that have at least 10 reads total:

~~~
keep <- rowSums(counts(dds)) >= 10
dds <- dds[keep,]
~~~
{: .source}

### Set your control

The following command will explicitly set the "untreated" group as the reference
sample:

~~~
dds$condition <- relevel(dds$condition, ref = "untreated")
~~~
{: .source}

### Differential Expression analysis

Now that we have set up the experiment, we can do the statistical analysis to
determine which genes are differentially expressed:

~~~
dds <- DESeq(dds)
res <- results(dds)
res
~~~
{: .source}

Let's get a summary of our results:

~~~
summary(res)
~~~
{: .source}

### Exploring and Exporting results

DESeq2 also has some built-in plotting functions. For example:

~~~
plotMA(res, ylim=c(-2,2))
~~~
{: .source}

### Exporting results to CSV format

In order to share your results, you might want to export them to a file that
can be opened as a spreadsheet.

~~~
resOrdered <- res[order(res$pvalue),]
write.csv(as.data.frame(resOrdered),
          file="condition_treated_results.csv")
~~~
{: .source}
