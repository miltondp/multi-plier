# MultiPLIER

### A unsupervised transfer learning approach for rare disease transcriptomics

**Taroni JN, Grayson PC, Hu Q, Eddy S, Kretzler M, Merkel PA, and Greene CS*. 2018.**

*Correspondence [via issues](https://github.com/greenelab/multi-plier/issues) or to [greenescientist@gmail.com](mailto:greenescientist@gmail.com)

**TODO:** preprint DOI, link to release that's on figshare

## Data

Data used in this analysis repo were processed in [greenelab/rheum-plier-data](https://github.com/greenelab/). 
Please see that repository for relevant citations.

The recount2 training data and the corresponding PLIER model (MultiPLIER) are too large to be stored with Git LFS, so we have placed them on [figshare](https://figshare.com/). **DOI: [10.6084/m9.figshare.5716033.v4](https://doi.org/10.6084/m9.figshare.5716033.v4)**

## Dependencies

We have prepared a [Docker](https://www.docker.com) image that contains all the dependencies required to reproduce these analyses. See [`docker/Dockerfile`](https://github.com/greenelab/multi-plier/blob/eb30c25e236ae8590d275f9d351f804bd922ca0a/docker/Dockerfile) for more information about dependencies. 

After installing Docker ([Docker documentation](https://docs.docker.com)), the image can be obtained:

```
docker pull jtaroni/multi-plier:0.1.0
```

## Overview

Unsupervised machine learning methods provide a promising means to analyze and interpret large datasets. 
However, most datasets generated by individual researchers remain too small to fully benefit from these methods. 
In the case of rare diseases, there may be too few cases available, even when multiple studies are combined. 
We sought to determine whether or not machine learning models could be constructed from large public data compendia and then transferred to small datasets for subsequent analysis. 
We trained models using **Pathway Level Information ExtractoR ([PLIER](https://doi.org/10.1101/116061))** ([Github](https://github.com/wgmao/PLIER)) over datasets of different types and scales. 
Models constructed from large public datasets were 
i) more detailed than those constructed from individual datasets; 
ii) included features that aligned well to important biological factors; 
iii) transferrable to rare disease datasets where the models describe biological processes related to disease severity more effectively than models trained within those datasets. 

We call this approach **MultiPLIER** because we train on multiple datasets, tissues, and biological conditions.

We focus on groups of systemic autoimmune conditions in this project; one group of conditions is rare and the other disease is not. 
First, we establish that PLIER is appropriate for use in a single tissue, multi-dataset compendium ([greenelab/rheum-plier-data/sle-wb](https://github.com/greenelab/rheum-plier-data/tree/master/sle-wb)) constructed from publicly available [systemic lupus erythematosus](https://ghr.nlm.nih.gov/condition/systemic-lupus-erythematosus) (SLE) whole blood (WB) _microarray_ data. 
We demonstrate that MultiPLIER, trained on the [recount2](https://jhubiostatistics.shinyapps.io/recount/) _RNA-seq_ compendium, performs similarly in capturing certain cell type-specific signals and captures additional pathway signals over an SLE WB model.
We also analyze expression data from 3 tissues from [anti-neutrophilic cytoplasmic antibodies (ANCA)-associated vasculitis](https://rarediseases.info.nih.gov/diseases/13011/anca-associated-vasculitis) (AAV), a family of rare diseases, with MultiPLIER.

![](https://github.com/greenelab/multi-plier/blob/master/diagrams/overview_figure.png)

**Overview figure of dataset-specific PLIER and MultiPLIER.** Boxes with solid colored fills represent inputs to the model. White boxes with colored outlines represent model output. 
**(A)** PLIER ([Mao et al., 2017](https://doi.org/10.1101/116061)) automatically extracts latent variables (LVs), shown as the matrix `B`, and their loadings (`Z`). 
We can train PLIER model for each of three datasets from different tissues, which results in three dataset-specific latent spaces. 
**(B)** PLIER takes as input a prior information/knowledge matrix `C` and applies a constraint such that some of the loadings (`Z`) and therefore some of the LVs capture biological signal in the form of curated pathways or cell type-specific gene sets. 
**(C)** Ideally, an LV will map to a single gene set or a group of highly related gene sets to allow for easy interpretation of the model. 
PLIER applies a penalty on `U` to facilitate this. 
Purple fill in a cell indicates a non-zero value and a darker purple indicates a higher value. 
We show an undesirable `U` matrix in the top toy example **(Ci)** and a favorable `U` matrix in the bottom toy example **(Cii)**. **(D)** 
If models have been trained on individual datasets, we may be required to find “matching” LVs in different dataset- or tissue-specific models using the loadings (`Z`) from each model. 
Using a metric like the Pearson correlation between loadings, we may or may not be able to find a well-correlated match between datasets. 
**(E)** The MultiPLIER approach: train a PLIER on a large collection of uniformly processed data from many different biological contexts and conditions (recount2; [Collado-Torres et al., 2017](https://http//doi.org/10.1038/nbt.3838))—a MultiPLIER model—and then project the individual datasets into the MultiPLIER latent space. 
The hatched fill indicates the sample dataset of origin. 
**(F)** Latent variables from the MultiPLIER model can be tested for differential expression between disease and controls in multiple tissues.

For more information about the training set, please see [this notebook](https://greenelab.github.io/multi-plier/26-describe_recount2.nb.html).

### Notebooks

Analysis notebooks are numbered and present in the top level directory.
We've enabled [Github pages](https://pages.github.com/) for easy viewing of the notebooks.
Some steps in the pipeline are R scripts rather than notebooks due to their computationally intensive nature;
we exclude these from the TOC below.

* [PLIER functions proof of concept](https://greenelab.github.io/multi-plier/01-PLIER_util_proof-of-concept_notebook.nb.html)
* [Exploratory analysis of the recount2 PLIER model (MultiPLIER)](https://greenelab.github.io/multi-plier/02-recount2_PLIER_exploration.nb.html)
* [MultiPLIER on isolated immune cell populations microarray data](https://greenelab.github.io/multi-plier/03-isolated_cell_type_populations.nb.html)
* [Reconstruction of isolated immune cell data](https://greenelab.github.io/multi-plier/04-isolated_immune_cell_reconstruction.nb.html)
* [Training PLIER on the SLE whole blood compendium](https://greenelab.github.io/multi-plier/05-sle-wb_PLIER.nb.html)
* [Analyzing cell type-associated LVs in the SLE WB data with SLE WB PLIER model](https://greenelab.github.io/multi-plier/06-sle-wb_cell_type.nb.html)
* [Analyzing cell type patterns in the SLE WB data using MultiPLIER](https://greenelab.github.io/multi-plier/07-sle_cell_type_recount2_model.nb.html)
* [Identifying interferon-related LVs in the SLE WB and MultiPLIER models](https://greenelab.github.io/multi-plier/08-identify_ifn_LVs.nb.html)
* [Preparing IFN trials data for plotting](https://greenelab.github.io/multi-plier/09-sle_ifn_data_prep.nb.html)
* [Plotting IFN trial results](https://greenelab.github.io/multi-plier/10-sle_ifn_analysis.nb.html)
* [Training a PLIER model on the NARES nasal brushing microarray dataset](https://greenelab.github.io/multi-plier/12-train_NARES_PLIER.nb.html)
* [Comparing the NARES and MultiPLIER latent spaces](https://greenelab.github.io/multi-plier/13-compare_NARES_B.nb.html)
* [Comparing the MultiPLIER neutrophil-associated LV expression values to `MCPcounter` estimates](https://greenelab.github.io/multi-plier/14-NARES_MCPcounter.nb.html)
* [Evaluating PLIER models trained on subsampled recount2 compendia](https://greenelab.github.io/multi-plier/15-evaluate_subsampling.nb.html)
* [Plotting metrics for PLIER model repeats](https://greenelab.github.io/multi-plier/17-plotting_repeat_evals.nb.html)
* [Identifying differentially expressed MultiPLIER LVs (DELVs) in the NARES nasal brushing dataset](https://greenelab.github.io/multi-plier/18-NARES_differential_expression.nb.html)
* [Identifying DELVs in granulomatosis with polyangiitis (GPA) peripheral blood mononuclear cells (PBMCs)](https://greenelab.github.io/multi-plier/19-GPA_blood_differential_expression.nb.html)
* [Identifying DELVs in microdissected glomeruli cohort](https://greenelab.github.io/multi-plier/20-kidney_differential_expression.nb.html)
* [Identifying DELVs common across the 3 AAV tissues](https://greenelab.github.io/multi-plier/21-AAV_DLVE.nb.html)
* [ANCA antigens in the GPA PBMCs](https://greenelab.github.io/multi-plier/22-GPA_blood_top_LVs.nb.html)
* [Examining high weight genes in DELVs](https://greenelab.github.io/multi-plier/23-explore_AAV_recount_LVs.nb.html)
* [Exploring a rituximab (RTX) dataset](https://greenelab.github.io/multi-plier/24-explore_rtx.nb.html) (preliminary)
* [Predicting RTX response](https://greenelab.github.io/multi-plier/25-predict_response.nb.html) (very preliminary, see [#18](https://github.com/greenelab/multi-plier/pull/18))
* [Describing the recount2 training set with MetaSRA predictions](https://greenelab.github.io/multi-plier/26-describe_recount2.nb.html)

Note that not all analyses present in this repository are included in the preprint.

The [`figure_notebooks`](https://github.com/greenelab/multi-plier/tree/master/figure_notebooks) directory contains notebooks that were used specifically to generate figures suitable for publication ([`figure_notebooks/figures`](https://github.com/greenelab/multi-plier/tree/master/figure_notebooks/figures)).

## License 

This repository is dual licensed as **[BSD 3-Clause](https://github.com/greenelab/multi-plier/blob/master/LICENSE_BSD-3.md)** (source code) and **[CC0 1.0](https://github.com/greenelab/multi-plier/blob/master/LICENSE_CC0.md)** (figures, documentation, and our arrangement of the facts contained in the underlying data), with the following exceptions:

* recount2 data downloaded from [this figshare accession](https://doi.org/10.6084/m9.figshare.5716033.v4) is licensed **CC-BY**.
