# Be careful when left_join tables with duplicated rows

- **Author:** Ming Tommy Tang
- **Date:** Oct 17, 2021
- **Tags:** R, data
- **Source:** [https://divingintogeneticsandgenomics.com/post/be-careful-when-left-join-tables-with-duplicated-rows/](https://divingintogeneticsandgenomics.com/post/be-careful-when-left-join-tables-with-duplicated-rows/)

---

This is going to be a really short blog post. I recently found that if I join two tables with one of the tables having duplicated rows, the final joined table also contains the duplicated rows. It could be the expected behavior for others but I want to make a note here for myself.

```r
library(tidyverse)

df1<- tibble(key = c("A", "B", "C", "D", "E"),
             value = 1:5)

df1
```

```bash
## # A tibble: 5 x 2
##   key   value
##   <chr> <int>
## 1 A         1
## 2 B         2
## 3 C         3
## 4 D         4
## 5 E         5
```

dataframe 2 has two identical rows for `B`.

```r
df2<- tibble(key = c("B", "B", "D", "C", "A", "E"),
             value2 = c(5, 5:9))

df2
```

```bash
## # A tibble: 6 x 2
##   key   value2
##   <chr>  <dbl>
## 1 B          5
## 2 B          5
## 3 D          6
## 4 C          7
## 5 A          8
## 6 E          9
```

Let’s see how the `left_join` behaviors:

```
left_join(df1, df2)
```

```bash
## Joining, by = "key"
```

```bash
## # A tibble: 6 x 3
##   key   value value2
##   <chr> <int>  <dbl>
## 1 A         1      8
## 2 B         2      5
## 3 B         2      5
## 4 C         3      7
## 5 D         4      6
## 6 E         5      9
```

You see there are two identical rows for `B`!

If that’s not what you want, you can either filter `df2` first or filter the the joined table.

```
left_join(df1, df2) %>%
  distinct(key, value, value2, .keep_all =TRUE)
```

```bash
## Joining, by = "key"
```

```bash
## # A tibble: 5 x 3
##   key   value value2
##   <chr> <int>  <dbl>
## 1 A         1      8
## 2 B         2      5
## 3 C         3      7
## 4 D         4      6
## 5 E         5      9
```
