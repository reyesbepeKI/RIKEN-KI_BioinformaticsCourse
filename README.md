# RIKEN-KI_BioinformaticsCourse

## Task 1 - Literature

### 1. Read the research article of the hands-on working group you are assigned to (see file “Student Groups.pdf” in shared folder General course material).

Done!

---
### 2. Answer the following questions:

#### a. What is the medically relevant insight from the article?

Insulin resistance in human subcutaneous fat is selective, not global. In women with obesity, some insulin-driven pathways stay intact, others are lost, and most of the lost ones return after weight loss.

The study compared 23 women with obesity (OB) before and 2 years after gastric bypass surgery (post-obese, POB) with 23 never-obese (NO) controls. Insulin response was measured by hyperinsulinemic euglycemic clamp, with fat biopsies taken before and at the end of the clamp.

#### b. Which genomics technology/ technologies were used?

* CAGE (5′ cap analysis of gene expression): The core technology. It sequences the 5′ ends of capped RNAs on Illumina HiSeq 2000/2500 to map transcription start sites. Reads were mapped with Bowtie to hg19, with an S. pombe cross-mapping step to get uniquely mapped human reads. CTSSs were clustered into 54,527 tag clusters (TCs) with Paraclu, and 29,189 TCs passed filtering. These were assigned to 15,056 genes with GENCODE v19 and FANTOM5 lncRNA annotations. A gene can have several TCs, which reflects multiple promoters.
* Differential expression: edgeR generalized linear models, FDR < 0.05.
* Gene set enrichment: clusterProfiler, using a signed −log10 P score per gene.
* TF binding site enrichment: UniBind ChIP-seq-derived TFBS sets, tested with LOLA (Fisher exact test) on ±500 bp regions around differentially expressed TCs, with liftOver to hg38 and bedtools for region handling.
* Clinical phenotyping: hyperinsulinemic euglycemic clamp (M value) and HOMA-IR. These are physiological measures, not genomics.

---
### 3. Further related research questions

#### a. List and explain at least three questions/ hypotheses you can think of that extend the analysis presented in the paper.

* Do the transcriptional changes translate into protein and signaling changes?
Most TF genes were not themselves insulin-regulated, so the TFBS signals (SREBP, LXR, and others) probably reflect post-transcriptional events. Hypothesis: in obesity, proximal Akt signaling is partly blunted, but mTORC1/SREBP signaling is preserved. Phosphoproteomics and proteomics of paired biopsies could test this, and would show where along the pathway the split occurs.
* Does the ribosomal protein downregulation affect translation?
Hypothesis: insulin-driven downregulation of RNP genes (via ETS/GABPA/YY1) selectively changes translation of specific mRNAs, and this is lost in obesity. Ribosome profiling (Ribo-seq) or polysome profiling on paired biopsies could test it.
* Does the selective resistance depend on cell type, and is it intrinsic to adipocytes?
Bulk WAT mixes adipocytes, stromal/vascular cells, endothelial cells and immune cells. Hypothesis: the lost remodeling/angiogenesis response is mainly endothelial or stromal, while lipogenic genes are adipocyte-driven. Single-nucleus RNA-seq of fasting and clamp biopsies, or isolated adipocytes versus stromal fractions, could resolve this. Cell composition changes, such as fewer macrophages after weight loss, could also confound the bulk results.

---
## Task 2 - Git repositories and R Markdown

Done! 

---
## Task 3 - Introduction to R and online R course

Done!

---
## Task 4 - R basic operations

### 1. What is the square root of 10?
```r
10^(1/2)
```
Result: 3.16

---
### 2. What is the logarithm of 32 to the base 2?
```r
log2(32)
```
Result: 5

---
### 3. What is the sum of the numbers from 1 to 1000?

```r
sum(1:1000)
```
Result: 500500

---
### 4. What is the sum of all even numbers from 2 to 1000?
```r
sum(seq(2, 1000, by = 2))
```
Result: 250500

---
### 5. How many pairwise comparisons are there for 100 genes?
```r
n <- 100
choose(n, 2)
```
Result: 4950

---
### 6. And how many ways to arrange 100 genes in triples?
```r
n <- 100
choose(n, 3)
```
Result: 161700

---
## Task 5 - Using R example datasets

### 1. Use the R internal CO2 dataset (“data(CO2)”).

```r
data(CO2)
```

---
### 2. Describe briefly the content of the CO2 dataset using the help function.
```r
help(CO2)
```

This dataset comes from an experiment on the cold tolerance of the grass species *Echinochloa crus-galli*. Researchers measured the CO2 uptake of twelve plants, six from Quebec and six from Mississippi, across a range of ambient CO2 concentrations. For each origin, half of the plants were chilled overnight before the measurements began.

The data frame contains 84 rows and 5 columns, with 7 measurements recorded for each plant. The columns are:

* **Plant:** the plant ID, which encodes origin, treatment, and plant number:
  - Qn1-Qn3: Quebec, nonchilled, plants 1 to 3
  - Qc1-Qc3: Quebec, chilled, plants 1 to 3
  - Mn1-Mn3: Mississippi, nonchilled, plants 1 to 3
  - Mc1-Mc3: Mississippi, chilled, plants 1 to 3
* **Type:** the plant's place of origin, either Quebec or Mississippi.
* **Treatment:** whether the plant was chilled or nonchilled.
* **conc:** the ambient CO2 concentration in mL/L. Every plant was measured at the same seven levels: 95, 175, 250, 350, 500, 675, and 1000.
* **uptake:** the CO2 uptake rate in μmol/m²·sec.

---
### 3. What is the average and median CO2 uptake of the plants from Quebec and Mississippi?
```r
Quebec <- subset(CO2, Type == "Quebec")
summary(Quebec$uptake)
```

|Median|Mean|
|-|-|
|37.15|33.54|

The average CO2 uptake of the plants from Quebec is 33.54, while the median is 37.15.

```r
Mississippi <- subset(CO2, Type == "Mississippi")
summary(Mississippi$uptake)
```
|Median|Mean|
|-|-|
|19.30|20.88|

The average CO2 uptake of the plants from Mississippi is 20.88, while the median is 19.30.

---
### 4. [Optional] In the “airway” example data from Bioconductor, how many genes are expressed in each sample? How many genes are not expressed in any sample?
```r
library(airway)
data(airway)
help(airway)
counts_mat <- assay(airway)

colSums(counts_mat > 0)
sum(rowSums(counts_mat) == 0)
```
Genes expressed in all samples:
|SRR1039508|SRR1039509|SRR1039512|SRR1039513|SRR1039516|SRR1039517|SRR1039520|SRR1039521|
|-|-|-|-|-|-|-|-|
|24633|24527|25699|23124|25508|25998|24662|23991|

Genes not expressed in any sample: 30208

---
## Task 6 - R Functions

### 1. Write a function that calculates the ratio of the mean and the median of a given vector.

```r
task_6_1 <- function(x) {
  mean(x) / median(x)
}
```

---
### 2. Write a function that ignores the lowest and the highest value from a given vector and calculate the mean.

```r
task_6_2 <- function(x) {
  x_sorted <- sort(x)
  mean(x_sorted[-c(1, length(x_sorted))])
}
```
This functions sorts the vector, and then calculates the mean based on indices 2 and up to the second largest value. 

