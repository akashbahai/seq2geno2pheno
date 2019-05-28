# Seq2Geno2Pheno

### What is Seq2Geno2Pheno?
This is a computational framework for genomic studies of bacterial population. Compared to conventional methods, such as shell scripts, this package emphasizes data reproducibility and computational environment management. 

### What can Seq2Geno2Pheno do?
Seq2Geno2Pheno includes two main stages. The first stage Seq2Geno aims to compute genomic features with sequencing reads. Briefly speaking, it covers (1) variant calling, (2) expression analysis, (3) _de novo_ assembly and gene content identification, and (4) phylogeny inference. These are followed by optional functionals of (5) differential expression analysis and (6) ancestral reconstruction. 
The outputs from Seq2Geno are formatted for the subsequent stage: Geno2Pheno. This stage mainly trains phenotype predictors with the genomic data. Furthermore, it reports lists of genomic factors that are potentially linked to the target phenotype using feature selection techniques.

### Get started
- Prerequisites

    - conda (tested version: 4.6.14)
    - python (tested verson: 3.6)
    - Linux (tested version: Debian GNU/Linux 8.8 jessie)
    - git (tested version: 2.18)

- Installation

    1. Download Seq2Geno2Pheno:

	`git clone --recurse-submodules https://github.com/hzi-bifo/seq2geno2pheno.git`

	The option `--recurse-submodules` helps to download the submodules that are located at another repository (i.e. Seq2Geno and Geno2Pheno). The flag is available only in git version >2.13, and users of earlier git version may need to find the substitute. 

    2. Install the core environment:

	Please check install/INSTALL to learn more details. 
	The core environment means the set of computational tools that are required to initiate Seq2Geno2Pheno. The environemnt, like those in the other parts of this package, is stated in a file that is recognizable by Conda, which makes the installation of everything achievable in one command. 

    3. Install the process-spcific environment:
	
	Many processes in the package require specific environments, which will be automatically installed when for the first time called by an scheduled process. Therefore, we encourage every user to run Seq2Geno2Pheno with our toy dataset or any other small dataset to install these environments and facillitate future usages.

    4. Finally, set the variable `PATH` and `SGP_HOME`. You may want to try

	```
	export SGP_HOME=/where/the/seq2geno2pheno/is/at
	export PATH=$SGP_HOME:$PATH
	```

	or inserting the above line in the `.profile` of your home directory, which will be more convenient for future usages.

### Usage and Input

```
usage: sgp [-h] [-v] -f CONFIG_F

Seq2Geno2Pheno: the pipline tool for genomic features computation and phenotype predictor training

optional arguments:
  -h, --help
    show this help message and exit
  -v
    show program's version number and exit
  -f CONFIG_F
    the yaml file where the arguments are listed
```

Example:
```
   sgp -f options.yml
```

The only input file (i.e. `options.yml`) is a yaml file where all options are described. The file consists of five main parts:

1. config_f: the input filename itself

2. review_args: boolean value to specify if only displaying the arguments

3. features: target computational processes

| option | action | values ([default])|
| --- | --- | --- |
| dryrun | display the processes and exit | [Y]/N |
| s | identify variants (SNPs) | Y/[N] |
| d | create _de novo_ assemblies and analyze gene contents | Y/[N] |
| e | count expression levels | Y/[N] |
| p | infer the phylogeny | Y/[N] |
| de | conduct analysis of differential expression | Y/[N] |
| ar | ancestrally reconstruct the expression levels | Y/[N] |

4. general: materials and resources

    - cores: available number of CPUs 

    - wd: the working directory
    The intermediate and final files will be created under the folder. The final outcomes will be symlinked to RESULTS/.

    - DNA-reads: The list of DNA-seq data 

    It should be a two-column list, where the first column includes all samples and the second column lists the __paired-end reads files__. The two reads file are separated by a comma. The first line is the first sample.
    ```
    sample01	sample01_1.fastq.gz,sample01_2.fastq.gz
    sample02	sample02_1.fastq.gz,sample02_2.fastq.gz
    sample03	sample03_1.fastq.gz,sample03_2.fastq.gz
    ```

    - RNA-reads: The list of RNA-seq data

    It should be a two-column list, where the first column includes all samples and the second column lists the __short reads files__. The first line is the first sample.
    ```
    sample01	sample01.rna.fastq.gz
    sample02	sample02.rna.fastq.gz
    sample03	sample03.rna.fastq.gz
    ```

    - pheno: The phenotype table

    For n samples with m phenotypes, the table is n-by-m and the upper-left is blank. The table is tab-separated. The header line includes the name of phenotypes. Missing values are allowed.
    ```
	    virulence
    sample01	high
    sample02	mediate
    sample03	low
    ```

    - ref-fa, ref-gff, ref-gbk	The reference data

    The fasta, gff, and genbank files of a reference genome. They should have the same sequence ids. 

    - adaptor: The adaptor file (optional)

    The fasta file of adaptors of DNA-seq. It is used to process the DNA-seq reads. 

5. prediction: parameters about machine learning

    - pred: the name of machine learning project

    - models: the machine learning algorithm

    - part: the method to partition samples

    - fold_n: the number of fold for cross-validation

    - optimize: the target metric to optimize

    - test_perc: the percentage of samples 
    It specifies the amount of sample that will be isolated from the whole set for independent testing. These samples will not be used to train the predictor but only evaluate the performance. 

    - k-mer: the k-mer size for encoding genome sequences

    - cls: classification labels
    The file specifies classification labels based on the phenotypes. It is a two-column tab-separated file, where the first column is the phenotypes and the second column includes their prediction labels. 
    ```
    high	class_1
    mediate	class_1
    low	class_2
    ```

    - out: the prediction output folder

### License
Apache 2.0 (please refer to LICENSE)

### Contact
method 1. Open an issue in the repository
method 2. Send email to 
- Tzu-Hao Kuo (Tzu-Hao.Kuo@helmholtz-hzi.de) for problems about Seq2Geno or general questions
- Ehsaneddin Asgari (asgari@berkeley.edu) for problems about Geno2Pheno
#####Please remember to state how the problem can be reproduced and, if accessible, what solutions have been tried. 
