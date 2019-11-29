![ONT_logo](/ONT_logo.png)
-----------------------------

Pipeline for differential gene expression (DGE) and differential transcript usage (DTU) analysis using long reads
==================================================================================================================

This pipeline uses [snakemake](https://snakemake.readthedocs.io/en/stable/), [minimap2](https://github.com/lh3/minimap2), [salmon](https://combine-lab.github.io/salmon/), [edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html), [DEXSeq](https://bioconductor.org/packages/release/bioc/html/DEXSeq.html) and [stageR](https://bioconductor.org/packages/release/bioc/html/stageR.html) to automate simple [differential gene expression](https://www.ebi.ac.uk/training/online/course/functional-genomics-ii-common-technologies-and-data-analysis-methods/differential-gene) and [differential transcript usage](http://dx.doi.org/10.12688/f1000research.15398.2) workflows on long read data.

This branch is meant to analyse paired samples (for example treated and untreated samples from the same individuals).

Getting Started
===============

## Input

The input files and parameters are specified in `config.yml`:

- `transcriptome` - the input transcriptome.
- `annotation` - the input annotation in GFF format.
- `control_samples` - a dictionary with control sample names and paths to the fastq files.
- `treated_samples` - a dictionary with treated sample names and paths to the fastq files.

## Output

- `alignments/*.bam` - unsorted transcriptome alignments (input to `salmon`).
- `alignments_sorted/*.bam` - sorted and indexed transcriptome alignments.
- `counts` - counts generated by `salmon`.
- `merged/all_counts.tsv` - the transcript count table including all samples.
- `merged/all_counts_filtered.tsv` - the transcript count table including all samples after filtering.
- `merged/all_gene_counts.tsv` - the gene count table including all samples.
- `de_analysis/coldata.tsv` - the condition table used to build model matrix.
- `de_analysis/de_params.tsv` - analysis parameters generated from `config.yml`.
- `de_analysis/results_dge.tsv` and `de_analysis/results_dge.pdf`- results of `edgeR` differential gene expression analysis.
- `de_analysis/results_dtu_gene.tsv`, `de_analysis/results_dtu_transcript.tsv` and `de_analysis/results_dtu.pdf` - results of differential transcript usage by `DEXSeq`.
- `de_analysis/results_dtu_stageR.tsv` - results of the `stageR` analysis of the `DEXSeq` output.
- `de_analysis/dtu_plots.pdf` - DTU results plot based on the `stageR` results and filtered counts.


## Dependencies

- [miniconda](https://conda.io/miniconda.html) - install it according to the [instructions](https://conda.io/docs/user-guide/install/index.html).
- [snakemake](https://anaconda.org/bioconda/snakemake) install using `conda`.
- [pandas](https://anaconda.org/conda-forge/pandas) - install using `conda`.
- The rest of the dependencies are automatically installed using the `conda` feature of `snakemake`.

## Layout

* `README.md`
* `Snakefile`         - master snakefile
* `config.yml`        - YAML configuration file
* `snakelib/`         - snakefiles collection included by the master snakefile
* `lib/`              - python files included by analysis scripts and snakefiles
* `scripts/`          - analysis scripts
* `data/`             - input data needed by pipeline - use with caution to avoid bloated repo
* `results/`          - pipeline results to be commited - use with caution to avoid bloated repo

## Installation

Clone the repository:

```bash
git clone -b paired_dge_dtu https://github.com/nanoporetech/pipeline-transcriptome-de.git
```

## Usage:

Edit `config.yml` to set the input datasets and parameters then issue:

```bash
snakemake --use-conda -j <num_cores> all
```

Help
====

## Licence and Copyright

(c) 2018 Oxford Nanopore Technologies Ltd.

This Source Code Form is subject to the terms of the Mozilla Public
License, v. 2.0. If a copy of the MPL was not distributed with this
file, You can obtain one at http://mozilla.org/MPL/2.0/.

## FAQs and tips

## References and Supporting Information

This pipeline is largely based on the approach described in the following paper:

- Love MI, Soneson C and Patro R. *Swimming downstream: statistical analysis of differential transcript usage following Salmon quantification.* F1000Research 2018, 7:952
(doi: [10.12688/f1000research.15398.3](http://dx.doi.org/10.12688/f1000research.15398.3))


See the post announcing the tool at the Oxford Nanopore Technologies community [here](https://community.nanoporetech.com/posts/new-transcriptomics-analys).
