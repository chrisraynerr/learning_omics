# add pct_in for each cluster for scRNAseq result table using list column

- **Author:** Ming Tommy Tang
- **Date:** Feb 4, 2020
- **Tags:** scRNAseq
- **Source:** [https://divingintogeneticsandgenomics.com/post/add-pct-in-for-each-cluster-for-scrnaseq-result-table-using-list-column/](https://divingintogeneticsandgenomics.com/post/add-pct-in-for-each-cluster-for-scrnaseq-result-table-using-list-column/)

---

Using nested dataframe and list column has transformed my way of data wrangling in R. For more on this topic, I highly recommend [purrr tutorial](https://jennybc.github.io/purrr-tutorial/index.html) from Jenney Bryan.

In this post, I am going to show you how I use this to solve a problem for adding `pct_in` column from the differential scRNAseq result table.

I am going to use [`presto`](https://github.com/immunogenomics/presto) for differential gene expression test. `presto` performs a fast Wilcoxon rank sum test and auROC analysis. It can be used for differential accessible region test for scATACseq data as well. Because scATACseq data can have over 800k regions in my hand, I found it is much faster than `Seurat` and also gives sensible results. Using presto also gives you all the genes/regions without filtering. This is particularly useful if you want to run GSEA which requires all genes as input. see [this part](https://crazyhottommy.github.io/scRNA-seq-workshop-Fall-2019/scRNAseq_workshop_3.html) for our scRNAseq workshop.

Let’s download some scRNAseq example data from [https://satijalab.org/seurat/v3.1/atacseq_integration_vignette.html](https://satijalab.org/seurat/v3.1/atacseq_integration_vignette.html).

`curl -L https://www.dropbox.com/s/3f3p5nxrn5b3y4y/pbmc_10k_v3.rds\?dl\=1 -o pbmc_10k_v3.rds`

### read into R

```r
# install_github('immunogenomics/presto')
library(presto)
library(Seurat)
library(dplyr)
library(tibble)
library(furrr)
library(tictoc)
pbmc<- readRDS("~/pbmc_10k_v3.rds")

head(wilcoxauc(pbmc, "RNA_snn_res.0.4" ))
```

```bash
##      feature group     avgExpr         logFC statistic       auc
## 1 AL627309.1     0 0.004455649 -0.0001475212   9648856 0.5007586
## 2 AL669831.5     0 0.058653109  0.0155762453   9974323 0.5176497
## 3     FAM87B     0 0.001185081  0.0007726323   9649065 0.5007694
## 4  LINC00115     0 0.023454801 -0.0001696563   9701960 0.5035145
## 5     FAM41C     0 0.025523520  0.0035611306   9745564 0.5057775
## 6 AL645608.3     0 0.000766896  0.0003039512   9642626 0.5004352
##           pval         padj    pct_in    pct_out
## 1 3.450509e-01 4.033191e-01 0.6350267 0.48136646
## 2 7.957294e-12 2.279022e-11 8.3221925 4.58074534
## 3 2.429577e-02 3.743801e-02 0.2005348 0.04658385
## 4 5.546520e-02 8.014953e-02 3.3422460 2.59316770
## 5 1.339544e-03 2.430893e-03 3.5427807 2.34472050
## 6 1.485920e-01 1.932465e-01 0.1336898 0.04658385
```

by default, `presto` and `Seurat` compare a gene in cells of one group versus all other groups of cells and calculate the
statistics. In the output, you see `pct_in` and `pct_out` columns which show the percentage of cells express this gene in the `in` group and perecentage of cells express this gene in the `out` groups. What if you want to know `pct_out` in each of the group? How do you add that information to the dataframe? In addtion, you may also want to add the number of cells in each cluster into the dataframe.

```r
res<- wilcoxauc(pbmc, "RNA_snn_res.0.4" )

## how many genes in the result?
length(unique(res$feature))
```

```bash
## [1] 19089
```

```bash
## for each group we have the same number of genes
count(res, group) %>% arrange(as.numeric(group))
```

```bash
## # A tibble: 13 x 2
##    group     n
##    <chr> <int>
##  1 0     19089
##  2 1     19089
##  3 2     19089
##  4 3     19089
##  5 4     19089
##  6 5     19089
##  7 6     19089
##  8 7     19089
##  9 8     19089
## 10 9     19089
## 11 10    19089
## 12 11    19089
## 13 12    19089
```

get a dataframe for number of cells in each group

```r
(cell_number<- pbmc@meta.data %>%
  count(RNA_snn_res.0.4) %>%
  dplyr::rename(group = RNA_snn_res.0.4, cell_number = n))
```

```bash
## # A tibble: 13 x 2
##    group cell_number
##    <fct>       <int>
##  1 0            2992
##  2 1            1596
##  3 2            1047
##  4 3             959
##  5 4             592
##  6 5             544
##  7 6             460
##  8 7             383
##  9 8             337
## 10 9             328
## 11 10             74
## 12 11             68
## 13 12             52
```

Let’s nest the dataframe by gene

```r
res_nest<- res %>%
  group_by(feature) %>% 
  tidyr::nest()

res_nest
```

```bash
## # A tibble: 19,089 x 2
## # Groups:   feature [19,089]
##    feature    data                                                         
##    <chr>      <S3: vctrs_list_of>                                          
##  1 AL627309.1 0                    , 1                    , 10            …
##  2 AL669831.5 0                   , 1                   , 10              …
##  3 FAM87B     0                    , 1                    , 10            …
##  4 LINC00115  0                    , 1                    , 10            …
##  5 FAM41C     0                   , 1                   , 10              …
##  6 AL645608.3 0                    , 1                    , 10            …
##  7 SAMD11     0                    , 1                    , 10            …
##  8 NOC2L      0                   , 1                   , 10              …
##  9 KLHL17     0                   , 1                   , 10              …
## 10 PLEKHN1    0                    , 1                    , 10            …
## # … with 19,079 more rows
```

`res_nest` is a nested dataframe with a list column named `data`. Let’s check the first entry of this list.

```r
df<- res_nest$data[[1]] %>% arrange(as.numeric(group))
head(df)  
```

```bash
## # A tibble: 6 x 9
##   group avgExpr     logFC statistic   auc   pval  padj pct_in pct_out
##   <chr>   <dbl>     <dbl>     <dbl> <dbl>  <dbl> <dbl>  <dbl>   <dbl>
## 1 0     0.00446 -0.000148  9648856. 0.501 0.345  0.403  0.635   0.481
## 2 1     0.00206 -0.00300   6232089  0.498 0.0916 0.162  0.251   0.587
## 3 2     0.00285 -0.00192   4377551  0.499 0.251  0.364  0.287   0.561
## 4 3     0.00685  0.00255   4067216. 0.501 0.661  0.723  0.626   0.519
## 5 4     0.00650  0.00208   2620733  0.501 0.612  0.719  0.676   0.520
## 6 5     0.00834  0.00401   2422859  0.501 0.492  0.648  0.735   0.518
```

Now, we can collect the `pct_in` for this gene from `df`.

```r
(df<- df %>% 
  left_join(cell_number, by = c("group" = "group")) %>%
  mutate(pct_in_group = paste(group, pct_in, sep= "_")))
```

```bash
## Warning: Column `group` joining character vector and factor, coercing into
## character vector
```

```bash
## # A tibble: 13 x 11
##    group avgExpr    logFC statistic   auc   pval  padj pct_in pct_out
##    <chr>   <dbl>    <dbl>     <dbl> <dbl>  <dbl> <dbl>  <dbl>   <dbl>
##  1 0     0.00446 -1.48e-4  9648856. 0.501 0.345  0.403  0.635   0.481
##  2 1     0.00206 -3.00e-3  6232089  0.498 0.0916 0.162  0.251   0.587
##  3 2     0.00285 -1.92e-3  4377551  0.499 0.251  0.364  0.287   0.561
##  4 3     0.00685  2.55e-3  4067216. 0.501 0.661  0.723  0.626   0.519
##  5 4     0.00650  2.08e-3  2620733  0.501 0.612  0.719  0.676   0.520
##  6 5     0.00834  4.01e-3  2422859  0.501 0.492  0.648  0.735   0.518
##  7 6     0.00816  3.78e-3  2070947  0.502 0.303  0.507  0.870   0.513
##  8 7     0.00245 -2.20e-3  1728031  0.499 0.460  0.685  0.261   0.541
##  9 8     0       -4.73e-3  1524082. 0.497 0.172  0.348  0       0.550
## 10 9     0.00533  7.97e-4  1498956. 0.502 0.333  0.524  0.915   0.516
## 11 10    0.00609  1.55e-3   349088. 0.504 0.333  0.586  1.35    0.524
## 12 11    0       -4.59e-3   316676  0.497 0.546  0.848  0       0.534
## 13 12    0.0295   2.50e-2   247320. 0.507 0.163  0.437  1.92    0.522
## # … with 2 more variables: cell_number <int>, pct_in_group <chr>
```

```r
# interleave the pct_in and number_in 
pct_in_groups<- df$pct_in
num_in_groups<- df$cell_number
names_pct_in_groups<-  paste(df$group,"pct_in", sep = "_")
names_num_in_groups<- paste(df$group, "cell_num", sep= "_")
# https://stackoverflow.com/questions/16443260/interleave-lists-in-r
out<- c(rbind(num_in_groups, pct_in_groups))
names(out)<- c(rbind(names_num_in_groups, names_pct_in_groups))
out<- bind_rows(out)
out
```

```bash
## # A tibble: 1 x 26
##   `0_cell_num` `0_pct_in` `1_cell_num` `1_pct_in` `2_cell_num` `2_pct_in`
##          <dbl>      <dbl>        <dbl>      <dbl>        <dbl>      <dbl>
## 1         2992      0.635         1596      0.251         1047      0.287
## # … with 20 more variables: `3_cell_num` <dbl>, `3_pct_in` <dbl>,
## #   `4_cell_num` <dbl>, `4_pct_in` <dbl>, `5_cell_num` <dbl>,
## #   `5_pct_in` <dbl>, `6_cell_num` <dbl>, `6_pct_in` <dbl>,
## #   `7_cell_num` <dbl>, `7_pct_in` <dbl>, `8_cell_num` <dbl>,
## #   `8_pct_in` <dbl>, `9_cell_num` <dbl>, `9_pct_in` <dbl>,
## #   `10_cell_num` <dbl>, `10_pct_in` <dbl>, `11_cell_num` <dbl>,
## #   `11_pct_in` <dbl>, `12_cell_num` <dbl>, `12_pct_in` <dbl>
```

We have 13 groups of cells, so we get a tibble of 1 x 26 with number of cells and percentage of cells for each group in each column. We now only need to `cbind` this info back to each gene.

Let’s write a function

```r
add_pct_in<- function(df, cell_number){
  df<- df %>% 
  left_join(cell_number, by = c("group" = "group")) %>%
  mutate(pct_in_group = paste(group, pct_in, sep= "_"))
  
  pct_in_groups<- df$pct_in
  num_in_groups<- df$cell_number
  names_pct_in_groups<-  paste(df$group,"pct_in", sep = "_")
  names_num_in_groups<- paste(df$group, "cell_num", sep= "_")
  # https://stackoverflow.com/questions/16443260/interleave-lists-in-r
  out<- c(rbind(num_in_groups, pct_in_groups))
  names(out)<- c(rbind(names_num_in_groups, names_pct_in_groups))
  out<- bind_rows(out)
  return(out)
}

## test this function for one gene
add_pct_in(df = res_nest$data[[1]], cell_number = cell_number )
```

```bash
## Warning: Column `group` joining character vector and factor, coercing into
## character vector
```

```bash
## # A tibble: 1 x 26
##   `0_cell_num` `0_pct_in` `1_cell_num` `1_pct_in` `10_cell_num` `10_pct_in`
##          <dbl>      <dbl>        <dbl>      <dbl>         <dbl>       <dbl>
## 1         2992      0.635         1596      0.251            74        1.35
## # … with 20 more variables: `11_cell_num` <dbl>, `11_pct_in` <dbl>,
## #   `12_cell_num` <dbl>, `12_pct_in` <dbl>, `2_cell_num` <dbl>,
## #   `2_pct_in` <dbl>, `3_cell_num` <dbl>, `3_pct_in` <dbl>,
## #   `4_cell_num` <dbl>, `4_pct_in` <dbl>, `5_cell_num` <dbl>,
## #   `5_pct_in` <dbl>, `6_cell_num` <dbl>, `6_pct_in` <dbl>,
## #   `7_cell_num` <dbl>, `7_pct_in` <dbl>, `8_cell_num` <dbl>,
## #   `8_pct_in` <dbl>, `9_cell_num` <dbl>, `9_pct_in` <dbl>
```

Because we are going to apply this function to over 10,000 genes, I am going to use the parallized `purrr`: `furrr`.
[https://github.com/DavisVaughan/furrr](https://github.com/DavisVaughan/furrr)

```python
plan(multiprocess, workers = 8)

# this will start 8 workers, but each worker will consume 20Mb memory 
print(object.size(res), units= "Mb")
```

```bash
## 20 Mb
```

```r
tic()
info<- furrr::future_map_dfr(res_nest$data, ~ add_pct_in(df= .x, cell_number = cell_number)) 
toc()
```

```bash
## 9.007 sec elapsed
```

```
head(info)
```

```bash
## # A tibble: 6 x 26
##   `0_cell_num` `0_pct_in` `1_cell_num` `1_pct_in` `10_cell_num` `10_pct_in`
##          <dbl>      <dbl>        <dbl>      <dbl>         <dbl>       <dbl>
## 1         2992      0.635         1596     0.251             74        1.35
## 2         2992      8.32          1596     3.57              74       20.3 
## 3         2992      0.201         1596     0.0627            74        0   
## 4         2992      3.34          1596     2.26              74        9.46
## 5         2992      3.54          1596     1.63              74        9.46
## 6         2992      0.134         1596     0                 74        0   
## # … with 20 more variables: `11_cell_num` <dbl>, `11_pct_in` <dbl>,
## #   `12_cell_num` <dbl>, `12_pct_in` <dbl>, `2_cell_num` <dbl>,
## #   `2_pct_in` <dbl>, `3_cell_num` <dbl>, `3_pct_in` <dbl>,
## #   `4_cell_num` <dbl>, `4_pct_in` <dbl>, `5_cell_num` <dbl>,
## #   `5_pct_in` <dbl>, `6_cell_num` <dbl>, `6_pct_in` <dbl>,
## #   `7_cell_num` <dbl>, `7_pct_in` <dbl>, `8_cell_num` <dbl>,
## #   `8_pct_in` <dbl>, `9_cell_num` <dbl>, `9_pct_in` <dbl>
```

```bash
## cbind back to the nested dataframe
bind_cols(res_nest, info) %>%
  head()
```

```bash
## # A tibble: 6 x 28
## # Groups:   feature [6]
##   feature data  `0_cell_num` `0_pct_in` `1_cell_num` `1_pct_in`
##   <chr>   <S3:>        <dbl>      <dbl>        <dbl>      <dbl>
## 1 AL6273… 0   …         2992      0.635         1596     0.251 
## 2 AL6698… 0   …         2992      8.32          1596     3.57  
## 3 FAM87B  0   …         2992      0.201         1596     0.0627
## 4 LINC00… 0   …         2992      3.34          1596     2.26  
## 5 FAM41C  0   …         2992      3.54          1596     1.63  
## 6 AL6456… 0   …         2992      0.134         1596     0     
## # … with 22 more variables: `10_cell_num` <dbl>, `10_pct_in` <dbl>,
## #   `11_cell_num` <dbl>, `11_pct_in` <dbl>, `12_cell_num` <dbl>,
## #   `12_pct_in` <dbl>, `2_cell_num` <dbl>, `2_pct_in` <dbl>,
## #   `3_cell_num` <dbl>, `3_pct_in` <dbl>, `4_cell_num` <dbl>,
## #   `4_pct_in` <dbl>, `5_cell_num` <dbl>, `5_pct_in` <dbl>,
## #   `6_cell_num` <dbl>, `6_pct_in` <dbl>, `7_cell_num` <dbl>,
## #   `7_pct_in` <dbl>, `8_cell_num` <dbl>, `8_pct_in` <dbl>,
## #   `9_cell_num` <dbl>, `9_pct_in` <dbl>
```

Finally, we can unnest the dataframe

```
bind_cols(res_nest, info) %>% 
  tidyr::unnest() %>%
  head()
```

```bash
## Warning: `cols` is now required.
## Please use `cols = c(data)`
```

```bash
## # A tibble: 6 x 36
## # Groups:   feature [1]
##   feature group avgExpr    logFC statistic   auc   pval  padj pct_in
##   <chr>   <chr>   <dbl>    <dbl>     <dbl> <dbl>  <dbl> <dbl>  <dbl>
## 1 AL6273… 0     0.00446 -1.48e-4  9648856. 0.501 0.345  0.403  0.635
## 2 AL6273… 1     0.00206 -3.00e-3  6232089  0.498 0.0916 0.162  0.251
## 3 AL6273… 10    0.00609  1.55e-3   349088. 0.504 0.333  0.586  1.35 
## 4 AL6273… 11    0       -4.59e-3   316676  0.497 0.546  0.848  0    
## 5 AL6273… 12    0.0295   2.50e-2   247320. 0.507 0.163  0.437  1.92 
## 6 AL6273… 2     0.00285 -1.92e-3  4377551  0.499 0.251  0.364  0.287
## # … with 27 more variables: pct_out <dbl>, `0_cell_num` <dbl>,
## #   `0_pct_in` <dbl>, `1_cell_num` <dbl>, `1_pct_in` <dbl>,
## #   `10_cell_num` <dbl>, `10_pct_in` <dbl>, `11_cell_num` <dbl>,
## #   `11_pct_in` <dbl>, `12_cell_num` <dbl>, `12_pct_in` <dbl>,
## #   `2_cell_num` <dbl>, `2_pct_in` <dbl>, `3_cell_num` <dbl>,
## #   `3_pct_in` <dbl>, `4_cell_num` <dbl>, `4_pct_in` <dbl>,
## #   `5_cell_num` <dbl>, `5_pct_in` <dbl>, `6_cell_num` <dbl>,
## #   `6_pct_in` <dbl>, `7_cell_num` <dbl>, `7_pct_in` <dbl>,
## #   `8_cell_num` <dbl>, `8_pct_in` <dbl>, `9_cell_num` <dbl>,
## #   `9_pct_in` <dbl>
```

There are possiblely other easier ways to achieve the same result. Please share your thoughts in the comments!
