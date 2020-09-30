![link](/software/bin/report/logo.png)

### Rapid and comprehensive RNA-seq exploration and visualisation for unlimited differential datasets

<br>

# Table of contents
1. [Description](#Description)
2. [Pipeline overview](#Pipeline_overview)
3. [Example outputs](#Example_outputs)
4. [Download and first time setup](#Download_and_first_time_setup)
5. [Basic input files](#Basic_input_files)
6. [Quick start guide](#Quick_start_guide)
7. [Advanced guide](#Advanced_guide)
8. [Pathway analysis](#Pathway_analysis)
9. [Results](#Results)
10. [Default R settings](#Default_R_settings)
11. [FAQ](#FAQ)
12. [List of parameters](#List_of_parameters)
13. [Contact and citation](#Contact_and_citation)

<br>

# Description <a name="Description"></a>

<br>

Once bulk RNA-seq differential data has been processed, i.e. aligned and then expression and differential tables generated, there remains the essential but lengthy process where the biology is explored, visualised and interpreted. Typically culminating in final manuscript figures. Remarkably, in both academia and industry the favoured method of bioinformaticians for completing this downstream step remains a semi-manually coded command line and R based (or similar) analysis which is laborious in the extreme, often taking weeks and months to complete.

Searchlight2 is a bulk RNA-seq exploration, visualisation and interpretation pipeline, which aims to automate this downstream analysis stage in its entirety, which it does so exclusively. When used alongside any standard alignment and processing pipeline (e.g. Star2, Hisat2, Kallisto, DEseq2, EdgeR, etc.) bioinfromaticians can consistently complete new bulk RNA-seq projects using under three hours of labour only. I.e. from raw sequence data to final manuscript figures, including all steps in-between - such as analysis plan, statistical analysis, visualisation, interpretation, panel selection and plot tweaking. 

Searchlight2 is suitable for use with any differential bulk RNA-seq experiment regardless of organism, experimental design, sample number or complexity. Results are indistinguishable from a manual analysis. The novelty of Searchlight2 is not complexity or that it is conceptually very challenging. It is brute force and user friendly. Its strength and novelty lie in: (1) recognising the need for independent but overlapping workflows allowing users to tailor analysis to meet specific questions; (2) providing a fully comprehensive statistical and visual analysis on the global, pathway and single gene levels; (3) providing means for comprehensive and familiar downstream user modification of all plots via user friendly R scripts and a Shiny graphical user interface; (4) allowing users to modify the default behaviour and visuals to their own taste, via the R-snippet database; (5) providing reports; (6) by being fully automated.

Searchlight2 accepts typical RNA-seq downstream analysis inputs - such as a sample sheet, expression matrix and any number of differential expression tables.  Searchlight2 is designed to help bioinformaticians, RNA-seq service providers and bench scientists progress bulk RNA-seq research projects rapidly and with minimal effort, thus freeing up resources for further in-depth analysis or alternative analytical approaches


<br>

# Pipeline overview <a name="Pipeline_overview"></a>

<br>

From the outset it is important to note that Searchlight2 is not a processing pipeline as it does not perform alignment, count reads or calculate expression and
differential expression values. These stages must be completed prior to the use of Searchlight2. Any processing pipeline is suitable (FastP, Hisat2, Star2, Kallisto, Deseq2, EdgeR, etc.), so long as you have a matrix of expression values (TPM, RPKM, Rlog, etc) and at least one differential expression table (DEseq2, EdgeR, etc) you may use Searchlight2. 

Searchlight2 is executed as a single command. Firstly, it validates the input files and combines them into a single “master gene table”, from which the downstream analysis is based. Next, it iterates through each workflow, generating: intermediate files; statistical analysis result files; per plot and per workflow R scripts, plots; a report in HMTL; and finally a Shiny app.

![link](/media/outline.png)

<br>

# Example outputs <a name="Example_outputs"></a>

<br>

**A screenshot of the report format**. [An example of the report is given here](https://github.com/Searchlight2/example-reports).

![link](/media/report.png)

<br>

**A screenshot of the Shiny app**. 

![link](/media/shiny.png)

<br>

**Example outputs from a dataset exploring the effect of TGFB1 on primary cardiac fibroblasts.** This dataset has two sample groups, control and cells treated with TGFB1. The analysis, interpretation and figure generation was completed by a bioinformatician using 44 minutes and 30 seconds of work from a starting point of raw counts. 

![link](/media/Ex1.png)

<br>

**Example outputs from a dataset exploring the effect of parkin mediated mitochondrial depletion in senescent MRC5 fibroblasts.** This dataset has three sample groups, proliferating (Prolif), senescent(Senes) and mitochondria depleted senescent (Senes MtD). The analysis, interpretation and figure generation was completed by a bioinformatician using 2 hours, 2 minutes and 43 seconds of work from a starting point of fastQ files. 

![link](/media/Ex2.png)

<br>

**Example outputs from a dataset exploring the synergistic effects of using a combination of RITA and CPI-203 on Chronic myeloid leukaemia (CML) haemopoietic stem cell (HSC) survival.** This dataset has four sample groups, Control, RITA, CPI and RITA plus CPI (Combo). The analysis, interpretation and figure generation was completed by a bioinformatician using 2 hours, 37 minutes and 11 seconds of work from a starting point of fastQ files. 

![link](/media/Ex3.png)

<br>

# Download and first time setup <a name="Download_and_first_time_setup"></a>

<br>

Searchlight2 can be downloaded from this Github page. By clicking the green "Code" button near the top right of the page. Then selecting Download Zip. 

* Once downloaded place the zip file into a folder of choice and unzip
* No further installation is required for the software
* Next, you will need to install Python (2.7) and the libraries Scipy and Numpy. [Here is an online guide](https://wiki.python.org/moin/BeginnersGuide/Download)
* Next, you will need to install R. We recommend doing so via RStudio (choose the free version). [Here is an online guide](https://rstudio.com/products/rstudio/download/)
* Finally, you will need to install several widely used R-packages: ggplot2, reshape, amap, grid, gridExtra, gtable, ggally, network, sna [Here is an online guide](https://www.datacamp.com/community/tutorials/r-packages-guide)
* If you also want to use the Shiny app feature you will need to install these additional R packages: shiny, shinyFiles, fs, shinycssloaders, graphics, dplyr

If you do not pre-install the R libraries Searchlight2 will run successfully and produce plot R code and reports, however it won't be able to generate the actual images.

<br>

# Basic input files <a name="Basic_input_files"></a>


<br>

Searchlight2 is strict about the format of its inputs (but not the source) to ensure that analysis is correct. Setting up the input files is the most fiddly step but only takes a few minutes. **All input files for Searchlight2 must be tab delimited.**

* Expression Matrix (EM). Any standard matrix of expression values (TPM, RPKM, Rlog, etc). With genes by row and samples by column. The first column should be the gene ID (Ensembl, Refseq, etc). There must be a header row with the first cell as "ID" and the rest the sample names. Sample names can't start with a number and can only include numbers, letters and underscore (_). [Here is an example EM file.](https://raw.githubusercontent.com/Searchlight2/Searchlight2/master/example_data/normexp.tsv)


* Differential expression table(s) (DE). Any standard differential expression table (DESeq2, EdgeR, etc). With the genes by row and the columns trimmed down to include only: gene ID, log2 fold change, p-value and adjusted p-value (in this order). There must be a header row with the headers as exactly: "ID", "Log2Fold", "P", "P.Adj". Not case sensitive, ignoring the quotes. The ID type must be the same as the expression matrix. I.e you can't use ensembl IDs for the expression matrix and Refseq for the differential expression tables. Please supply one differential expression table per comparison. [Here is an example DE file.](https://raw.githubusercontent.com/Searchlight2/Searchlight2/master/example_data/ML_vs_LP.tsv)

* Sample sheet (SS). A standard tab delimited sample sheet listing each sample by name in the first column and the sample group it belongs to in the second. There must be a header row with the headers as exactly: "sample", "sample_group". Not case sensitive, ignoring the quotes. If you have several layers of sample groupings (such as cell type and also treatment and also age) you may include additional coulmns, under any header that you wish. Sample names and sample group names can't start with a number and can only include numbers, letters and underscore (_). [Here is an example SS file.](https://github.com/Searchlight2/Searchlight2/blob/master/example_data/sample_sheet.tsv)

* Background file (BG). A typical background annotation file for the organism and transcriptome build, listing all genes. We supply several of these with the software and they can easily be generated from Ensembls biomart. Genes should be in rows and specific annotation by column. The file must only have the annotations: Gene ID, Gene Symbol, Chromosome, Start position, Stop position and Biotype (type of gene). There must be a header row with the headers as exactly: "ID", "Symbol", "Chromosome", "Start", "Stop", "Biotype". Not case sensitive, ignoring the quotes. If you are unsure as to the gene symbol, just use the ID. If you are unsure at to the biotype simply put "gene" in every cell for that column. Please be aware, for every unique biotype (e.g. coding_gene, linc_RNA, etc) you enter Searchlgiht2 will perform and entire additional analysis (as well as asingle combined), which can be slow. For that reason we recommend simply to have all biotypes set as "gene" in you BG file, for your first few runs. [Here is an example BG file.](https://raw.githubusercontent.com/Searchlight2/Searchlight2/master/databases/background/human/Ensembl.GRCh37.p13.tsv)

<br>

# Quick start guide <a name="Quick_start_guide"></a>

<br>

To run Searchlight2, firstly ensure that you have correctly prepared your four basic input files as desribed [here](#Basic_input_files). The next (final) step is to set-up and run the command. Searchlight2 can be executed by navigating to the /Searchlight2/Software/ folder and running: 

<br>

```
python Searchlight2.py 
```

<br>

This will throw an error as you have not yet added the input parameters. Searchlight2 parameters have the format --parameter,sub-parameter=value. The key parameters are: 

<br>

```
--out path=my_desired_output_path
--bg file=path_to_my_background_file
--em file=path_to_my_expression_matrix_file
--ss file=path_to_my_sample_sheet_file
--de file=path_to_my_differential_expression_file,numerator=sample_group_1,denominator=sample_group_2
```

<br>

Mostly this is straightforward, except for the --de parameter which requires the additional sub-parameters: "numerator=" and denomintor=". Here you are expected to enter the names of the two sample groups that are being compared differentially. For example: if the DE file was of a comparison between WT and KO samples, we would enter numerator=KO,denominator=WT. The numerator should always be the sample group where a positive fold change in the DE file means an increase in expression. **It is very important to note** that the values entered into numerator= and denominator= (in this case KO & WT) must also be in the sample_group column of the sample sheet. With the same spelling. **Furthermore all paths must be complete from "root"** and not abbreviated from the working directory. **Importantly, do not put spaces between sub-parameters.**

<br>

To execute Searchlight2 using the provided sample dataset we might run the following. In this example we have simply placed Searchlight2 into a downloads folder, where we will also store the results. This will execute a comprehensive Searchlight2 analysis, producing statistical analysis, intermediate files, plots, reports, r-code and a Shiny app.

<br>

```
python Searchlight2.py --out path=/home/john/Downloads/results --bg file=/home/john/Downloads/Searchlight2/backgrounds/mouse/Ensembl.GRCm38.p6.tsv --em file=/home/john/Downloads/Searchlight2/sample_datasets/EM.tsv --ss file=/home/john/Downloads/Searchlight2/sample_datasets/SS.tsv --de file=/home/john/Downloads/Searchlight2/sample_datasets/DE_WT_vs_KO.tsv ,numerator=KO,denominator=WT
```

<br>

Broken down the command looks like this. 

<br>

```
python Searchlight2.py 
--out path=/home/john/Downloads/results 
--bg file=/home/john/Downloads/Searchlight2/backgrounds/mouse/Ensembl.GRCm38.p6.tsv 
--em file=/home/john/Downloads/Searchlight2/sample_datasets/EM.tsv 
--ss file=/home/john/Downloads/Searchlight2/sample_datasets/SS.tsv 
--de file=/home/john/Downloads/Searchlight2/sample_datasets/DE_WT_vs_KO.tsv,numerator=KO,denominator=WT
```

<br>


# Advanced guide <a name="Advanced_guide"></a>

# Pathway analysis <a name="Pathway_analysis"></a>

# Results <a name="Results"></a>

# Default R settings <a name="Default_R_settings"></a>

# List of parameters <a name="List_of_parameters"></a>

# FAQ <a name="FAQ"></a>

# Contact and citation <a name="Contact_and_citation"></a>

