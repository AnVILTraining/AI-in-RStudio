

# Run DESeq2

Perform bulk RNA-seq Differential Expression analysis with local AI, gander, and DESeq2

<br>

## Learning objectives

1. Perform pre-filtering on a raw count matrix to optimize performance and visualization
1. Execute a standard differential expression analysis using DESeq2
1. Explore and visualize results

## Filter low counts

### Install and Load Required Packages

1. Run the following code in the R Console:


``` r
# Bioconductor packages
BiocManager::install("DESeq2")
library(DESeq2)
```

### Filter genes with low counts

Filtering low-count genes before using DESeq2 is optional, but it is a standard practice that helps optimize performance and clean up graphical outputs. 
For example, eliminating genes with few or no reads reduces the memory size of the data object, speeds up count modeling and improves noisiness in visual representations. 

1. Retain genes with a total count of at least 1 across all samples. 

In a clean R Script:

> **HIGHLIGHT**: `counts`
>
> **PROMPT**: Filter genes with ≥ 1 counts in all samples and create a new filtered counts object

***sample gander output***:


``` r
counts_filtered <- counts[rowSums(counts >= 1) == ncol(counts), ]
```

2. Check for presence of zero counts

In a clean R Script:

> **HIGHLIGHT**: `counts_filtered`
>
> **PROMPT**: Are there any genes with zero counts?

***sample gander output***:


``` r
any(colSums(counts_filtered == 0) > 0)
```

3. Compare counts to filtered counts

> **HIGHLIGHT**: both, `counts` and `counts_filtered`
>
> **PROMPT**: Summarize counts_filtered and compare to counts

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_52.png" width="100%" style="display: block; margin: auto;" />

***sample gander output***:


``` r
summary(counts_filtered)
summary(counts)
```

## Run Differential expression analysis with DEseq2

### Run DESeq2 on filtered counts

1. In a clean R Script: 

> **HIGHLIGHT**: both, `counts_filtered` and `metadata`
>
> **PROMPT**: Perform differential expression with filtered counts using DESeq2 and `dex` as the design condition then save the results as a new object.


We recommend highlighting both objects, the filtered counts, and the metadata, for gander to peek at both to understand the full analysis context. Note that output names like counts_filtered can vary.

***sample gander output***:


``` r
library(DESeq2)

dds <- DESeqDataSetFromMatrix(countData = counts_filtered,
                              colData = metadata,
                              design = ~ dex)

dds <- DESeq(dds)

results <- results(dds, contrast = c("dex", "trt", "untrt"))
```

### Inspect and Summarize Results

1. View results. In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: View results table

***sample gander output***:


``` r
results %>%
  as.data.frame() %>%
  rownames_to_column(var = "gene") %>%
  head()
```

Running the code above gives this:

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_59.png" width="100%" style="display: block; margin: auto;" />

2. Order results by p-value. 

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: View results table

***sample gander output***:


``` r
results[order(results$pvalue), ]
```
Running the code above gives this:

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_66.png" width="100%" style="display: block; margin: auto;" />

3. Summarize results.

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: Summarize results

***sample gander output***:


``` r
summary(results)
```

Running the code above gives this:

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_72.png" width="100%" style="display: block; margin: auto;" />

4. Count genes passing a significance threshold

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: How many genes have padj < 0.05?

***sample gander output***:


``` r
sum(results$padj < 0.05, na.rm = TRUE)
```

5. View top 10 significant genes

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: View top 10 significant upregulated genes based on padj

***sample gander output***:


``` r
results %>%
  as.data.frame() %>%
  filter(padj < 0.05) %>%
  arrange(desc(log2FoldChange)) %>%
  head(10) %>%
  select(c("log2FoldChange", "padj"))
```

### Filter results based on padj value

1. Filter to keep only statistically significant results based on a modified adjusted p-value threshold.

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: Filter results based on cutoff padj < 0.05 and save as a new filtered-results object

***sample gander output***:


``` r
filtered_results <- results[results$padj < 0.05, ]
```

::: {.warning}
ERROR! When running code above, `filtered_results <- results[results$padj < 0.05, ]`, there is an error: *Error: logical subscript contains NAs.*
:::

2. Troubleshoot an error

To troubleshoot error above, highlight both, the code and the error message and feed both to gander:

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_93.png" width="100%" style="display: block; margin: auto;" />
***sample gander output***:


``` r
filtered_results <- results[!is.na(results$padj)&results$padj < 0.05, ]
```
The code is now fixed and will not produce an error.

## Visualize plots and save results

### Visualize an MA plot

1. Generate and saves an MA plot to visualize differential gene expression data.

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: Use DESeq2 to create an MA plot. Save the plot.

***sample gander output***:


``` r
library(DESeq2)
plotMA(results, alpha=0.1, ylim=c(-3,3))
dev.copy(png, "ma_plot.png", width=600, height=600)
dev.off()
```

Running the code above gives this:

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_88.png" width="100%" style="display: block; margin: auto;" />

Or, AI may also choose to build an MA plot using ggplot2: 


``` r
library(ggplot2)
results_df <- as.data.frame(results)
results_df$significant<-results_df$padj < 0.05
ggplot(results_df, aes(x = baseMean, y = log2FoldChange, color = significant)) +
  geom_point(alpha = 0.6) +
  scale_x_log10() +
  geom_hline(yintercept = 0, linetype = "dashed") +
  labs(title = "MA Plot", x = "Mean of normalized counts", y = "Log2 fold change") +
  theme_minimal() +
  scale_color_manual(values = c("FALSE" = "black", "TRUE" = "red"))
```

Running the code above gives this:

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_77.png" width="100%" style="display: block; margin: auto;" />

### Visualize a volcano plot

1. Create a volcano plot to visualize differential gene expression.

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: Create a volcano plot

***sample gander output***:


``` r
library(ggplot2)

results_df <- as.data.frame(results)
results_dfsignificant<-ifelse(results_df$padj < 0.05 & abs(results_df$log2FoldChange) > 1, 
                                 ifelse(results_df$log2FoldChange > 1, "Up", "Down"), "NotSignificant")

ggplot(results_df, aes(x = log2FoldChange, y = -log10(padj))) +
  geom_point(aes(color = significant), alpha = 0.6) +
  scale_color_manual(values = c("Up" = "red", "Down" = "blue", "NotSignificant" = "gray")) +
  geom_hline(yintercept = -log10(0.05), linetype = "dashed") +
  geom_vline(xintercept = c(-1, 1), linetype = "dashed") +
  labs(title = "Volcano Plot", x = "Log2 Fold Change", y = "-Log10 Adjusted P-value") +
  theme_minimal()
```

Running the code above gives this:

<img src="04-run-deseq2_files/figure-html//1zrqf2fLtglxi5l1qx5GCaRlDkKR8ZFp-ldmlqtqWGZg_h324ef5e179219b9e_0_83.png" width="100%" style="display: block; margin: auto;" />

2. Save results as a .csv file

In a clean R Script: 

> **HIGHLIGHT**: `results`
>
> **PROMPT**: Save results as a csv file

***sample gander output***:


``` r
as.data.frame(results) %>% write.csv("results.csv", row.names = FALSE)
```

Note: `row.names = TRUE` is typically preferred for DESeq2 outputs so that gene identifiers are preserved in the first column of the CSV file).

