# How to separate a comma delimited string into multiple lines in R and python

- **Author:** Ming Tommy Tang
- **Date:** May 14, 2024
- **Tags:** python, rstats
- **Source:** [https://divingintogeneticsandgenomics.com/post/how-to-separate-a-comma-delimited-string-into-multiple-lines-in-r-and-python/](https://divingintogeneticsandgenomics.com/post/how-to-separate-a-comma-delimited-string-into-multiple-lines-in-r-and-python/)

---

**To not miss a post like this, sign up for my [newsletter](https://divingintogeneticsandgenomics.ck.page/profile) to learn computational
biology and bioinformatics.**

### The problem

```r
df<- data.frame(id = c(1,2,3), value = c('x,y', 'z,w', 'a'))
df
```

```bash
#>   id value
#> 1  1   x,y
#> 2  2   z,w
#> 3  3     a
```

we want to put x,y in the first row into two rows:

1, x

1, y

and put z,w into two rows too.

### solution with R

There is a neat function `separate_rows` that does exactly this in `tidyr` package:

```r
library(tidyr)
library(dplyr)

df %>%
        tidyr::separate_rows(value, sep=",")
```

```bash
#> # A tibble: 5 × 2
#>      id value
#>   <dbl> <chr>
#> 1     1 x    
#> 2     1 y    
#> 3     2 z    
#> 4     2 w    
#> 5     3 a
```

We can also convert it back using some purrr tricks.

```r
library(tidyr)
library(purrr)

df_nest<- df %>%
        tidyr::separate_rows(value, sep=",") %>%
        group_by(id) %>%
        nest()

df_nest
```

```bash
#> # A tibble: 3 × 2
#> # Groups:   id [3]
#>      id data            
#>   <dbl> <list>          
#> 1     1 <tibble [2 × 1]>
#> 2     2 <tibble [2 × 1]>
#> 3     3 <tibble [1 × 1]>
```

```
df_nest$data[[1]]
```

```bash
#> # A tibble: 2 × 1
#>   value
#>   <chr>
#> 1 x    
#> 2 y
```

```bash
# convert it back 
df_nest %>%
        mutate(new_data = map_chr(data, ~paste(.x$value, collapse = ",")))
```

```bash
#> # A tibble: 3 × 3
#> # Groups:   id [3]
#>      id data             new_data
#>   <dbl> <list>           <chr>   
#> 1     1 <tibble [2 × 1]> x,y     
#> 2     2 <tibble [2 × 1]> z,w     
#> 3     3 <tibble [1 × 1]> a
```

### solution with python

Note, how to use python in Rmarkdown, read [https://rstudio.github.io/reticulate/articles/versions.html](https://rstudio.github.io/reticulate/articles/versions.html)

```r
library(reticulate)
use_condaenv("r-reticulate")
```

```python
import pandas as pd

df = pd.DataFrame({'id': [1, 2, 3], 'data': ['x, y', 'z, w', 'a']})

print(df)

# Split text into a list
```

```bash
#>    id  data
#> 0   1  x, y
#> 1   2  z, w
#> 2   3     a
```

```python
df['data'] = df['data'].str.split(',')
print(df)

# Convert list into multiple rows
```

```bash
#>    id     data
#> 0   1  [x,  y]
#> 1   2  [z,  w]
#> 2   3      [a]
```

```python
df = df.explode('data')
print(df)
```

```bash
#>    id data
#> 0   1    x
#> 0   1    y
#> 1   2    z
#> 1   2    w
#> 2   3    a
```

we used `explode` function in pandas. Of course, you can do it manually:
read in the dataframe line by line, split the last column to a list using str.split(‘,’) and open an empty file and write the str.splited entries into new lines.

### use case in bioinformatics?

follow [https://yulab-smu.top/biomedical-knowledge-mining-book/clusterprofiler-go.html](https://yulab-smu.top/biomedical-knowledge-mining-book/clusterprofiler-go.html)
for gene set enrichment analysis.

```r
library(clusterProfiler)
data(geneList, package="DOSE")
gene <- names(geneList)[abs(geneList) > 2]

# Entrez gene ID
head(gene)
```

```bash
#> [1] "4312"  "8318"  "10874" "55143" "55388" "991"
```

Let’s do a go-term over-representation analysis

```r
library(org.Hs.eg.db)

ego <- enrichGO(gene          = gene,
                universe      = names(geneList),
                OrgDb         = org.Hs.eg.db,
                ont           = "CC",
                pAdjustMethod = "BH",
                pvalueCutoff  = 0.01,
                qvalueCutoff  = 0.05,
                readable      = TRUE)
head(ego)
```

```bash
#>                    ID                              Description GeneRatio
#> GO:0005819 GO:0005819                                  spindle    26/201
#> GO:0000779 GO:0000779 condensed chromosome, centromeric region    16/201
#> GO:0072686 GO:0072686                          mitotic spindle    17/201
#> GO:0000775 GO:0000775           chromosome, centromeric region    18/201
#> GO:0098687 GO:0098687                       chromosomal region    23/201
#> GO:0000776 GO:0000776                              kinetochore    15/201
#>              BgRatio       pvalue     p.adjust       qvalue
#> GO:0005819 306/11853 1.072029e-11 3.151766e-09 2.888837e-09
#> GO:0000779 114/11853 7.709944e-11 8.659125e-09 7.936756e-09
#> GO:0072686 133/11853 8.835841e-11 8.659125e-09 7.936756e-09
#> GO:0000775 158/11853 1.684987e-10 1.179661e-08 1.081250e-08
#> GO:0098687 272/11853 2.006225e-10 1.179661e-08 1.081250e-08
#> GO:0000776 106/11853 2.733425e-10 1.339378e-08 1.227644e-08
#>                                                                                                                                                               geneID
#> GO:0005819 CDCA8/CDC20/KIF23/CENPE/ASPM/DLGAP5/SKA1/NUSAP1/TPX2/TACC3/NEK2/CDK1/MAD2L1/KIF18A/BIRC5/KIF11/TRAT1/TTK/AURKB/PRC1/KIFC1/KIF18B/KIF20A/AURKA/CCNB1/KIF4A
#> GO:0000779                                                             CENPE/NDC80/HJURP/SKA1/NEK2/CENPM/CENPN/ERCC6L/MAD2L1/KIF18A/CDT1/BIRC5/TTK/NCAPG/AURKB/CCNB1
#> GO:0072686                                                      KIF23/CENPE/ASPM/SKA1/NUSAP1/TPX2/TACC3/CDK1/MAD2L1/KIF18A/KIF11/TRAT1/AURKB/PRC1/KIFC1/KIF18B/AURKA
#> GO:0000775                                                 CDCA8/CENPE/NDC80/TOP2A/HJURP/SKA1/NEK2/CENPM/CENPN/ERCC6L/MAD2L1/KIF18A/CDT1/BIRC5/TTK/NCAPG/AURKB/CCNB1
#> GO:0098687                   CDCA8/CENPE/NDC80/TOP2A/HJURP/SKA1/NEK2/CENPM/RAD51AP1/CENPN/CDK1/ERCC6L/MAD2L1/KIF18A/CDT1/BIRC5/EZH2/TTK/NCAPG/AURKB/CHEK1/CCNB1/MCM5
#> GO:0000776                                                                   CENPE/NDC80/HJURP/SKA1/NEK2/CENPM/CENPN/ERCC6L/MAD2L1/KIF18A/CDT1/BIRC5/TTK/AURKB/CCNB1
#>            Count
#> GO:0005819    26
#> GO:0000779    16
#> GO:0072686    17
#> GO:0000775    18
#> GO:0098687    23
#> GO:0000776    15
```

The last `geneID` column is a list of genes separated by `/`. If I want to
get a tidy dataframe, I can do

```
ego@result %>% 
        tidyr::separate_rows(geneID, sep="/")
```

```bash
#> # A tibble: 918 × 9
#>    ID       Description GeneRatio BgRatio   pvalue p.adjust  qvalue geneID Count
#>    <chr>    <chr>       <chr>     <chr>      <dbl>    <dbl>   <dbl> <chr>  <int>
#>  1 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 CDCA8     26
#>  2 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 CDC20     26
#>  3 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 KIF23     26
#>  4 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 CENPE     26
#>  5 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 ASPM      26
#>  6 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 DLGAP5    26
#>  7 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 SKA1      26
#>  8 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 NUSAP1    26
#>  9 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 TPX2      26
#> 10 GO:0005… spindle     26/201    306/11… 1.07e-11  3.15e-9 2.89e-9 TACC3     26
#> # ℹ 908 more rows
```

Now each gene has its own row!
