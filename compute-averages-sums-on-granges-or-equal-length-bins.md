# Compute averages/sums on GRanges or equal length bins

- **Author:** Ming Tommy Tang
- **Date:** Sep 7, 2018
- **Tags:** GRanges
- **Source:** [https://divingintogeneticsandgenomics.com/post/compute-averages-sums-on-granges-or-equal-length-bins/](https://divingintogeneticsandgenomics.com/post/compute-averages-sums-on-granges-or-equal-length-bins/)

---

Googling is a required technique for programmers. Once I have a programming problem in mind, the first thing I do is to google to see if other people have encountered the same problem and maybe they already have a solution. Do not re-invent the wheels. Actually, reading other people’s code and mimicing their code is a great way of learning. Today, I am going to show you how to compute binned averages/sums along a genome or any genomic regions of interest. All the codes I am going to show I found them online.

#### How to compute binned averages along a genome

I found it in the [How to tutorial](https://bioconductor.org/packages/release/bioc/vignettes/GenomicRanges/inst/doc/GenomicRangesHOWTOs.pdf) of the GRanges package.

```r
# using yeast with smaller genome
library(BSgenome.Scerevisiae.UCSC.sacCer2)
library(GenomicRanges)
set.seed(55)
my_var <- RleList(
        lapply(seqlengths(Scerevisiae),
        function(seqlen) {
        tmp <- sample(50L, seqlen, replace=TRUE) %/% 50L
        Rle(cumsum(tmp - rev(tmp)))
        }
        ),
        compress=FALSE)

my_var
```

```bash
## RleList of length 18
## $chrI
## integer-Rle of length 230208 with 8693 runs
##   Lengths:  32  33  51  21  22   9   1  36 ...   1   9  22  21  51  33  33
##   Values :   0   1   2   1   0  -1   0   1 ...   0  -1   0   1   2   1   0
## 
## $chrII
## integer-Rle of length 813178 with 31959 runs
##   Lengths: 56 10 52 12 69  4 48 35 11  1 ...  1 11 35 48  4 69 12 52 10 57
##   Values :  0  1  0  1  0  1  2  1  2  3 ...  3  2  1  2  1  0  1  0  1  0
## 
## $chrIII
## integer-Rle of length 316617 with 12209 runs
##   Lengths:  20 116   9   2  21  16  43   3 ...  43  16  21   2   9 116  21
##   Values :   0  -1   0   1   0  -1  -2  -3 ...  -2  -1   0   1   0  -1   0
## 
## $chrIV
## integer-Rle of length 1531919 with 60091 runs
##   Lengths: 39 80 67 22 48 77 19 45 13  3 ...  3 13 45 19 77 48 22 67 80 40
##   Values :  0 -1  0  1  2  3  2  1  0  1 ...  1  0  1  2  3  2  1  0 -1  0
## 
## $chrV
## integer-Rle of length 576869 with 22903 runs
##   Lengths:  11  29   7   1  10  29  63  32 ...  63  29  10   1   7  29  12
##   Values :   0  -1  -2  -3  -4  -3  -4  -5 ...  -4  -3  -4  -3  -2  -1   0
## 
## ...
## <13 more elements>
```

tile the whole genome to 100 bp bins

```r
bins <- tileGenome(seqinfo(Scerevisiae), tilewidth=100,cut.last.tile.in.chrom=TRUE)
```

compute the binned average for my_var

```
binnedAverage(bins, my_var, "binned_var")
```

```bash
## GRanges object with 121639 ranges and 1 metadata column:
##            seqnames       ranges strand | binned_var
##               <Rle>    <IRanges>  <Rle> |  <numeric>
##        [1]     chrI   [  1, 100]      * |       1.03
##        [2]     chrI   [101, 200]      * |       0.75
##        [3]     chrI   [201, 300]      * |       0.92
##        [4]     chrI   [301, 400]      * |       2.75
##        [5]     chrI   [401, 500]      * |       6.06
##        ...      ...          ...    ... .        ...
##   [121635]  2micron [5901, 6000]      * |       4.76
##   [121636]  2micron [6001, 6100]      * |       2.62
##   [121637]  2micron [6101, 6200]      * |       0.87
##   [121638]  2micron [6201, 6300]      * |       0.03
##   [121639]  2micron [6301, 6318]      * |          0
##   -------
##   seqinfo: 18 sequences (2 circular) from sacCer2 genome
```

The key is to convert any values (sequencing depth across the genome, RNA-seq counts etc) into a RleList object, then one can use the `binnedAverage` to calculate the average across each small bin of the genome.

#### Transformation of GRange object to density per bin

see [post](https://stat.ethz.ch/pipermail/bioconductor/2013-January/050445.html)

```r
### 'x': a GenomicRanges objects with non-NA seqlengths.
### 'binsize': a single positive integer.
### 'mcolnames': names of numeric metadata columns in 'x' to "average"
###              i.e. to propagate to the result after averaging them
###              on each bin.
### Returns a GRanges object with: (a) the same seqinfo as 'x',
### (b) ranges of width 'binsize' covering all the sequences in
### 'seqinfo(x)', and (c) the "averaged" metadata columns specified
### in 'mcolnames'.

averagePerBin <- function(x, binsize, mcolnames=NULL)
{
     if (!is(x, "GenomicRanges"))
         stop("'x' must be a GenomicRanges object")
     if (any(is.na(seqlengths(x))))
         stop("'seqlengths(x)' contains NAs")
     bins <- IRangesList(lapply(seqlengths(x),
                                function(seqlen)
                                  IRanges(breakInChunks(seqlen, binsize))))
     ans <- as(bins, "GRanges")
     seqinfo(ans) <- seqinfo(x)
     if (is.null(mcolnames))
         return(ans)
     averageMCol <- function(colname)
     {
         cvg <- coverage(x, weight=colname)
         views_list <- RleViewsList(
                           lapply(names(cvg),
                               function(seqname)
                                   Views(cvg[[seqname]], bins[[seqname]])))
         unlist(viewMeans(views_list), use.names=FALSE)
     }
     mcols(ans) <- DataFrame(lapply(mcols(x)[mcolnames], averageMCol))
     ans
}
```

```r
library(BSgenome.Hsapiens.UCSC.hg19)

testset.gr<- GRanges("chr1", IRanges(start=seq( 50000, 55000,by = 100), width=50), strand = "+")

## Set the sequence lengths.
seqlengths(testset.gr) <- seqlengths(Hsapiens)[seqlevels(testset.gr)]

## Add the density metadata col.
mcols(testset.gr)$density <- 100

## Compute the average per bin for the specified metadata cols.
avg_per_bin <- averagePerBin(testset.gr, 100, mcolnames="density")

avg_per_bin[avg_per_bin$density > 0]
```

```bash
## GRanges object with 52 ranges and 1 metadata column:
##        seqnames         ranges strand |   density
##           <Rle>      <IRanges>  <Rle> | <numeric>
##    [1]     chr1 [49901, 50000]      * |         1
##    [2]     chr1 [50001, 50100]      * |        50
##    [3]     chr1 [50101, 50200]      * |        50
##    [4]     chr1 [50201, 50300]      * |        50
##    [5]     chr1 [50301, 50400]      * |        50
##    ...      ...            ...    ... .       ...
##   [48]     chr1 [54601, 54700]      * |        50
##   [49]     chr1 [54701, 54800]      * |        50
##   [50]     chr1 [54801, 54900]      * |        50
##   [51]     chr1 [54901, 55000]      * |        50
##   [52]     chr1 [55001, 55100]      * |        49
##   -------
##   seqinfo: 1 sequence from an unspecified genome
```

Note that calling `averagePerBin()` without specifying ‘mcolnames’ is a convenient way to generate genomic bins covering the entire genome (and in that case the supplied GRanges doesn’t even need to contain ranges). similar to `tileGenome`.

```r
empty_gr <- GRanges(seqinfo=seqinfo(Hsapiens))
empty_gr
```

```bash
## GRanges object with 0 ranges and 0 metadata columns:
##    seqnames    ranges strand
##       <Rle> <IRanges>  <Rle>
##   -------
##   seqinfo: 93 sequences (1 circular) from hg19 genome
```

```
averagePerBin(empty_gr, 25000000)
```

```bash
## GRanges object with 205 ranges and 0 metadata columns:
##               seqnames                 ranges strand
##                  <Rle>              <IRanges>  <Rle>
##     [1]           chr1 [        1,  25000000]      *
##     [2]           chr1 [ 25000001,  50000000]      *
##     [3]           chr1 [ 50000001,  75000000]      *
##     [4]           chr1 [ 75000001, 100000000]      *
##     [5]           chr1 [100000001, 125000000]      *
##     ...            ...                    ...    ...
##   [201] chrUn_gl000245             [1, 36651]      *
##   [202] chrUn_gl000246             [1, 38154]      *
##   [203] chrUn_gl000247             [1, 36422]      *
##   [204] chrUn_gl000248             [1, 39786]      *
##   [205] chrUn_gl000249             [1, 38502]      *
##   -------
##   seqinfo: 93 sequences (1 circular) from hg19 genome
```

#### How to compute averages of a meta column from one GRanges on the other GRanges

see a [post](https://support.bioconductor.org/p/57956/)

```r
size <- 50000
windowSize <- 10

dat <- GRanges("chr1", IRanges(start=1:size, width=2), strand="+",score=sample(1:size, size))

# windows
GRwin <- GRanges("chr1", IRanges(start=(0:(size/windowSize))*windowSize, width=windowSize), strand="+")

## make a RleList object from the data
score <- coverage(dat, weight="score")
```

Then to summarize ‘score’ on your fixed-size tiling windows, you need a summarizing function like the `binnedAverage()` function shown in `?tileGenome`. `binnedAverage()` computes the average on each window but it’s easy to write a summarizing function that computes the sum:

```r
binnedSum <- function(bins, numvar, mcolname)
{
  stopifnot(is(bins, "GRanges"))
  stopifnot(is(numvar, "RleList"))
  stopifnot(identical(seqlevels(bins), names(numvar)))
  bins_per_chrom <- split(ranges(bins), seqnames(bins))
  sums_list <- lapply(names(numvar),
      function(seqname) {
          views <- Views(numvar[[seqname]],
                         bins_per_chrom[[seqname]])
          viewSums(views)
      })
  new_mcol <- unsplit(sums_list, as.factor(seqnames(bins)))
  mcols(bins)[[mcolname]] <- new_mcol
  bins
}

GRwin2 <- binnedSum(GRwin, score, "binned_score")

GRwin2
```

```bash
## GRanges object with 5001 ranges and 1 metadata column:
##          seqnames         ranges strand | binned_score
##             <Rle>      <IRanges>  <Rle> |    <integer>
##      [1]     chr1       [ 0,  9]      + |       304897
##      [2]     chr1       [10, 19]      + |       517317
##      [3]     chr1       [20, 29]      + |       377486
##      [4]     chr1       [30, 39]      + |       274838
##      [5]     chr1       [40, 49]      + |       513542
##      ...      ...            ...    ... .          ...
##   [4997]     chr1 [49960, 49969]      + |       515986
##   [4998]     chr1 [49970, 49979]      + |       521740
##   [4999]     chr1 [49980, 49989]      + |       424913
##   [5000]     chr1 [49990, 49999]      + |       514258
##   [5001]     chr1 [50000, 50009]      + |        11963
##   -------
##   seqinfo: 1 sequence from an unspecified genome; no seqlengths
```

#### turning a GRanges metadata column into RleList object.

see a [post](https://support.bioconductor.org/p/50014/)

```r
gr<- GRanges(c("chr1", "chr2"), IRanges(c(10, 50), c(16, 55)), scores= c(20, 10))
seqlengths(gr) <- c(100, 100)

coverage(gr, weight=gr$scores)
```

```bash
## RleList of length 2
## $chr1
## numeric-Rle of length 100 with 3 runs
##   Lengths:  9  7 84
##   Values :  0 20  0
## 
## $chr2
## numeric-Rle of length 100 with 3 runs
##   Lengths: 49  6 45
##   Values :  0 10  0
```

Depending on your needs, the ranges which aren’t present in the GRanges object may effectively have missing scores and need to be NA, and 0 is a valid score for the ranges which are present. One hack would be to add 1 to all of the scores, replace the zeros in the `coverage()` result with NAs and subtract 1:

```r
gr$scores <- gr$scores + 1L
cov <- coverage(gr, weight  = "scores")
cov[cov == 0L] <- NA
cov <- cov - 1L
```

Alternatively you could call `coverage()` a 2nd time with no weights to find the regions with no coverage, and set them to NA:

```r
cvg <- coverage(gr, weight=gr$scores)
cvg[coverage(gr) == 0] <- NA
```

It turns out that there are functions to convert between meta data column and RleList. Just be careful with the different behaviors of different functions.

```bash
# ?bindAsGRanges
# ?mcolAsRleList

mcolAsRleList(gr, varname = "scores")
```

```bash
## RleList of length 2
## $chr1
## numeric-Rle of length 100 with 3 runs
##   Lengths:  9  7 84
##   Values : NA 21 NA
## 
## $chr2
## numeric-Rle of length 100 with 3 runs
##   Lengths: 49  6 45
##   Values : NA 11 NA
```

```
bindAsGRanges(cvg)
```

```bash
## GRanges object with 2 ranges and 1 metadata column:
##       seqnames    ranges strand |        V1
##          <Rle> <IRanges>  <Rle> | <numeric>
##   [1]     chr1  [10, 16]      * |        21
##   [2]     chr2  [50, 55]      * |        11
##   -------
##   seqinfo: 2 sequences from an unspecified genome
```

```
bindAsGRanges(coverage(gr,weight=gr$scores))
```

```bash
## GRanges object with 6 ranges and 1 metadata column:
##       seqnames    ranges strand |        V1
##          <Rle> <IRanges>  <Rle> | <numeric>
##   [1]     chr1 [ 1,   9]      * |         0
##   [2]     chr1 [10,  16]      * |        21
##   [3]     chr1 [17, 100]      * |         0
##   [4]     chr2 [ 1,  49]      * |         0
##   [5]     chr2 [50,  55]      * |        11
##   [6]     chr2 [56, 100]      * |         0
##   -------
##   seqinfo: 2 sequences from an unspecified genome
```

```bash
# or coerce using as
as(cvg, "GRanges")
```

```bash
## GRanges object with 6 ranges and 1 metadata column:
##       seqnames    ranges strand |     score
##          <Rle> <IRanges>  <Rle> | <numeric>
##   [1]     chr1 [ 1,   9]      * |      <NA>
##   [2]     chr1 [10,  16]      * |        21
##   [3]     chr1 [17, 100]      * |      <NA>
##   [4]     chr2 [ 1,  49]      * |      <NA>
##   [5]     chr2 [50,  55]      * |        11
##   [6]     chr2 [56, 100]      * |      <NA>
##   -------
##   seqinfo: 2 sequences from an unspecified genome
```

```
as(coverage(gr, weight = gr$scores), "GRanges")
```

```bash
## GRanges object with 6 ranges and 1 metadata column:
##       seqnames    ranges strand |     score
##          <Rle> <IRanges>  <Rle> | <numeric>
##   [1]     chr1 [ 1,   9]      * |         0
##   [2]     chr1 [10,  16]      * |        21
##   [3]     chr1 [17, 100]      * |         0
##   [4]     chr2 [ 1,  49]      * |         0
##   [5]     chr2 [50,  55]      * |        11
##   [6]     chr2 [56, 100]      * |         0
##   -------
##   seqinfo: 2 sequences from an unspecified genome
```

#### subset RleList with GRanges

```
cov[gr]
```

```bash
## RleList of length 2
## $chr1
## numeric-Rle of length 7 with 1 run
##   Lengths:  7
##   Values : 20
## 
## $chr2
## numeric-Rle of length 6 with 1 run
##   Lengths:  6
##   Values : 10
```
