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

To detect differentially expressed genes, you must first have a list of read
counts per gene (or transcript). In an RNA-Seq experiment you might get a list
of counts from a program like [Salmon](https://combine-lab.github.io/salmon/) or [htseq-count](https://htseq.readthedocs.io/en/release_0.9.1/). In this example,
we'll start with a list of read counts that is included in [pasilla](http://bioconductor.org/packages/release/data/experiment/html/pasilla.html),
 a Bioconductor experiment package.

Start by installing and loading the pasilla package:

~~~
biocLite("pasilla")
library("pasilla")
~~~
{: .source}

We're interested in two files: the counts of RNA-Seq reads per gene and the
description of the samples.
~~~
pasCts <- system.file("extdata",
                      "pasilla_gene_counts.tsv",
                      package="pasilla", mustWork=TRUE)
pasAnno <- system.file("extdata",
                       "pasilla_sample_annotation.csv",
                       package="pasilla", mustWork=TRUE)
~~~
{: .source}

Now we'll read in these files:

~~~
cts <- as.matrix(read.csv(pasCts,sep="\t",row.names="gene_id"))
coldata <- read.csv(pasAnno, row.names=1)
~~~
{: .source}

Take a look at cts and coldata by clicking on them in the RStudio environment
panel. We will only need two columns of coldata, so run the following command:

~~~
coldata <- coldata[,c("condition","type")]
~~~
{: .source}

Also, we need to make sure that the sample names are consistent between cts and
coldata, and in the same order. First, get rid of the "fb" on the row names of
coldata:

~~~
rownames(coldata) <- sub("fb", "", rownames(coldata))
~~~
{: .source}

Now, put the columns of the cts matrix in the same order as in coldata:

~~~
cts <- cts[, rownames(coldata)]
~~~
{: .source}

Now, the data is ready to be analyzed with DESeq2.

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
