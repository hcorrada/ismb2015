---
title       :  Interactive and exploratory visualization of epigenome-wide data
subtitle    : JSM2014, August 3, 2014
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

e.g.: http://epiviz.cbcb.umd.edu/?ws=45KBV4C7z3u
<footer class="source">[Nat. Methods, *in press*]</footer>

---

<iframe data-src="http://epiviz.cbcb.umd.edu/?ws=W3ieGz19icm" width="99%"></iframe>

---

## Communication with R/Bioc

Using the `epivizr` package

- Setup up an `epivizr` session

```r
library(epivizr)
data(tcga_colon_example)
mgr <- startEpiviz(workspace="qyOTB6vVnff")
```

- Add a device with `GRanges` data

```r
blocks_dev <- mgr$addDevice(colon_blocks, "450k blocks")
```

- Subset ranges by width

```r
keep <- width(colon_blocks) > 250000
mgr$updateDevice(blocks_dev, colon_blocks[keep,])
```

--- 

## Communication with R/Bioc

Using the `epivizr` package: browse by regions of interest.

- What's around the widest blocks?

```r
o <- order(-width(colon_blocks))
slideShowRegions <- colon_blocks[o[1:10],]
slideShowRegions <- slideShowRegions + 1e5
mgr$slideshow(slideShowRegions)
```

- Close session

```r
mgr$stopServer()
```

> `epivizr` uses WebSockets for connection, same as `shiny`. Big, big, big
> thanks to the @rstudio folks for working on this infrastructure.

---

## Plugins, plugins, plugins

This is how we integrate different data types and add new visualizations.

see: https://gist.github.com/11017650

```javascript
epiviz.plugins.charts.MyTrack.prototype.draw = function(range, data, slide, zoom) {
  epiviz.ui.charts.Track.prototype.draw.call(this, range, data, slide, zoom);

  // If data is defined, then the base class sets this._lastData to data.
  // If it isn't, then we'll use the data from the last draw call.
  // Same with this._lastRange and range.
  data = this._lastData;
  range = this._lastRange;

  // If data is not defined, there is nothing to draw
  if (!data || !range) { return []; }

  // Using D3, compute a function that maps base-pair locations to chart pixel coordinates
  var xScale = d3.scale.linear()
    .domain([range.start(), range.end()])
    .range([0, this.width() - this.margins().left() - this.margins().right()]);
```

---

http://epiviz.cbcb.umd.edu/?gist[]=11017650&ws=Y8kWxCO2Ajn
<iframe data-src="http://epiviz.cbcb.umd.edu/?gist[]=11017650&ws=Y8kWxCO2Ajn"></iframe>

---
## Plugins, plugins, plugins

see: https://gist.github.com/c41a2df3671395d8e4ad

```javascript
goog.provide('epiviz.plugins.data.UCSCDataProvider');

epiviz.plugins.data.UCSCDataProvider = function (id, endpoint) {
  epiviz.data.DataProvider.call(this, id || epiviz.plugins.data.UCSCDataProvider.DEFAULT_ID);
 
  this._endpoint = endpoint;
 
  this._refGene = new epiviz.measurements.Measurement(
    'refGene', // The column in the data source table that contains the values for this feature measurement
    'refGene', // A name not containing any special characters (only alphanumeric and underscores)
    epiviz.measurements.Measurement.Type.RANGE,
    'refGene', // Data source: the table/data frame containing the data
    'ucsc_refGene', // An identifier for use to group with other measurements from different data providers
    // that have the same seqName, start and end values
    this.id(), // Data provider
    null, // Formula: always null for measurements coming directly from the data provider
    'any', // Default chart type filter
```

---

http://epiviz.cbcb.umd.edu/?ws=cX1PgToUQs&seqName=chr11&start=59463945&end=60638081&gist[]=c41a2df3671395d8e4ad&settings=default&
<iframe data-src="http://epiviz.cbcb.umd.edu/?ws=cX1PgToUQs&seqName=chr11&start=59463945&end=60638081&gist[]=c41a2df3671395d8e4ad&settings=default&"></iframe>

---

## Plugins, plugins, plugins

<iframe data-src="http://epiviz.github.io"></iframe>

---

# Datatypes

<div class="centered">
<img src="images/datatypes.png" style="max-width=50%; min-width=50%"/>
</div>

- Based on "three-table" design
- Scripts can define coordinate space

---

http://epiviz.cbcb.umd.edu/?ws=SRHZlWRRAPd&gist[]=a82a998817564ce3fe48&settings=default&
<iframe data-src="http://epiviz.cbcb.umd.edu/?ws=SRHZlWRRAPd&gist[]=a82a998817564ce3fe48&settings=default&"></iframe>

--- 

## Build your own browser 

- Standalone version (no internet required, javascript code provided in `epivizr`)
- Browse your favorite genome:

```r
library(epivizr)
library(Mus.musculus)

mgr <- startStandalone(geneInfo=Mus.musculus, geneInfoName="mm10",
    				      keepSeqlevels=paste0("chr",c(1:19,"X","Y")))
```

---

## Analysis era

<div class="centered">
<img src="images/hadley.png" style="max-height: 50%; max-width: 50%"/>
<img src="images/hadley2.png" style="max-height: 30%; max-width: 30%"/>
<footer class="source">[H. Wickham]</footer>
</div>

One interpretation of *Big Data* is *Many relevant sources of contextual data*

- Easily access/integrate *contextual* data
- Driven by exploratory analysis of *immediate* data
- Iterative process
- Visual and computational exploration go hand in hand

---

## Creativity in exploration

We are building a software system to support creative exploratory analysis of genome-wide datasets...

<div class="centered">
<img src="images/terry.png" style="max-height: 60%; max-width: 60%"/>
<footer class="source">[T. Speed]</footer>

---

## Visualization goals

- Context 
  - Integrate and align multiple data sources; navigate; search
  - *Connect*: brushing
  - *Encode*: map visualization properties to data on the fly
  - *Reconfigure*: multiple views of the same data

<footer class="source">[Perer & Shneiderman]</footer>

---

## Visualization goals

- Data
  - *Select and filter*: tight-knit integration with R/Bioconductor; 
  - (future) filters on visualization propagate to data environment
- Model
    - New 'measurements' the result of modeling; perhaps suggested by data context

<footer class="source">[Perer & Shneiderman]</footer>

---

## Check it out:

- http://epiviz.github.io
- http://epiviz.cbcb.umd.edu
- http://github.com/epiviz

Nature Methods as of 2 hours ago...    
Follow us (brand new): @epiviz  

These slides available: http://hcorrada.github.io/jsm2014

---

## Acknowledgements

<img src="http://www.cs.umd.edu/~florinc/images/me.png"/>
Florin Chelaru, UMD

- CBCB@UMD
- JHU/Harvard: Kasper Hansen, Winston Timp, Rafael Irizarry, Andy Feinberg
- Genentech: Michael Lawrence
- Rstudio: Joe Cheng, et al.
- Funding: NIH, Genentech





