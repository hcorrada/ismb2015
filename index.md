---
title       :  Epiviz(r)
subtitle    : Turning a genome browser into a display device
author      : Hector Corrada Bravo
job         : Center for Bioinformatics and Computational Biology, University of Maryland
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [bootstrap]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Our motivation

Measuring DNA methylation and understanding role in expression regulation in solid tumors

![](images/block.png)

- Hansen, et al., *Nat. Genetics*, 2011
- Corrada Bravo, et al., *BMC Bioinformatics*, 2012
- Timp, et al., *Genome Medicine*, in press.

---

## Our motivation

Measuring DNA methylation and understanding role in expression regulation in solid tumors
<div class="centered">
<img src="images/antiprofile.png" style="max-height: 55%; max-width: 55%"/>
</div>

- Hansen, et al., *Nat. Genetics*, 2011
- Corrada Bravo, et al., *BMC Bioinformatics*, 2012
- Timp, et al., *Genome Medicine*, in press.

---

## Our motivation

Measuring DNA methylation and understanding role in expression regulation in solid tumors
<div class="centered">
<img src="images/barcode.png" style="max-height: 55%; max-width: 55%"/>
<img src="images/barcode2.png" style="max-height: 55%; max-width: 55%"/>
</div>

- Hansen, et al., *Nat. Genetics*, 2011
- Corrada Bravo, et al., *BMC Bioinformatics*, 2012
- Timp, et al., *Genome Medicine*, in press.

---

## Our motivation

Measuring DNA methylation and understanding role in expression regulation in solid tumors
<div class="centered">
<img src="images/minfi.png" style="max-height: 80%; max-width: 80%"/>
</div>

- Hansen, et al., *Nat. Genetics*, 2011
- Corrada Bravo, et al., *BMC Bioinformatics*, 2012
- Timp, et al., *Genome Medicine*, in press.

---

## What we wanted

> - Data transformation and modeling: data smoothing, region finding (`Bsmooth`, `minfi`)
> - Genome browsing: search by gene, search by overlap
> - Region analysis: overlap with other data (our own, other labs, UCSC, ensembl)
> - Regulation: expression data (Gene Expression Barcode)

--- 

## Analysis era 

> - Funding agencies calling for proposals to (strictly) *analyze* project data
>  - Epigenomics roadmap, Encode, TCGA, ...
> - Journals calling for (strictly) *analysis* papers (e.g., Nature Methods)
> - *We have unprecendented ability to measure*
> - *and lots of publicly available data to contextualize it*

--- 

## Analysis era 

- Funding agencies calling for proposals to (strictly) *analyze* project data
- Epigenomics roadmap, Encode, TCGA, ...
- Journals calling for (strictly) *analysis* papers (e.g., Nature Methods)
- *We have unprecendented ability to measure*
- *and lots of publicly available data to contextualize it*

<div class="centered">
<img src="images/hadley.png" style="max-height: 50%; max-width: 50%"/>
<img src="images/hadley2.png" style="max-height: 30%; max-width: 30%"/>
<footer class="source">[H. Wickham]</footer>
</div>

---

<div class="centered">
<img src="images/epiviz_2_logo_medium.png" style="max-height: 40%; max-width: 40%"/>
</div>

### Integrative, visual and computational exploratory analysis of genomic data

- Browser-based
- Interactive
- Integration of data
- Reproducible dissemination
- Communication with R/Bioc: `epivizr` package

> **I want to use a genome browser track as a display device in R!!**

[http://epiviz.cbcb.umd.edu](http://epiviz.cbcb.umd.edu)
<footer class="source">[Nat. methods, *in press*]</footer>
