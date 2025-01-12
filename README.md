# Awesome Bioinformatics Benchmarks [![Build Status](https://travis-ci.org/j-andrews7/Awesome-Bioinformatics-Benchmarks.svg?branch=master)](https://travis-ci.org/j-andrews7/Awesome-Bioinformatics-Benchmarks)

A curated list of bioinformatics benchmarking papers and resources.

The credit for this format goes to Sean Davis for his [awesome-single-cell](https://github.com/seandavi/awesome-single-cell) repository and Ming Tang for his [ChIP-seq-analysis](https://github.com/crazyhottommy/ChIP-seq-analysis) repository. 

If you have a benchmarking study that is not yet included on this list, please make a [Pull Request](https://github.com/j-andrews7/Awesome-Bioinformatics-Benchmarks/pulls).

## Contents
   * [Rules for Included Papers](#rules-for-included-papers)
   * [Format &amp; Organization](#format--organization)
   * [Tool/Method Sections](#toolmethod-sections)
      * [DNase &amp; ChIP-seq](#dnase--chip-seq)
         * [Peak Callers](#peak-callers)
      * [RNA-seq](#rna-seq)
         * [Normalisation Methods](#normalisation-methods)
         * [Differential Gene Expression](#differential-gene-expression)
         * [Cell-Type Deconvolution](#cell-type-deconvolution)
      * [RNA/cDNA Microarrays](#rnacdna-microarrays)
      * [Variant Callers](#variant-callers)
         * [Germline SNP/Indel Callers](#germline-snpindel-callers)
         * [Somatic SNV/Indel callers](#somatic-snvindel-callers)
         * [CNV Callers](#cnv-callers)
         * [SV callers](#sv-callers)
      * [Single Cell](#single-cell)
         * [Trajectory Inference](#trajectory-inference)
         * [Integration/Batch Correction](#integrationbatch-correction)
         * [Cell Annotation/Inference](#cell-annotationinference)
   * [Contributors](#contributors)


## Rules for Included Papers
 - Papers must be objective comparisons of 3 or more tools/methods.
 - Papers should **not** be from authors showing why their tool/method is better than others.
 - Benchmarking data should be publicly available or simulation code/methods must be well-documented and reproducible.
 
Additional guidelines/rules may be added as necessary.

## Format & Organization
Please include the following information when adding papers.

**Title:**

**Authors:**

**Journal Info:**

**Description:**

**Tools/methods compared:**

**Recommendation(s):**

**Additional links (optional):**

Papers within each section should be ordered by publication date, with more recent papers listed first.

# Tool/Method Sections
Additional sections/sub-sections can be added as needed.


## DNase & ChIP-seq

### Peak Callers

**Title:** [Features that define the best ChIP-seq peak calling algorithms](https://academic.oup.com/bib/article/18/3/441/2453291)

**Authors:** Reuben Thomas, et al.

**Journal Info:** Briefings in Bioinformatics, May 2017

**Description:** This paper compared six peak calling methods on 300 simulated and three real ChIP-seq data sets across a range of significance values. 
Methods were scored by sensitivity, precision, and F-score.

**Tools/methods compared:** `GEM`, `MACS2`, `MUSIC`, `BCP`, `Threshold-based method (TM)`, `ZINBA`.

**Recommendation(s):** Varies. [BCP](http://ranger.sourceforge.net/manual1.18.html) and [MACS2](https://github.com/taoliu/MACS) performed the best across all metrics on the simulated data. 
For Tbx5 ChIP-seq, [GEM](http://groups.csail.mit.edu/cgs/gem/) performed the best, with BCP also scoring highly. 
For histone H3K36me3 and H3K4me3 data, all methods performed relatively comparably with the exception of ZINBA, which the authors could not get to run properly. 
[MUSIC](https://github.com/gersteinlab/MUSIC) and BCP had a slight edge over the others for the histone data. 

More generally, they found that methods that utilize variable window sizes and Poisson test to rank peaks are more powerful than those that use a Binomial test. 

---

**Title:** [A Comparison of Peak Callers Used for DNase-Seq Data](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0096303)

**Authors:** Hashem Koohy, et al.

**Journal Info:** PLoS ONE, May 2014

**Description:** This paper compares four peak callers specificity and sensitivity on DNase-seq data from two publications composed of three cell types, using ENCODE data for the same cell types as a benchmark. 
The authors tested multiple parameters for each caller to determine the best settings for DNase-seq data for each.

**Tools/methods compared:** `F-seq`, `Hotspot`, `MACS2`, `ZINBA`.

**Recommendation(s):** [F-seq](https://github.com/aboyle/F-seq) was the most sensitive, though [MACS2](https://github.com/taoliu/MACS) and [Hotspot](https://github.com/rthurman/hotspot) both performed competitively as well. 
ZINBA was the least performant by a massive margin, requiring much more time to run, and was also the least sensitive.

## RNA-seq

### Normalisation Methods

**Title:** [Statistical models for RNA-seq data derived from a two-condition 48-replicate experiment](https://academic.oup.com/bioinformatics/article/31/22/3625/240923)

**Authors:** Marek Gierliński\*, Christian Cole\*, Pietá Schofield\*, Nicholas J. Schurch\*, et al.

**Journal Info:** Bioinformatics, November 2015

**Description:** This paper compares the effect of normal, log-normal, and negative binomial distribution assumptions on RNA-seq gene read-counts from 48 RNA-seq replicates.

**Tools/methods compared:** `normal`, `log-normal`, `negative binomial`.

**Recommendation(s):** Assuming a normal distribution leads to a large number of false positives during differential gene expression. A log-normal distribution model works well unless a sample contains zero counts. Use tools that assume a negative binomial distribution (`edgeR`, `DESeq`, `DESeq2`, etc).

---

**Title:** [A comprehensive evaluation of normalization methods for Illumina high-throughput RNA sequencing data analysis](https://academic.oup.com/bib/article/14/6/671/189645). 

**Authors:** Marie-Agnès Dillies, et al.

**Journal Info:** Briefings in Bioinformatics, November 2013

**Description:** This paper compared seven RNA-seq normalization methods in the context of differential expression analysis on four real datasets and thousands of simulations.

**Tools/methods compared:** `Total Count (TC)`, `Upper Quartile (UQ)`, `Median (Med),` `DESeq`, `edgeR`, `Quantile (Q)`, `RPKM`.

**Recommendation(s):** The authors recommend [DESeq](https://bioconductor.org/packages/release/bioc/html/DESeq.html) ([DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html) now available as well) or [edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html), as those methods are robust to the presence of different library sizes and compositions, whereas the (still common) Total Count and RPKM methods are ineffective and should be abandoned.

### Differential Gene Expression

**Title:** [How well do RNA-Seq differential gene expression tools perform in a complex eukaryote? A case study in Arabidopsis thaliana.](https://www.ncbi.nlm.nih.gov/pubmed/30726870)

**Authors:** Kimon Froussios\*, Nick J Schurch\*, et al.

**Journal Info:** Bioinformatics, February 2019

**Description:** This paper compared nine differential gene expression tools (and their underlying model distributions) in 17 RNA-seq replicates of *Arabidopsis thaliana*.
Handling of inter-replicate variability and false positive fraction were the benchmarking metrics used.

**Tools/methods compared:** `baySeq`, `DEGseq`, `DESeq`, `DESeq2`, `EBSeq`, `edgeR`, `limma`, `Poisson-Seq`, `SAM-Seq`.

**Recommendation(s):** Six of the tools that utilize negative binomial or log-normal distributions ([edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html), [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html), [DESeq](https://bioconductor.org/packages/release/bioc/html/DESeq.html), [baySeq](https://bioconductor.org/packages/release/bioc/html/baySeq.html), [limma](https://bioconductor.org/packages/release/bioc/html/limma.html), and [EBseq](https://bioconductor.org/packages/release/bioc/html/EBSeq.html) control their identification of false positives well.

**Additional links (optional):** The authors released their benchmarking scripts on [Github](https://github.com/bartongroup/KF_arabidopsis-GRNA).

---

**Title:** [How many biological replicates are needed in an RNA-seq experiment and which differential expression tool should you use?](https://www.ncbi.nlm.nih.gov/pubmed/27022035)

**Authors:** Nicholas J. Schurch\*, Pietá Schofield\*, Marek Gierliński\*, Christian Cole\*, Alexander Sherstnev\*, et al. 

**Journal Info:** RNA, March 2016

**Description:** This paper compared 11 differential expression tools on varying numbers of RNA-seq biological replicates (3-42) between two conditions.
Each tool was compared against itself as a standard (using all replicates) and against the other tools.

**Tools/methods compared:** `baySeq`, `cuffdiff`, `DEGSeq`, `DESeq`, `DESeq2`, `EBSeq`, `edgeR (exact and glm modes)`, `limma`, `NOISeq`, `PoissonSeq`, `SAMSeq`. 

**Recommendation(s):** With fewer than 12 biological replicates, [edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html) and [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html) were the top performers. 
As replicates increased, [DESeq](https://bioconductor.org/packages/release/bioc/html/DESeq.html) did a better job minimizing false positives than other tools.

Additionally, the authors recommend at least six biological replicates should be used, rising to at least 12 if users want to identify all significantly differentially expressed genes no matter the fold change magnitude.

---

**Title:** [Comprehensive evaluation of differential gene expression analysis methods for RNA-seq data](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2013-14-9-r95)

**Authors:** Franck Rapaport, et al.

**Journal Info:** Genome Biology, September 2013

**Description:** This paper compared six differential expression methods on three cell line data sets from ENCODE (GM12878, H1-hESC, and MCF-7) and two samples from the SEQC study, which had a large fraction of differentially expressed genes validated by qRT-PCR.
Specificity, sensitivity, and false positive rate were the main benchmarking metrics used. 

**Tools/methods compared:** `Cuffdiff`, `edgeR`, `DESeq`, `PoissonSeq`, `baySeq`, `limma`.

**Recommendation(s):** Though no method emerged as favorable in all conditions, those that used negative binomial modeling ([DESeq](https://bioconductor.org/packages/release/bioc/html/DESeq.html), [edgeR](https://bioconductor.org/packages/release/bioc/html/edgeR.html), [baySeq](https://bioconductor.org/packages/release/bioc/html/baySeq.html)) generally performed best.

The more replicates, the better. Replicate numbers (both biological and technical) have a greater impact on differential detection accuracy than sequencing depth.


### Cell-Type Deconvolution

**Title:** [Comprehensive evaluation of transcriptome-based cell-type quantification methods for immuno-oncology](https://academic.oup.com/bioinformatics/article/35/14/i436/5529146)

**Authors:** Markus List\*, Tatsiana Aneichyk\*, et al.

**Journal Info:** Bioinformatics, July 2019

**Description:** This paper benchmarks and compares seven methods for computational deconvolution of cell-type abundance in bulk RNA-seq samples. Each method was tested on both simulated and true bulk RNA-seq samples validated by FACS.

**Tools/methods compared:** `quanTIseq`, `TIMER`, `CIBERSORT`, `CIBERSORT abs. mode`, `MCPCounter`, `xCell`, `EPIC`.

**Recommendation(s):** Varies. In general, the authors recommend [EPIC](https://gfellerlab.shinyapps.io/EPIC_1-1/) and [quanTIseq](http://icbi.at/software/quantiseq/doc/index.html) due to their overall robustness and absolute (rather than relative) scoring, though [xCell](http://xcell.ucsf.edu/) is recommended for binary presence/absence of cell types and [MCPcounter](https://github.com/ebecht/MCPcounter) was their recommended relative scoring method.

**Additional links:** The authors created an [R package called immunedeconv](https://github.com/icbi-lab/immunedeconv) for easy installation and use of all these methods. For developers, they have made available their [benchmarking pipeline](https://github.com/icbi-lab/immune_deconvolution_benchmark) so that others can reproduce/extend it to test their own tools/methods.

## RNA/cDNA Microarrays


## Variant Callers

### Germline SNP/Indel Callers

**Title:** [Systematic comparison of germline variant calling pipelines cross multiple next-generation sequencers](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6597787/)

**Authors:** Jiayun Chen, et al.

**Journal Info:** Scientific Reports, June 2019

**Description:** This paper compared three variant callers for WGS and WES samples from NA12878 across five next-gen sequencing platforms

**Tools/methods compared:** `GATK`, `Strelka2`, `Samtools-Varscan2`.

**Recommendation(s):** Though all methods tested generally scored well, [Strelka2](https://github.com/Illumina/strelka) had the highest F-scores for both SNP and indel calling in addition to being the most computationally performant.

---

**Title:** [Comparison of three variant callers for human whole genome sequencing](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6294778/) 

**Authors:** Anna Supernat, et al.

**Journal Info:** Scientific Reports, December 2018

**Description:** The paper compared three variant callers for WGS samples from NA12878 at 10x, 15x, and 30x coverage.

**Tools/methods compared:** `DeepVariant`, `GATK`, `SpeedSeq`.

**Recommendation(s):** All methods had similar F-scores, precision, and recall for SNP calling, but [DeepVariant](https://github.com/google/deepvariant) scored higher across all metrics for indels at all coverages.

---

**Title:** [A Comparison of Variant Calling Pipelines Using Genome in a Bottle as a Reference](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4619817/)

**Authors:** Adam Cornish, et al.

**Journal Info:** BioMed Research International, October 2015

**Description:** This paper compared 30 variant calling pipelines composed of six different variant callers and five different aligners on NA12878 WES data from the "Genome in a Bottle" consortium.

**Tools/methods compared:** 
 - Variant callers: `FreeBayes`, `GATK-HaplotypeCaller`, `GATK-UnifiedGenotyper`, `SAMtools mpileup`, `SNPSVM`
 - Aligners: `bowtie2`, `BWA-mem`, `BWA-sampe`, `CUSHAW3`, `MOSAIK`, `Novoalign`.

**Recommendation(s):** [Novoalign](http://www.novocraft.com/products/novoalign/) with [GATK-UnifiedGenotyper](https://software.broadinstitute.org/gatk/documentation/tooldocs/3.8-0/org_broadinstitute_gatk_tools_walkers_genotyper_UnifiedGenotyper.php) exhibited the highest sensitivity while producing few false positives. 
In general, [BWA-mem](https://github.com/lh3/bwa) was the most consistent aligner, and `GATK-UnifiedGenotyper` performed well across the top aligners (BWA, bowtie2, and Novoalign).


### Somatic SNV/Indel callers

**Title:** [Evaluation of Nine Somatic Variant Callers for Detection of Somatic Mutations in Exome and Targeted Deep Sequencing Data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4803342/)

**Authors:** Anne Bruun Krøigård, et al.

**Journal Info:** PLoS ONE, March 2016

**Description:** This paper performed comparisons between nine somatic variant callers on five paired tumor-normal samples from breast cancer patients subjected to WES and targeted deep sequencing.

**Tools/methods compared:** `EBCall`, `Mutect`, `Seurat`, `Shimmer`, `Indelocator`, `SomaticSniper`, `Strelka`, `VarScan2`, `Virmid`.

**Recommendation(s):** [EBCall](https://github.com/friend1ws/EBCall), [Mutect](https://github.com/broadinstitute/mutect), [Virmid](https://sourceforge.net/p/virmid/wiki/Home/), and [Strelka](https://github.com/Illumina/strelka) (now Strelka2) were most reliable for both WES and targeted deep sequencing. 
[EBCall](https://github.com/friend1ws/EBCall) was superior for indel calling due to high sensitivity and robustness to changes in sequencing depths.

---

**Title:** [Comparison of somatic mutation calling methods in amplicon and whole exome sequence data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3986649/)

**Authors:** Huilei Xu, et al.

**Journal Info:** BMC Genomics, March 2014

**Description:** Using the "Genome in a Bottle" gold standard variant set, this paper compared five somatic mutation calling methods on matched tumor-normal amplicon and WES data.

**Tools/methods compared:**  `GATK-UnifiedGenotyper followed by subtraction`, `MuTect`, `Strelka`, `SomaticSniper`, `VarScan2`.

**Recommendation(s):** [MuTect](https://github.com/broadinstitute/mutect) and [Strelka](https://github.com/Illumina/strelka) (now Strelka2) had the highest sensitivity, particularly at low frequency alleles, in addition to the highest specificity.  


### CNV Callers

**Title:** [Benchmark of tools for CNV detection from NGS panel data in a genetic diagnostics context](https://www.biorxiv.org/content/10.1101/850958v1)

**Authors:** José Marcos Moreno-Cabrera, et al.

**Journal Info:** bioRxiv, November 2019.

**Description:** This paper compared five germline copy number variation callers against four genetic diagnostics datasets (495 samples, 231 CNVs validated by MLPA) using both default and optimized parameters.
Sensitivity, specificity, positive predictive value, negative predictive value, F1 score, and various correlation coefficients were used as benchmarking metrics.

**Tools/methods compared:** `DECoN`, `CoNVaDING`, `panelcn.MOPS`, `ExomeDepth`, `CODEX2`.

**Recommendation(s):** Most tools performed well, but varied based on datasets. 
The authors felt [DECoN](https://www.imm.ox.ac.uk/research/units-and-centres/mrc-wimm-centre-for-computational-biology/groups/lunter-group/lunter-group/decon-detection-of-exon-copy-number) and [panelcn.MOPS](https://bioconductor.org/packages/release/bioc/html/panelcn.mops.html) with optimized parameters were sensitive enough to be used as screening methods in genetic dianostics.

**Additional links:** The authors have made their benchmarking code ([CNVbenchmarkeR](https://github.com/TranslationalBioinformaticsIGTP/CNVbenchmarkeR)) available, which can be run to determine optimal parameters for each algorithm for a given user's data.

---

**Title:** [An evaluation of copy number variation detection tools for cancer using whole exome sequencing data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5452530/)

**Authors:** Fatima Zare, et al.

**Journal Info:** BMC Bioinformatics, May 2017

**Description:** This paper compared six copy number variation callers on ten TCGA breast cancer tumor-normal pair WES datasets in addition to simulated datasets from VarSimLab.
Sensitivity, specificity, and false-discovery rate were used as the benchmarking metrics.

**Tools/methods compared:** `ADTEx`, `CONTRA`, `cn.MOPS`, `ExomeCNV`, `VarScan2`, `CoNVEX`.

**Recommendation(s):** All tools suffered from high FDRs (\~30-60%), but [ExomeCNV]https://github.com/cran/ExomeCNV) (a now defunct R package) had the highest overall sensitivity. 
[VarScan2](http://dkoboldt.github.io/varscan/) had moderate sensitivity and specificity for both amplifications and deletions.



### SV callers

**Title:** [Comprehensive evaluation and characterisation of short read general-purpose structural variant calling software](https://www.nature.com/articles/s41467-019-11146-4)

**Authors:** Daniel L. Cameron, et al.

**Journal Info:** Nature Communications, July 2019

**Description:** This paper compared 10 structural variant callers on four cell line WGS datasets (NA12878, HG002, CHM1, and CHM13) with orthogonal validation data.
Precision and recall were the benchmarking metrics used.

**Tools/methods compared:** `BreakDancer`, `cortex`, `CREST`, `DELLY`, `GRIDSS`, `Hydra`, `LUMPY`, `manta`, `Pindel`, `SOCRATES`.

**Recommendation(s):** The authors found [GRIDSS](https://github.com/PapenfussLab/gridss) and [manta](https://github.com/Illumina/manta) consistently performed well, but also provide more general guidelines for both users and developers.

 - Use a caller that utilizes multiple sources of evidence and assembly.
 - Use a caller that can call all events you care about.
 - Ensemble calling is not a cure-all and generally don't outperform the best individual callers (at least on these datasets).
 - Do not use callers that rely only on paired-end data.
 - Calls with high read counts are typically artefacts.
 - Simulations aren't real - benchmarking solely on simulations is a bad idea.
 - Developers - be wary of incomplete trust sets and the potential for overfitting. Test tools on multiple datasets.
 - Developers - make your tool easy to use with basic sanity checks to protect against invalid inputs. Use standard file formats.
 - Developers - use all available evidence and produce meaningful quality scores.
 
 ---
 
 **Title:** [Comprehensive evaluation of structural variation detection algorithms for whole genome sequencing](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1720-5)

**Authors:** Shunichi Kosugi, et al.

**Journal Info:** Genome Biology, June 2019

**Description:** This study compared 69 structural variation callers on simulated and real (NA12878, HG002, and HG00514) datasets.
F-scores, precision, and recall were the main benchmarking metrics. 

**Tools/methods compared:** `1-2-3-SV`, `AS-GENESENG`, `BASIL-ANISE,` `BatVI`, `BICseq2`, `BreakDancer`, `BreakSeek`, `BreakSeq2`, `Breakway`, `CLEVER`, `CNVnator`,
`Control-FREEC`, `CREST`, `DELLY`, `DINUMT`, `ERDS`, `FermiKit`, `forestSV`, `GASVPro`, `GenomeSTRiP`, `GRIDSS`, `HGT-ID`, `Hydra-sv`, `iCopyDAV`, `inGAP-sv`, `ITIS`,
`laSV`, `Lumpy`, `Manta`, `MATCHCLIP`, `Meerkat`, `MELT`, `MELT-numt`, `MetaSV`, `MindTheGap`, `Mobster`, `Mobster-numt`, `Mobster-vei`, `OncoSNP-SEQ`, `Pamir`, `PBHoney`,
`PBHoney-NGM`, `pbsv`, `PennCNV-Seq`, `Pindel`, `PopIns`, `PRISM`, `RAPTR`, `readDepth`, `RetroSeq`, `Sniffles`, `Socrates`, `SoftSearch`, `SoftSV`, `SoloDel`, `Sprites`,
`SvABA`, `SVDetect`, `Svelter`, `SVfinder`, `SVseq2`, `Tangram`, `Tangram-numt`, `Tangram-vei`, `Tea`, `TEMP`, `TIDDIT`, `Ulysses`, `VariationHunter`, `VirusFinder`, `VirusSeq`, `Wham`.

**Recommendation(s):** Varies greatly depending on type and size of the structural variant in addition to read length. 
`GRIDSS`, `Lumpy`, `SVseq2`, `SoftSV`, and `Manta` performed well calling deletions of diverse sizes.
`TIDDIT`, `forestSV`, `ERDS`, and `CNVnator` called large deletions well, while `pbsv`, `Sniffles`, and `PBHoney` were the best performers for small deletions.
For duplications, good choices included `Wham`, `SoftSV`, `MATCHCLIP`, and `GRIDSS`, while `CNVnator`, `ERDS`, and `iCopyDAV` excelled calling large duplications.
For insertions, `MELT`, `Mobster`, `inGAP-sv`, and methods using long read data were most effective.

## Single Cell

### Trajectory Inference

**Title:** [A comparison of single-cell trajectory inference methods](https://doi.org/10.1038/s41587-019-0071-9)

**Authors:** Wouter Saelens\*, Robrecht Cannoodt\*, et al.

**Journal Info:** Nat Biotech, April 2019

**Description:** A comprehensive evaluation of 45 trajectory inference methods, this paper provides an unmatched comparison of the rapidly evolving field of single-cell trajectory inference. Each method was scored on accuracy, scalability, stability, and usability. Should be considered a gold-standard for other benchmarking studies.

**Tools/methods compared:** `PAGA`, `RaceID/StemID`, `SLICER`, `Slingshot`, `PAGA Tree`, `MST`, `pCreode`, `SCUBA`, `Monocle DDRTree`, `Monocle ICA`, `cellTree maptpx`, `SLICE`, `cellTree VEM`, `EIPiGraph`, `Sincell`, `URD`, `CellTrails`, `Mpath`, `CellRouter`, `STEMNET`, `FateID`, `MFA`, `GPfates`, `DPT`, `Wishbone`, `SCORPIUS`, `Component 1`, `Embeddr`, `MATCHER`, `TSCAN`, `Wanderlust`, `PhenoPath`, `topslam`, `Waterfall`, `EIPiGraph linear`, `ouijaflow`, `FORKS`, `Angle`, `EIPiGraph cycle`, `reCAT`.

**Recommendation(s):** Varies depending on dataset and expected trajectory type, though [PAGA, PAGA Tree](https://scanpy.readthedocs.io/en/latest/examples.html#trajectory-inference), [SCORPIUS](https://github.com/rcannood/SCORPIUS), and [Slingshot](https://bioconductor.org/packages/release/bioc/html/slingshot.html) all scored highly across all metrics. 

Authors wrote an [interactive Shiny app](https://dynverse.org/users/3-user-guide/2-guidelines/) to help users choose the best methods for their data.

**Additional links:** The [dynverse site](https://dynverse.org/) contains numerous packages for users to run and compare results from different trajectory methods on their own data without installing each individually by using Docker. Additionally, they provide several tools for developers to wrap and benchmark their own method against those included in the study. 

### Integration/Batch Correction

### Cell Annotation/Inference

# Contributors

 - Jared Andrews ([@j-andrews7](https://github.com/j-andrews7/))
 - Kevin Blighe ([@kevinblighe](https://github.com/kevinblighe), [biostars](https://www.biostars.org/u/41557/))
 - Jeremy Leipzig ([@leipzig](https://github.com/leipzig))

