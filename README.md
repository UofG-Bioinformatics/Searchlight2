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
7. [Including pathway analysis](#Including_pathway_analysis)
8. [Advanced guide](#Advanced_guide)
9. [Results](#Results)
10. [Default R settings](#Default_R_settings)
11. [Pathway database formats](#Pathway_database_formats)
12. [FAQ](#FAQ)
13. [List of parameters](#List_of_parameters)
14. [Contact and citation](#Contact_and_citation)

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

The default settings for deciding statistical significance are p.adj <= 0.05 and an absolute log2 fold change of >= 1 (i.e. at least 2 fold). These can be altered by using the log2fold= and p.adj= sub-parameters of the --de parameter. For example, re-running the sample command with the following would use a p.adj cut-off of 0.01 and no log2 fold cut-off:

<br>

```
--de file=/home/john/Downloads/Searchlight2/sample_datasets/DE_WT_vs_KO.tsv,numerator=KO,denominator=WT,p.adj=0.01,log2fold=0
```

<br>

# Including pathway analysis <a name="Including_pathway_analysis"></a>

<br>

In the results you may notice that it does not currently include a pathway analysis (over-representation analysis (ORA) or upstream regulator analysis (URA)). This is because we have yet to include an ORA or URA database (such as GEO or TRRUST). To include over-representation analysis add the --ora parameter to the command:

<br>

```
--ora file=/home/john/Downloads/Searchlight2/gene_set_databases/GO_bp_mouse.tsv,type=GO_bp
```

<br>

The file= sub-parameter should point to a valid gene-set database file (such as GO, string, KEGG, etc) in the GMT format. Several of these are included in the download (Searchlight2/gene_set_databases/). For details of the GMT format please see [pathway database formats](#Pathway_database_formats). The type= sub-parameter is simply the name that you wish the database to be known as in the results. This name cannot start with a number and must include only letters, numbers and underscore (_). 

<br>

To also include an upstream regulator analysis add the --ura parameter to the command:

<br>

```
--ura file=/home/john/Downloads/Searchlight2/upstream_regulator_databases/TRRUST_mouse.tsv,type=TRRUST
```

<br>

The sub-parameters are analogous to those of the ORA parameter. Note that the upstream regulator database must be in the TRRUST format - see [pathway database formats](#Pathway_database_formats).

<br>

**It is important to note** that you may include as many different ORA or URA databases in a single run of Searchlight2. TO do so simply include an extra ORA or URA parameter pointing to a different database file. E.g.

<br>

```
--ora file=/home/john/Downloads/Searchlight2/gene_set_databases/GO_bp_mouse.tsv,type=GO_bp
--ora file=/home/john/Downloads/Searchlight2/gene_set_databases/GO_cc_mouse.tsv,type=GO_cc
--ora file=/home/john/Downloads/Searchlight2/gene_set_databases/GO_mf_mouse.tsv,type=GO_mf
--ura file=/home/john/Downloads/Searchlight2/upstream_regulator_databases/TRRUST_mouse.tsv,type=TRRUST
--ura file=/home/john/Downloads/IPA/IPA_database.tsv,type=IPA
```

<br>


# Advanced guide <a name="Advanced_guide"></a>

<br>

**The advanced guide deals with what to do if you experiment has more than two types of sample, and so involves more than one differential expression comparison.** Searchlight2 fully supports these datasets, which we will refer to as "signature based". 

<br>

If you have more than one differential expression file (e.g. WT vs KO and KO vs KO_rescue) you may run both comparisons simultaneously simply by adding a second --pde parameter. The broken down command for the sample dataset migh look like this:

<br>

```
python Searchlight2.py 
--out path=/home/john/Downloads/results 
--bg file=/home/john/Downloads/Searchlight2/backgrounds/mouse/Ensembl.GRCm38.p6.tsv 
--em file=/home/john/Downloads/Searchlight2/sample_datasets/EM.tsv 
--ss file=/home/john/Downloads/Searchlight2/sample_datasets/SS.tsv 
--de file=/home/john/Downloads/Searchlight2/sample_datasets/DE_WT_vs_KO.tsv,numerator=KO,denominator=WT
--de file=/home/john/Downloads/Searchlight2/sample_datasets/DE_KO_rescue_vs_KO.tsv,numerator=KO_rescue,denominator=KO
```

<br>

In this scenario Searchlight2 will run a seperate PDE workflow for each, and will provide a seperate set of results for each (including a seperate report). **There is no upper limit to the number of differential comparisons that can be supplied this way**. 

<br>

So far, we have considered the sitation where we have several independant pairwise comparisons. E.g. n * (A vs B). However, often it is desirable to ask how several groups of samples compare to each other. For example knowing what changes between WT and KO or what changes between KO and KO + rescue is interesting, but it does not tell us the extent to which the rescue returns the KO phenotype to WT. For this we need to perform a (A vs B VS B vs C) comparison, known as a multiple differential expression comparsion (MDE). These are simple to perform using the --mpde parameter, but provide an extremely powerful signature based analysis of whole experiments. The --mde parameter has the following format:

<br>

```
python Searchlight2.py 
--out path=/home/john/Downloads/results 
--bg file=/home/john/Downloads/Searchlight2/backgrounds/mouse/Ensembl.GRCm38.p6.tsv 
--em file=/home/john/Downloads/Searchlight2/sample_datasets/EM.tsv 
--ss file=/home/john/Downloads/Searchlight2/sample_datasets/SS.tsv 
--de file=/home/john/Downloads/Searchlight2/sample_datasets/DE_WT_vs_KO.tsv,numerator=KO,denominator=WT
--de file=/home/john/Downloads/Searchlight2/sample_datasets/DE_KO_rescue_vs_KO.tsv,numerator=KO_rescue,denominator=KO
--mde name=rescue_effect,numerator=KO*denominator=WT,numerator=KO_rescue*denominator=KO
```

<br>

The --mde parameter has two sub-parameters. The first (name=) is simply a name for the workflow output folder. This as usual cannot start with a number and must include only letters, numbers and underscore. The second (numerator=group_1*denominator=group_2) is more complicated. The second sub-parameter should be included once for each differential comparison that you want to include in the MDE. In the above example there are two - numerator=KO*denominator=WT and numerator=KO_rescue*denominator=KO.


Put simply for each PDE you want to include in the MPDE you copy the "numerator=group_1,denominator=group_2" sub-parameters from PDE and replace the , with a *.


**It is very important to note** that there are few strict rules as to which DE's you include in a mde. I.e. there is no upper limit to the number of DE's that can be included in a single MDE parameter. 


The second sub-parameter is 




I.e. a single MPDE workflow can compare 2, 5, 10 or even a hundred different differential expression comparisons to each other simultaneously. Though we don't recommend going over 4 as the complexity becomes increasingly difficult to interpret. In situations where you have several 



In some situations we might wish to add additional sample groups to the results of a PDE analysis. For example 





# Results <a name="Results"></a>

# Default R settings <a name="Default_R_settings"></a>

# Pathway database formats <a name="Pathway_database_formats"></a>

# FAQ <a name="FAQ"></a>

# List of parameters <a name="List_of_parameters"></a>

# Contact and citation <a name="Contact_and_citation"></a>

