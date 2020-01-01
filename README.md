[![DOI](https://zenodo.org/badge/90571457.svg)](https://zenodo.org/badge/latestdoi/90571457) Latest public release

[![Build Status](https://travis-ci.org/HegemanLab/w4mclassfilter_galaxy_wrapper.svg?branch=master)](https://travis-ci.org/HegemanLab/w4mclassfilter_galaxy_wrapper) Current build status for master branch on GitHub

[Repository 'w4mclassfilter' in Galaxy Toolshed](https://toolshed.g2.bx.psu.edu/repository?repository_id=5f24951d82ab40fa)

# W4M Data Subset

#### A Galaxy tool to select a subset of Workflow4Metabolomics data

*W4M Data Subset* is [Galaxy tool-wrapper](https://docs.galaxyproject.org/en/latest/dev/schema.htm) to wrap the
[w4mclassfilter R package](https://github.com/HegemanLab/w4mclassfilter) for use with the
[Workflow4Metabolomics](http://workflow4metabolomics.org/) flavor of
[Galaxy](https://galaxyproject.org/).
This tool was developed and tested with [planemo](http://planemo.readthedocs.io/en/latest/).

#### Author

Arthur Eschenlauer (University of Minnesota, esch0041@umn.edu)

#### R package wrapped by this tool

The *w4mclassfilter* package is available from the Hegeman lab github repository [https://github.com/HegemanLab/w4mclassfilter/releases](https://github.com/HegemanLab/w4mclassfilter/releases).

#### Tool in Galaxy toolshed

The "w4mclassfilter" Galaxy tool, built from this repository, is in the main Galaxy toolshed at [https://toolshed.g2.bx.psu.edu/repository?repository_id=5f24951d82ab40fa](https://toolshed.g2.bx.psu.edu/repository?repository_id=5f24951d82ab40fa)

#### Tool updates

See the **NEWS** section at the bottom of this page

## Motivation

GC-MS and LC-MS experiments seek to resolve (as "features") chemicals that
have distinct chromatographic retention-time ("rt") and (after
ionization) mass-to-charge ratio ("m/z" or "mz"). (If a chemical is
fragmented or may have a variety of adducts, several features may result.)
Data for a sample are collected as MS intensities, each of
which is associated with a position on a 2D plane with dimensions of rt
and m/z. Ideally, features would be sufficiently reproducible among
sample-runs to distinguish features that are commmon among samples from
those that differ.

The Workflow4Metabolomics suite of Galaxy tools
(W4M, [Giacomoni *et al.*, 2014, Guitton *et al.* 2017])
uses the XCMS preprocessing tools for 
"retention-time correction" to
align features among samples. Features may be better aligned if pooled
samples and blanks are included.

Multivariate statistical tools may be used to discover clusters of
similar samples [Th&#233;venot *et al.*, 2015].
However, once retention-time alignment of features has been achieved
among samples in GC-MS and LC-MS datasets:

- The presence of pools and blanks may confound identification and separation of sample clusters.
- Multivariate statistical algorithms may be impacted by missing values or dimensions that have zero variance.


## Description

The W4M Data Subset tool selects subsets of samples, features, or data values and conditions the data for further analysis.

- The tool takes as input the data matrix, sample metadata, and variable metadata datasets produced by W4M\'s XCMS [Smith *et al.*, 2006] and CAMERA [Kuhl *et al.*, 2012] tools.
- The tool produces the same trio of output datasets, modified as described below.

This tool can perform several operations to reduce the number samples or features to be analyzed (although *this should be done only in a statistically sound manner* consistent with the nature of the experiment):

- Samples may be selected by designating a "sample class" column in sampleMetadata and specifying criteria to include or exclude samples based on the contents of this column.
- Features may be selected by specifying minimum or maximum value (or both) allowable in columns of variableMetadata.
- Features may be selected by "range of row-maximum for each feature", i.e., by specifying minimum or maximum intensity (or both) allowable in each row of the dataMatrix (i.e., for the feature across all samples).
- To exclude minimal features from consideration, features may be selected by "range of maximum intensity for each feature", i.e., by specifying the minimum permitted maximum intensity in each row of the dataMatrix (i.e., for the feature across all samples).

This tool also conditions data for downstream statistical analysis:

- Samples that are missing from either sampleMetadata or dataMatrix are eliminated.
- Features that are missing from either variableMetadata or dataMatrix are eliminated.
- Features and samples that have zero variance are eliminated.
- Samples and features are ordered consistently in variableMetadata, sampleMetadata, and dataMatrix. (The columns for sorting variableMetadata or sampleMetadata may be specified.)
- The names of the first columns of variableMetadata and sampleMetadata are set respectively to "variableMetadata" and "sampleMetadata".
- If desired, the values in the dataMatrix may be log-transformed.
- Negative intensities become missing values.
- If desired, each missing value in dataMatrix may be replaced with zero or the median value observed for the corresponding feature.
- If desired, a "center" for each treatment can be computed in lieu of the samples for that treatment.

This tool may be applied several times sequentially, which may be useful for:

- analyzing subsets of samples for progressively smaller sets of treatment-levels, or
- choosing subsets of samples or features, respectively based on criteria in columns of the sampleMetadata or variableMetadata tables.

## NEWS

### Changes in version 0.98.18

#### New features

* Enhancement: Added option "compute center for each treatment" [https://github.com/HegemanLab/w4mclassfilter/issues/6](https://github.com/HegemanLab/w4mclassfilter/issues/6).
* Enhancement: Added option "enable sorting on multiple columns of metadata" [https://github.com/HegemanLab/w4mclassfilter/issues/7](https://github.com/HegemanLab/w4mclassfilter/issues/7).
* Enhancement: Added option "always treat negative intensities as missing values" [https://github.com/HegemanLab/w4mclassfilter\_galaxy\_wrapper/issues/7](https://github.com/HegemanLab/w4mclassfilter_galaxy_wrapper/issues/7).

#### Internal modifications

* Use v0.98.18 of the [w4mclassfilter bioconda package](https://bioconda.github.io/recipes/w4mclassfilter/README.html).


### (Version numbers 0.98.15-0.98.17 skipped)


### Changes in version 0.98.14

#### New features

* Enhancement [https://github.com/HegemanLab/w4mclassfilter\_galaxy\_wrapper/issues/6](https://github.com/HegemanLab/w4mclassfilter_galaxy_wrapper/issues/6) - "Provide sort options for features and samples".

#### Internal modifications

* Use v0.98.14 of the [w4mclassfilter bioconda package](https://bioconda.github.io/recipes/w4mclassfilter/README.html).

### Changes in version 0.98.13

#### New features

* Support enhancement https://github.com/HegemanLab/w4mclassfilter/issues/4 - "add and test no-imputation and centering-imputation functions":
  - Support no imputation.
  - Support imputating missing feature-intensities as median intensity for the corresponding feature.

#### Internal modifications

* Use v0.98.13 of the [w4mclassfilter bioconda package](https://bioconda.github.io/recipes/w4mclassfilter/README.html).


### (Version number 0.98.12 skipped)


### Changes in version 0.98.11

#### New features

* none

#### Internal modifications

* Use v0.98.8 of the [w4mclassfilter bioconda package](https://bioconda.github.io/recipes/w4mclassfilter/README.html).

### Changes in version 0.98.10

#### New features

* none

#### Internal modifications

* Quality-assurance improvements - Changes for IUC conformance and automated Planemo testing on Travis CI.
* Forbid hyphens in sample and variable names because R does not permit them.

### Changes in version 0.98.9

#### New features

* none

#### Internal modifications

* Added missing support for hyphen character in regular expressions

### Changes in version 0.98.8

#### New features

* The tool now appears in Galaxy with a new, more representative name: "W4M Data Subset". (Earlier versions of this tool appeared in Galaxy with the name "Sample Subset".)
* Option was added to log-transform data matrix values.
* Output datasets are named in conformance with the W4M convention of appending the name of each preprocessing tool to the input dataset name.
* Superflous "Column that names the sample" input parameter was eliminated.
* Some documentation was updated or clarified.

#### Internal modifications

* None

### Changes in version 0.98.7

#### New features

* First column of output variableMetadata (that has feature names) now is always named `variableMetadata`
* First column of output sampleMetadata now (that has sample names) is always named `sampleMetadata`

#### Internal modifications

* Now uses w4mclassfilter R package v0.98.7.

### Changes in version 0.98.6

#### New features

* Added support for filtering out features whose attributes fall outside specified ranges. For more detail, see "Variable-range filters" above.

#### Internal modifications

* Now uses w4mclassfilter R package v0.98.6.
* Now sorts sample names and feature names in output files because some statistical tools expect the same order in dataMatrix row and column names as in the corresponding metadata files.

### Changes in version 0.98.3

#### New features

* Improved reference-list.

#### Internal modifications

* Improved input handling.
* Now uses w4mclassfilter R package v0.98.3, although that version has no functional implications for this tool.

### Changes in version 0.98.1

#### New features

* First release - Wrap the w4mclassfilter R package that implements filtering of W4M data matrix, variable metadata, and sample metadata by class of sample.
* *dataMatrix* *is* modified by the tool, so it *does* appear as an output file
* *sampleMetadata* *is* modified by the tool, so it *does* appear as an output file
* *variableMetadata* *is* modified by the tool, so it *does* appear as an output file

#### Internal modifications

* none

## Citations

Benjamini, Yoav and Hochberg, Yosef (1995) **Controlling the False Discovery Rate: A Practical and Powerful Approach to Multiple Testing.** In *Journal of the royal statistical society. Series B (Methodological), 1 (57), pp. pp. 289-300.* [http://www.jstor.org/stable/2346101](http://www.jstor.org/stable/2346101)

Kuhl, Carsten and Tautenhahn, Ralf and B&#246;ttcher, Christoph and Larson, Tony R. and Neumann, Steffen (2011). **CAMERA: An Integrated Strategy for Compound Spectra Extraction and Annotation of Liquid Chromatography/Mass Spectrometry Data Sets.** In *Analytical Chemistry, 84 (1), pp. 283-289.* [doi:10.1021/ac202450g](http://dx.doi.org/10.1021/ac202450g)

Giacomoni, F. and Le Corguille, G. and Monsoor, M. and Landi, M. and Pericard, P. and Petera, M. and Duperier, C. and Tremblay-Franco, M. and Martin, J.-F. and Jacob, D. and *et al.* (2014). **Workflow4Metabolomics: a collaborative research infrastructure for computational metabolomics.** In *Bioinformatics, 31 (9), pp. 1493–1495.* [doi:10.1093/bioinformatics/btu813](http://dx.doi.org/10.1093/bioinformatics/btu813)

Guitton, Yann and Tremblay-Franco, Marie and Le Corguill&#233;, Gildas and Martin, Jean-François and P&#233;t&#233;ra, M&#233;lanie and Roger-Mele, Pierrick and Delabri&#232;re, Alexis and Goulitquer, Sophie and Monsoor, Misharl and Duperier, Christophe and *et al.* (2017). **Create, run, share, publish, and reference your LC–MS, FIA–MS, GC–MS, and NMR data analysis workflows with the Workflow4Metabolomics 3.0 Galaxy online infrastructure for metabolomics.** In *The International Journal of Biochemistry &amp; Cell Biology, pp. 89-101.* [doi:10.1016/j.biocel.2017.07.002](http://dx.doi.org/10.1016/j.biocel.2017.07.002)

Smith, Colin A. and Want, Elizabeth J. and O'Maille, Grace and Abagyan, Ruben and Siuzdak, Gary (2006). **XCMS: Processing Mass Spectrometry Data for Metabolite Profiling Using Nonlinear Peak Alignment, Matching, and Identification.** In *Analytical Chemistry, 78 (3), pp. 779–787.* [doi:10.1021/ac051437y](http://dx.doi.org/10.1021/ac051437y)

Th&#233;venot, Etienne A. and Roux, Aur&#233;lie and Xu, Ying and Ezan, Eric and Junot, Christophe (2015). **Analysis of the Human Adult Urinary Metabolome Variations with Age, Body Mass Index, and Gender by Implementing a Comprehensive Workflow for Univariate and OPLS Statistical Analyses.** In *Journal of Proteome Research, 14 (8), pp. 3322–3335.* [doi:10.1021/acs.jproteome.5b00354](http://dx.doi.org/10.1021/acs.jproteome.5b00354)

Yekutieli, Daniel and Benjamini, Yoav (2001) **The control of the false discovery rate in multiple testing under dependency.** In *The Annals of Statistics, 29 (4), pp. 1165-1188.* [doi:10.1214/aos/1013699998](http://dx.doi.org/10.1214/aos/1013699998)
