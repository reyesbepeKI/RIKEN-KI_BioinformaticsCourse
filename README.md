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

---
### 3. Read about piping from here:https://r4ds.had.co.nz/pipes.html#pipes (you don’t have to learn everything, a basic understanding of the usage is enough). Write a short (max. 300 characters, no spaces) explanation of why, how, and when not to use pipes.

Answer: Pipes are a tool that can be used when you want to do a sequence of multiple operations, and you want to write it easy to read and follow. In R pipes look like this: %>%.

When not to use pipes:
- If what you would want to do would need a pipe that is very long.
- If you have more than one input or output. 
- When you want to create complex relationships - pipes are linear. 

---
### 4. Familiarize yourself with the apply-family of functions (apply, lapply, sapply etc.) http://uc-r.github.io/apply_family Write a short explanation (max. 300 characters, no spaces) of why they could be useful in your work.

Answer: The apply-functions (e.g. apply for matrices and data frames, lapply for lists, sapply for a simplified output etc.) works similarly to loops in that they can loop over every element in e.g. a list and do the set function for every element - but you can write the code in one line that is easy to read. 

Example of my usage:
```r
microarray_imputed <- as.data.frame(
  lapply(microarray, function(x) {
    x[is.na(x)] <- mean(x, na.rm = TRUE)
    x
  })
)
```
---
## Task 7 - Basic visualization with R

### 1. Compare the distributions of the body heights of the two species from the 'magic_guys.csv' dataset graphically

```r
magic_guys <- read.csv(file.choose())
install.packages("ggplotify")
                 
library(ggplotify)
library(tidyverse)
library(patchwork)
```

#### a. using the basic 'hist' function as well as ‘ggplot’ and ‘geom_histogram’ functions from the ggplot2 package. Optimize the plots for example by trying several different 'breaks'. Note that ggplot2-based functions give you many more options for changing the visualization parameters, try some of them.

Basic 'hist' function:
```r
par(mfrow = c(1, 2))

art1_data <- subset(magic_guys, species == "jedi")
art2_data <- subset(magic_guys, species == "sith")

hist1 <- hist(art1_data$length, main = "Jedi", xlab = "Length", col = "skyblue")
hist2 <- hist(art2_data$length, main = "Sith", xlab = "Length", col = "salmon")

par(mfrow = c(1, 1))
```
<img width="4032" height="625" alt="image" src="figures/jedi_sith_histogram.svg" />


ggplot2 version:
```r
gg_hist <- ggplot(magic_guys, aes(x=length, fill=species)) + 
  geom_histogram(alpha=0.5, position="identity", color="black")+
  facet_wrap(~ species) +
  labs(title="Distribution of length per species", x="Length", y="Frequency")+
  scale_fill_manual(values = c("jedi" = "skyblue", "sith" = "salmon")) +
  theme_minimal()
gg_hist
```
<img width="4032" height="625" alt="image" src="figures/ggplot2_histogram.svg" />

---
#### b. Do the same comparison as in a. but with boxplots. If you want to use the ggplot2-package, use the functions ‘ggplot’ and ‘geom_boxplot’.

Basic boxplot function:
```r
basic_box <- boxplot(length ~ species, data = magic_guys, 
        main = "Length per Species", xlab = "Species", ylab = "Length",
        col = c("skyblue", "salmon"))
```
<img width="4032" height="625" alt="image" src="figures/jedi_sith_boxplot.svg" />


ggplot2 version:
```r
gg_box <- ggplot(magic_guys, aes(x=species, y=length, fill=species)) + 
  geom_boxplot(alpha=0.5, color="black")+
  labs(title="Distribution of length per species", x="Species", y="Length")+
  scale_fill_manual(values = c("jedi" = "skyblue", "sith" = "salmon")) +
  theme_minimal()
gg_box
```
<img width="4032" height="625" alt="image" src="figures/ggplot2_boxplot.svg" />

---
#### c. Save the plots with the ‘png’, ‘pdf’, and ‘svg’ formats. In which situation would you use which file format?

Save plot from basic 'hist' function:
```r
par(mfrow = c(1, 2))

art1_data <- subset(magic_guys, species == "jedi")
art2_data <- subset(magic_guys, species == "sith")

hist1 <- as.ggplot (function() hist(art1_data$length, main = "Jedi", xlab = "Length", col = "skyblue"))
hist2 <- as.ggplot (function() hist(art2_data$length, main = "Sith", xlab = "Length", col = "salmon"))

comb_hist <- hist1 + hist2

par(mfrow = c(1, 1))

ggsave("jedi_sith_histogram.png", plot = comb_hist, width = 10, height = 5)
ggsave("jedi_sith_histogram.pdf", plot = comb_hist, width = 10, height = 5)
ggsave("jedi_sith_histogram.svg", plot = comb_hist, width = 10, height = 5)
```

Save ggplot2 histogram:
```r
ggsave("ggplot2_histogram.png", plot = gg_hist, width = 10, height = 5)
ggsave("ggplot2_histogram.pdf", plot = gg_hist, width = 10, height = 5)
ggsave("ggplot2_histogram.svg", plot = gg_hist, width = 10, height = 5)
```

Save plot from basic boxplot function:
```r
png("jedi_sith_boxplot.png", width = 1000, height = 500)

boxplot(length ~ species,
        data = magic_guys,
        main = "Length per Species",
        xlab = "Species",
        ylab = "Length",
        col = c("skyblue", "salmon"))

dev.off()


pdf("jedi_sith_boxplot.pdf")

boxplot(length ~ species,
        data = magic_guys,
        main = "Length per Species",
        xlab = "Species",
        ylab = "Length",
        col = c("skyblue", "salmon"))

dev.off()


install.packages("svglite")
library(svglite)

svglite("jedi_sith_boxplot.svg")

boxplot(length ~ species,
        data = magic_guys,
        main = "Length per Species",
        xlab = "Species",
        ylab = "Length",
        col = c("skyblue", "salmon"))

dev.off()
```

Save ggplot2 boxplot:
```r
ggsave("ggplot2_boxplot.png", plot = gg_box, width = 10, height = 5)
ggsave("ggplot2_boxplot.pdf", plot = gg_box, width = 10, height = 5)
ggsave("ggplot2_boxplot.svg", plot = gg_box, width = 10, height = 5)
```

Answer: I would use svg when I want to create a high-quality photo for web use, while pdf is better for printing. Both are vector format. Png can be used for both web use and printing, but one has to ensure that it is of good quality (raster format).  


---
### 2. Load the gene expression data matrix from the ‘microarray_data.tab’ dataset provided in the shared folder, it is a big tabular separated matrix.

```r
microarray <- read_tsv(file.choose())
```

#### a. How big is the matrix in terms of rows and columns?

```r
dim(microarray)
```
Answer: There are 551 rows and 1000 columns (genes).

---
#### b. Count the missing values per gene and visualize this result.

```r
missing_per_gene <- colSums(is.na(microarray))
missing_per_gene

missing_df <- data.frame(
  Gene = names(missing_per_gene),
  Missing = missing_per_gene
)
missing_df

ggplot(missing_df, aes(x = Missing)) +
  geom_histogram(binwidth = 1) +
  labs(
    title = "Distribution of missing values per gene",
    x = "Number of missing values",
    y = "Number of genes"
  ) +
  theme_bw()
```
<img width="4032" height="625" alt="image" src="figures/missing_values.svg" />

---
#### c. Find the genes for which there are more than X% (X=10%, 20%, 50%) missing values.

X=10%:
```r
genes_over_10pct <- names(missing_per_gene[missing_per_gene > 0.10 * nrow(microarray)])
genes_over_10pct
```
Answer: There are 512 genes with more than 10% missing values. 

X=20%:
```r
genes_over_20pct <- names(missing_per_gene[missing_per_gene > 0.20 * nrow(microarray)])
genes_over_20pct
```
Answer: There are 377 genes with more than 20% missing values. 

X=50%:
```r
genes_over_50pct <- names(missing_per_gene[missing_per_gene > 0.50 * nrow(microarray)])
genes_over_50pct
```
Answer: There are 74 genes with more than 50% missing values. 

[1] "g18"  "g48"  "g55"  "g58"  "g60"  "g66"  "g73"  "g79"  "g83"  "g91"  "g93"  "g94"  "g99" 
[14] "g105" "g109" "g132" "g135" "g137" "g138" "g172" "g260" "g290" "g301" "g329" "g352" "g355"
[27] "g362" "g363" "g368" "g383" "g388" "g389" "g390" "g391" "g406" "g417" "g431" "g432" "g440"
[40] "g461" "g462" "g498" "g519" "g527" "g531" "g532" "g538" "g572" "g575" "g576" "g577" "g585"
[53] "g615" "g619" "g663" "g669" "g751" "g753" "g766" "g768" "g788" "g802" "g804" "g838" "g851"
[66] "g854" "g864" "g892" "g893" "g898" "g919" "g932" "g971" "g980"


Alternative method:
```r
percent_missing <- colSums(is.na(microarray)) / nrow(microarray) * 100
names(percent_missing[percent_missing > 10])
names(percent_missing[percent_missing > 20])
names(percent_missing[percent_missing > 50])
```

---
#### d. Replace the missing values by the average expression value for the particular gene. (Note: Imputing data has to be used with caution!)

```r
sum(is.na(microarray))
```
In total there are 115 826 NAs. To remove them I am using lapply to go over every value in a gene, calculate the mean and then replace the NAs with the mean for that specific gene. 

```r
microarray_imputed <- as.data.frame(
  lapply(microarray, function(x) {
    x[is.na(x)] <- mean(x, na.rm = TRUE)
    x
  })
)

sum(is.na(microarray_imputed))
```
Not all NAs were removed - 18734 remain. 

Finding error:
```r
which(colSums(!is.na(microarray)) == 0)
gene_means <- sapply(microarray, mean, na.rm = TRUE)
sum(is.nan(gene_means))
problem_genes <- names(gene_means[is.nan(gene_means)])
problem_genes
```
34 genes contained only missing values. These values are now removed before imputing. Below is the updated code to calculate the mean and replace the NAs with the mean for the specific gene the value belongs to, without the genes with only NA. 

```r
microarray_filtered <- microarray[, colSums(!is.na(microarray)) > 0]

microarray_imputed <- as.data.frame(
  lapply(microarray_filtered, function(x) {
    x[is.na(x)] <- mean(x, na.rm = TRUE)
    x
  })
)

sum(is.na(microarray_imputed))
```
Now, there are no NAs in the datafile. 

---
### 3. Visualize the data in the CO2 dataset in a way that gives you a deeper understanding of the data. What do you see?

```r
co2_box <- ggplot(CO2,
                  aes(x = Type,
                      y = uptake,
                      fill = Type)) +
  geom_boxplot() +
  scale_fill_manual(values = c("darkgreen", "orange")) +
  theme_bw()

co2_box
```
<img width="4032" height="625" alt="image" src="figures/Uptake_vs_type_boxplot.svg" />

```r
ggplot(CO2,
       aes(x = conc,
           y = uptake,
           color = Type,
           linetype = Treatment,
           group = interaction(Type, Treatment))) +
  stat_summary(fun = mean, geom = "line", linewidth = 1) +
  stat_summary(fun = mean, geom = "point", size = 2) +
  scale_color_manual(values = c(
    "Quebec" = "darkgreen",
    "Mississippi" = "orange")
  ) +
  labs(
    title = "Mean CO2 uptake across concentrations",
    x = "CO2 concentration",
    y = "Mean uptake"
  ) +
  theme_bw()
```
<img width="4032" height="625" alt="image" src="figures/Mean_CO2_uptake.svg" />

```r
ggplot(CO2,
       aes(x = conc,
           y = uptake,
           group = Plant,
           color = Treatment)) +
  geom_line(alpha = 0.7) +
  geom_point(alpha = 0.7) +
  facet_wrap(~ Type) +
  scale_color_manual(values = c(
    "chilled" = "steelblue",
    "nonchilled" = "firebrick"
  )) +
  labs(
    title = "CO2 uptake for individual plants",
    x = "CO2 concentration",
    y = "CO2 uptake"
  ) +
  theme_bw()
```
<img width="4032" height="625" alt="image" src="figures/CO2_uptake_ind_plants.svg" />


The Quebec plants have a higher general CO2 uptake than the Mississippi plants (seen from mean, median and boxplot). In the interaction plot, we see that again, but also that the mean uptake of the chilled plants are lower than that of the non-chilled plants. The difference is larger between chilled and non-chilled plants from Mississippi. In the interaction plot it also looks like the Mississippi plants reach a plateau in CO2 uptake with the higher concentrations. The Quebec plants does not seem as if they have completely reached the plateau just yet at the highest measured CO2 concentration. Looking at the trajectories of the individual plants, coloured based on treatment, we can see that while the Mississippi plants separate clearly based on treatment, the same effect is not as obvious in the plants from Quebec, with one non-chilled plant reaching less CO2 uptake than one-two chilled plants for the majority of the measurements. Individual variation has a bigger effect in the Quebec plants - or the plants from Quebec seemes to be better at withstanding the cold so being chilled or not does not have a great effect on the CO2 uptake. 


---
## Task 8 - Basic visualization with R

### 1. Install the Tidybiology package, which includes the data ‘chromosome’ and ‘proteins’
devtools::install_github("hirscheylab/tidybiology")

#### a. Extract summary statistics (mean, median and maximum) for the following variables from the ‘chromosome’ data: variations, protein coding genes, and miRNAs. Utilize the tidyverse functions to make this as simply as possible.

```r
data(chromosome)

summary_stats <- chromosome %>%
  summarise(
    across(
      c(variations, protein_codinggenes, mi_rna),
      list(
        mean = mean,
        median = median,
        max = max
      )
    )
  )

summary_stats
```

Answer:
|variations_mean|variations_median|variations_max|protein_codinggenes_mean|protein_codinggenes_median|protein_codinggenes_max|mi_rna_mean|mi_rna_median|mi_rna_max|
|-|-|-|-|-|-|-|-|-|
|6484572|6172346|12945965|849.9583|836|2058|73.16667|75|134|

---
#### b. How does the chromosome size distribute? Plot a graph that helps to visualize this by using ggplot2 package functions.

```r
ggplot(chromosome, aes(x = length_mm)) +
  geom_histogram(color = "black", fill = "skyblue", bins = 15) +
  geom_vline(aes(xintercept = mean(length_mm)),
             color = "red",
             linewidth = 1) +
  theme_minimal()
```
<img width="4032" height="625" alt="image" src="figures/chromosome_size_distribution.svg" />

Answer: The length varies between rougly 13-88mm, with a mean length around 45mm. There are no chromosomes in the data set with a length between roughly 70-80mm. 

---
#### c. Does the number of protein coding genes or miRNAs correlate with the length of the chromosome? Make two separate plots to visualize these relationships.

```r
cor.test(chromosome$length_mm,
         chromosome$protein_codinggenes,
         method = "pearson")

cor.test(chromosome$length_mm,
         chromosome$mi_rna,
         method = "pearson")


ggplot(chromosome,
       aes(x = length_mm,
           y = protein_codinggenes)) +
  geom_point(size = 3, alpha = 0.7) +
  geom_text(aes(label = id),
            vjust = -0.9,
            size = 3)+
  geom_smooth(method = "lm", se = TRUE, color = "red") +
  labs(
    title = "Protein-coding genes vs chromosome length",
    subtitle = "Pearson r = 0.61, p < 0.001",
    x = "Length in mm",
    y = "Number of protein coding genes"
  ) +
  theme_bw()
```
<img width="4032" height="625" alt="image" src="figures/Protein-coding_genes_chromosome_length.svg" />

```r
ggplot(chromosome,
       aes(x = length_mm,
           y = mi_rna)) +
  geom_point(size = 3, alpha = 0.7) +
  geom_text(aes(label = id),
            vjust = -0.9,
            size = 3)+
  geom_smooth(method = "lm", se = TRUE, color = "red") +  
  labs(
    title = "miRNAs vs chromosome length",
    subtitle = "Pearson r = 0.74, p < 0.001",
    x = "Length in mm",
    y = "Number of miRNAs"
  ) +
  theme_bw()
```
<img width="4032" height="625" alt="image" src="figures/miRNA_chromosome_length.svg" />


Answer: There is a correlation between length and number of miRNAs, and length and number of protein-coding genes, showing that longer genes tend to have more miRNAs and more protein-coding genes. The correlation with length is stronger for number of miRNAs than for number of protein-coding genes.

---
#### d. Calculate the same summary statistics for the ‘proteins’ data variables length and mass. Create a meaningful visualization of the relationship between these two variables by utilizing the ggplot2 package functions. Play with the colors, theme- and other visualization parameters to create a plot that pleases you.

```r
data(proteins)

stats_proteins <- proteins %>%
  summarise(
    across(
      c(length, mass),
      list(
        mean = mean,
        median = median,
        max = max
      )
    )
  )

stats_proteins


cor.test(proteins$length,
         proteins$mass,
         method = "pearson")

ggplot(proteins,
       aes(x = length,
           y = mass)) +
  geom_point(size = 3, alpha = 0.7) +
  geom_smooth(method = "lm", se = TRUE, color = "red") +  
  labs(
    title = "Protein mass vs length",
    subtitle = "Pearson r = 0.999, p < 0.001",
    x = "Protein length",
    y = "Protein mass"
  ) +
  theme_bw()
```
<img width="4032" height="625" alt="image" src="figures/protein_mass_vs_length.svg" />

Answer: There is a very strong correlation between protein length and protein mass (Pearson r = 0.999, p < 0.001).

---
