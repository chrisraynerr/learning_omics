# How to create a GenomicRanges object in Bioconductor using canonical transcripts

- **Author:** Ming Tommy Tang
- **Date:** Jul 16, 2025
- **Tags:** Bioconductor, annotation, bioinformatics
- **Source:** [https://divingintogeneticsandgenomics.com/post/canonical-tx/](https://divingintogeneticsandgenomics.com/post/canonical-tx/)

---

**To not miss a post like this, sign up for my [newsletter](https://divingintogeneticsandgenomics.ck.page/profile) to learn computational
biology and bioinformatics.**

## Introduction to Annotation Data Packages in Bioconductor

Accurate gene and transcript annotation is the foundation of many bioinformatics workflows, including RNA-seq analysis, functional genomics, and variant annotation.

In the R/Bioconductor ecosystem, dedicated annotation data packages make it easy for researchers to access, query, and leverage gene models sourced from major biological databases.

Understanding the origin and structure of these annotation packages—such as those based on [UCSC](https://genome.ucsc.edu/), [Ensembl](https://useast.ensembl.org/), and other reference sets—is essential for reproducibility and clarity in your analyses.

### Key Annotation Databases and Their Packages

- 

**TxDb.Hsapiens.UCSC.hg38.knownGene**
Built using gene models from the UCSC Genome Browser’s “knownGene” track for the human hg38 assembly. `UCSC` integrates gene predictions from several sources, including RefSeq, GenBank, and Ensembl, but curates its own transcript IDs and structures.

- 

**EnsDb Data Packages**
Based on Ensembl, a comprehensive genome annotation resource. EnsDb packages (created via the `ensembldb` package) are derived directly from Ensembl’s annotation of genes, transcripts, and proteins for specific genome builds. They reflect Ensembl’s standardized data model and genomic coordinates.

### Major Transcript Reference Systems

Different annotation resources define transcripts differently, and “canonical” transcripts can refer to the preferred or most representative isoforms per gene. Here are some prominent systems:

**UCSC (knownGene)**

**Ensembl**

**RefSeq**

**MANE**

| Reference System | Description | Canonical Selection |
| --- | --- | --- |
|  | Aggregates gene predictions, uses its own transcript IDs; many overlaps with RefSeq/Ensembl | UCSC-specific rules |
|  | Own annotation pipeline; covers standard and alternative transcripts, updated regularly | Ensembl canonical |
|  | Curated by NCBI; focuses on high-confidence, experimentally validated transcripts | RefSeq Select |
|  | “Matched Annotation from NCBI and EMBL-EBI”; a collaboration to produce a single transcript per protein-coding gene, identical in both RefSeq and Ensembl | MANE Select |

#### Additional Notes

- **RefSeq** annotations are highly curated, with a focus on stability and biological accuracy.

- **MANE** transcripts provide a harmonized “best” transcript for each gene, facilitating cross-platform comparisons and clinical reporting.

- **Canonical transcripts** in Ensembl and UCSC are assigned based on expression, conservation, clinical relevance, or other attributes; the computational definitions may differ.

### Why These Differences Matter

Selecting the appropriate annotation resource in your analysis affects:

- **Transcript/gene definitions**: Variations in exon boundaries, IDs, and number of isoforms.

- **Reproducibility**: Results may change depending on annotation source/version.

- **Interpretation**: Clinical applications and downstream analyses (e.g., variant annotation) can be impacted by transcript choice.

In practice, choosing the right isoform of the mRNA can matter a lot for annotating your variants, the protein amino acid changes can be different based on the isoforms that are selected.

When annotating peaks for ChIP-seq data. Peaks have different distances among different isoforms.

### Create a TxDb object using canonical transcripts only

```r
library(GenomicRanges)
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
library(AnnotationDbi)
library(org.Hs.eg.db)
library(AnnotationHub)
library(ensembldb)
# load hg38 transcript
# directly filter canonical transcripts from the Ensembl database
ah<- AnnotationHub(localHub = FALSE)
edb<- ah[["AH98047"]]

edb
```

```bash
#> EnsDb for Ensembl:
#> |Backend: SQLite
#> |Db type: EnsDb
#> |Type of Gene ID: Ensembl Gene ID
#> |Supporting package: ensembldb
#> |Db created by: ensembldb package from Bioconductor
#> |script_version: 0.3.7
#> |Creation time: Sat Dec 18 14:48:15 2021
#> |ensembl_version: 105
#> |ensembl_host: localhost
#> |Organism: Homo sapiens
#> |taxonomy_id: 9606
#> |genome_build: GRCh38
#> |DBSCHEMAVERSION: 2.2
#> | No. of genes: 69329.
#> | No. of transcripts: 268255.
#> |Protein data available.
```

```
transcripts(edb)
```

```bash
#> GRanges object with 268255 ranges and 11 metadata columns:
#>                   seqnames            ranges strand |           tx_id
#>                      <Rle>         <IRanges>  <Rle> |     <character>
#>   ENST00000456328        1       11869-14409      + | ENST00000456328
#>   ENST00000450305        1       12010-13670      + | ENST00000450305
#>   ENST00000488147        1       14404-29570      - | ENST00000488147
#>   ENST00000619216        1       17369-17436      - | ENST00000619216
#>   ENST00000473358        1       29554-31097      + | ENST00000473358
#>               ...      ...               ...    ... .             ...
#>   ENST00000420810        Y 26549425-26549743      + | ENST00000420810
#>   ENST00000456738        Y 26586642-26591601      - | ENST00000456738
#>   ENST00000435945        Y 26594851-26634652      - | ENST00000435945
#>   ENST00000435741        Y 26626520-26627159      - | ENST00000435741
#>   ENST00000431853        Y 56855244-56855488      + | ENST00000431853
#>                               tx_biotype tx_cds_seq_start tx_cds_seq_end
#>                              <character>        <integer>      <integer>
#>   ENST00000456328   processed_transcript             <NA>           <NA>
#>   ENST00000450305 transcribed_unproces..             <NA>           <NA>
#>   ENST00000488147 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000619216                  miRNA             <NA>           <NA>
#>   ENST00000473358                 lncRNA             <NA>           <NA>
#>               ...                    ...              ...            ...
#>   ENST00000420810   processed_pseudogene             <NA>           <NA>
#>   ENST00000456738 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000435945 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000435741   processed_pseudogene             <NA>           <NA>
#>   ENST00000431853   processed_pseudogene             <NA>           <NA>
#>                           gene_id tx_support_level     tx_id_version gc_content
#>                       <character>        <integer>       <character>  <numeric>
#>   ENST00000456328 ENSG00000223972                1 ENST00000456328.2    55.3410
#>   ENST00000450305 ENSG00000223972             <NA> ENST00000450305.2    58.0696
#>   ENST00000488147 ENSG00000227232             <NA> ENST00000488147.1    61.1399
#>   ENST00000619216 ENSG00000278267             <NA> ENST00000619216.1    61.7647
#>   ENST00000473358 ENSG00000243485                5 ENST00000473358.1    58.5674
#>               ...             ...              ...               ...        ...
#>   ENST00000420810 ENSG00000224240             <NA> ENST00000420810.1    41.6928
#>   ENST00000456738 ENSG00000227629             <NA> ENST00000456738.1    44.7368
#>   ENST00000435945 ENSG00000237917             <NA> ENST00000435945.1    44.2875
#>   ENST00000435741 ENSG00000231514             <NA> ENST00000435741.1    55.1562
#>   ENST00000431853 ENSG00000235857             <NA> ENST00000431853.1    53.0612
#>                   tx_external_name tx_is_canonical         tx_name
#>                        <character>       <integer>     <character>
#>   ENST00000456328      DDX11L1-202               0 ENST00000456328
#>   ENST00000450305      DDX11L1-201               1 ENST00000450305
#>   ENST00000488147       WASH7P-201               1 ENST00000488147
#>   ENST00000619216    MIR6859-1-201               1 ENST00000619216
#>   ENST00000473358  MIR1302-2HG-202               1 ENST00000473358
#>               ...              ...             ...             ...
#>   ENST00000420810      CYCSP49-201               1 ENST00000420810
#>   ENST00000456738   SLC25A15P1-201               1 ENST00000456738
#>   ENST00000435945      PARP4P1-201               1 ENST00000435945
#>   ENST00000435741       CCNQP2-201               1 ENST00000435741
#>   ENST00000431853      CTBP2P1-201               1 ENST00000431853
#>   -------
#>   seqinfo: 456 sequences (1 circular) from GRCh38 genome
```

There is a metadata column called `tx_is_canonical` to show if the transcript is canonical or not.

```r
# Get transcripts directly from the EnsDb object with canonical filter
hg38_transcripts<- transcripts(edb, filter = ~ tx_is_canonical == TRUE)

# only the canonical transcripts are retained. tx_is_canonical are all 1s
hg38_transcripts
```

```bash
#> GRanges object with 69329 ranges and 11 metadata columns:
#>                   seqnames            ranges strand |           tx_id
#>                      <Rle>         <IRanges>  <Rle> |     <character>
#>   ENST00000450305        1       12010-13670      + | ENST00000450305
#>   ENST00000488147        1       14404-29570      - | ENST00000488147
#>   ENST00000619216        1       17369-17436      - | ENST00000619216
#>   ENST00000473358        1       29554-31097      + | ENST00000473358
#>   ENST00000607096        1       30366-30503      + | ENST00000607096
#>               ...      ...               ...    ... .             ...
#>   ENST00000420810        Y 26549425-26549743      + | ENST00000420810
#>   ENST00000456738        Y 26586642-26591601      - | ENST00000456738
#>   ENST00000435945        Y 26594851-26634652      - | ENST00000435945
#>   ENST00000435741        Y 26626520-26627159      - | ENST00000435741
#>   ENST00000431853        Y 56855244-56855488      + | ENST00000431853
#>                               tx_biotype tx_cds_seq_start tx_cds_seq_end
#>                              <character>        <integer>      <integer>
#>   ENST00000450305 transcribed_unproces..             <NA>           <NA>
#>   ENST00000488147 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000619216                  miRNA             <NA>           <NA>
#>   ENST00000473358                 lncRNA             <NA>           <NA>
#>   ENST00000607096                  miRNA             <NA>           <NA>
#>               ...                    ...              ...            ...
#>   ENST00000420810   processed_pseudogene             <NA>           <NA>
#>   ENST00000456738 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000435945 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000435741   processed_pseudogene             <NA>           <NA>
#>   ENST00000431853   processed_pseudogene             <NA>           <NA>
#>                           gene_id tx_support_level     tx_id_version gc_content
#>                       <character>        <integer>       <character>  <numeric>
#>   ENST00000450305 ENSG00000223972             <NA> ENST00000450305.2    58.0696
#>   ENST00000488147 ENSG00000227232             <NA> ENST00000488147.1    61.1399
#>   ENST00000619216 ENSG00000278267             <NA> ENST00000619216.1    61.7647
#>   ENST00000473358 ENSG00000243485                5 ENST00000473358.1    58.5674
#>   ENST00000607096 ENSG00000284332             <NA> ENST00000607096.1    31.1594
#>               ...             ...              ...               ...        ...
#>   ENST00000420810 ENSG00000224240             <NA> ENST00000420810.1    41.6928
#>   ENST00000456738 ENSG00000227629             <NA> ENST00000456738.1    44.7368
#>   ENST00000435945 ENSG00000237917             <NA> ENST00000435945.1    44.2875
#>   ENST00000435741 ENSG00000231514             <NA> ENST00000435741.1    55.1562
#>   ENST00000431853 ENSG00000235857             <NA> ENST00000431853.1    53.0612
#>                   tx_external_name tx_is_canonical         tx_name
#>                        <character>       <integer>     <character>
#>   ENST00000450305      DDX11L1-201               1 ENST00000450305
#>   ENST00000488147       WASH7P-201               1 ENST00000488147
#>   ENST00000619216    MIR6859-1-201               1 ENST00000619216
#>   ENST00000473358  MIR1302-2HG-202               1 ENST00000473358
#>   ENST00000607096    MIR1302-2-201               1 ENST00000607096
#>               ...              ...             ...             ...
#>   ENST00000420810      CYCSP49-201               1 ENST00000420810
#>   ENST00000456738   SLC25A15P1-201               1 ENST00000456738
#>   ENST00000435945      PARP4P1-201               1 ENST00000435945
#>   ENST00000435741       CCNQP2-201               1 ENST00000435741
#>   ENST00000431853      CTBP2P1-201               1 ENST00000431853
#>   -------
#>   seqinfo: 456 sequences (1 circular) from GRCh38 genome
```

There are unconventional chromosome names such as LRG_239,

```
seqnames(hg38_transcripts)
```

```bash
#> factor-Rle of length 69329 with 456 runs
#>   Lengths:                       5702 ...                        522
#>   Values : 1                          ...         Y                 
#> Levels(456): 1 10 11 12 13 14 15 16 ... LRG_763 LRG_792 LRG_793 LRG_93 MT X Y
```

```r
# only keep the standard names
hg38_transcripts<- hg38_transcripts %>%
  keepStandardChromosomes(pruning.mode = "coarse")

seqnames(hg38_transcripts)
```

```bash
#> factor-Rle of length 62800 with 25 runs
#>   Lengths: 5702 2429 3488 3152 1448 2358 2295 ... 3132 2554 2417   37 2522  522
#>   Values :   1    10   11   12   13   14   15 ...   7    8    9    MT   X    Y 
#> Levels(25): 1 10 11 12 13 14 15 16 17 18 19 2 20 21 22 3 4 5 6 7 8 9 MT X Y
```

rename chromosome from 1,2,3 to chr1, chr2, chr3 etc
so it can overlap with the peaks if your peaks are in chr1 start end format.

```
seqlevels(hg38_transcripts)
```

```bash
#>  [1] "1"  "10" "11" "12" "13" "14" "15" "16" "17" "18" "19" "2"  "20" "21" "22"
#> [16] "3"  "4"  "5"  "6"  "7"  "8"  "9"  "MT" "X"  "Y"
```

```r
# prefix chr
new_seqnames <- paste0("chr", seqlevels(hg38_transcripts))
names(new_seqnames) <- seqlevels(hg38_transcripts)

new_seqnames
```

```bash
#>       1      10      11      12      13      14      15      16      17      18 
#>  "chr1" "chr10" "chr11" "chr12" "chr13" "chr14" "chr15" "chr16" "chr17" "chr18" 
#>      19       2      20      21      22       3       4       5       6       7 
#> "chr19"  "chr2" "chr20" "chr21" "chr22"  "chr3"  "chr4"  "chr5"  "chr6"  "chr7" 
#>       8       9      MT       X       Y 
#>  "chr8"  "chr9" "chrMT"  "chrX"  "chrY"
```

```r
hg38_transcripts <- renameSeqlevels(hg38_transcripts, new_seqnames)

# add the gene symbol using database mapping for better accuracy
hg38_transcripts$SYMBOL <- mapIds(edb,
                                 keys = hg38_transcripts$tx_id,
                                 column = "SYMBOL",
                                 keytype = "TXID")

hg38_transcripts
```

```bash
#> GRanges object with 62800 ranges and 12 metadata columns:
#>                   seqnames            ranges strand |           tx_id
#>                      <Rle>         <IRanges>  <Rle> |     <character>
#>   ENST00000450305     chr1       12010-13670      + | ENST00000450305
#>   ENST00000488147     chr1       14404-29570      - | ENST00000488147
#>   ENST00000619216     chr1       17369-17436      - | ENST00000619216
#>   ENST00000473358     chr1       29554-31097      + | ENST00000473358
#>   ENST00000607096     chr1       30366-30503      + | ENST00000607096
#>               ...      ...               ...    ... .             ...
#>   ENST00000420810     chrY 26549425-26549743      + | ENST00000420810
#>   ENST00000456738     chrY 26586642-26591601      - | ENST00000456738
#>   ENST00000435945     chrY 26594851-26634652      - | ENST00000435945
#>   ENST00000435741     chrY 26626520-26627159      - | ENST00000435741
#>   ENST00000431853     chrY 56855244-56855488      + | ENST00000431853
#>                               tx_biotype tx_cds_seq_start tx_cds_seq_end
#>                              <character>        <integer>      <integer>
#>   ENST00000450305 transcribed_unproces..             <NA>           <NA>
#>   ENST00000488147 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000619216                  miRNA             <NA>           <NA>
#>   ENST00000473358                 lncRNA             <NA>           <NA>
#>   ENST00000607096                  miRNA             <NA>           <NA>
#>               ...                    ...              ...            ...
#>   ENST00000420810   processed_pseudogene             <NA>           <NA>
#>   ENST00000456738 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000435945 unprocessed_pseudogene             <NA>           <NA>
#>   ENST00000435741   processed_pseudogene             <NA>           <NA>
#>   ENST00000431853   processed_pseudogene             <NA>           <NA>
#>                           gene_id tx_support_level     tx_id_version gc_content
#>                       <character>        <integer>       <character>  <numeric>
#>   ENST00000450305 ENSG00000223972             <NA> ENST00000450305.2    58.0696
#>   ENST00000488147 ENSG00000227232             <NA> ENST00000488147.1    61.1399
#>   ENST00000619216 ENSG00000278267             <NA> ENST00000619216.1    61.7647
#>   ENST00000473358 ENSG00000243485                5 ENST00000473358.1    58.5674
#>   ENST00000607096 ENSG00000284332             <NA> ENST00000607096.1    31.1594
#>               ...             ...              ...               ...        ...
#>   ENST00000420810 ENSG00000224240             <NA> ENST00000420810.1    41.6928
#>   ENST00000456738 ENSG00000227629             <NA> ENST00000456738.1    44.7368
#>   ENST00000435945 ENSG00000237917             <NA> ENST00000435945.1    44.2875
#>   ENST00000435741 ENSG00000231514             <NA> ENST00000435741.1    55.1562
#>   ENST00000431853 ENSG00000235857             <NA> ENST00000431853.1    53.0612
#>                   tx_external_name tx_is_canonical         tx_name      SYMBOL
#>                        <character>       <integer>     <character> <character>
#>   ENST00000450305      DDX11L1-201               1 ENST00000450305     DDX11L1
#>   ENST00000488147       WASH7P-201               1 ENST00000488147      WASH7P
#>   ENST00000619216    MIR6859-1-201               1 ENST00000619216   MIR6859-1
#>   ENST00000473358  MIR1302-2HG-202               1 ENST00000473358 MIR1302-2HG
#>   ENST00000607096    MIR1302-2-201               1 ENST00000607096   MIR1302-2
#>               ...              ...             ...             ...         ...
#>   ENST00000420810      CYCSP49-201               1 ENST00000420810     CYCSP49
#>   ENST00000456738   SLC25A15P1-201               1 ENST00000456738  SLC25A15P1
#>   ENST00000435945      PARP4P1-201               1 ENST00000435945     PARP4P1
#>   ENST00000435741       CCNQP2-201               1 ENST00000435741      CCNQP2
#>   ENST00000431853      CTBP2P1-201               1 ENST00000431853     CTBP2P1
#>   -------
#>   seqinfo: 25 sequences (1 circular) from GRCh38 genome
```

Now we have a `GenomicRanges` object with only the canonical transcripts!! You can then use it to annotate your ChIP-seq peaks for example.

In my next blog post, I am going to show you how to calculate [regulatory potential score](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1934-6) using ChIP-seq peaks and this canonical transcripts annotation.

Let’s take a look at the UCSC annotation.

```r
txdb<- TxDb.Hsapiens.UCSC.hg38.knownGene

UCSC_transcripts<- transcripts(txdb)
UCSC_transcripts
```

```bash
#> GRanges object with 276905 ranges and 2 metadata columns:
#>                       seqnames        ranges strand |     tx_id
#>                          <Rle>     <IRanges>  <Rle> | <integer>
#>        [1]                chr1   11869-14409      + |         1
#>        [2]                chr1   12010-13670      + |         2
#>        [3]                chr1   29554-31097      + |         3
#>        [4]                chr1   30267-31109      + |         4
#>        [5]                chr1   30366-30503      + |         5
#>        ...                 ...           ...    ... .       ...
#>   [276901] chrX_MU273397v1_alt 239036-260095      - |    276901
#>   [276902] chrX_MU273397v1_alt 272358-282686      - |    276902
#>   [276903] chrX_MU273397v1_alt 314193-316302      - |    276903
#>   [276904] chrX_MU273397v1_alt 314813-315236      - |    276904
#>   [276905] chrX_MU273397v1_alt 324527-324923      - |    276905
#>                      tx_name
#>                  <character>
#>        [1] ENST00000456328.2
#>        [2] ENST00000450305.2
#>        [3] ENST00000473358.1
#>        [4] ENST00000469289.1
#>        [5] ENST00000607096.1
#>        ...               ...
#>   [276901] ENST00000710260.1
#>   [276902] ENST00000710028.1
#>   [276903] ENST00000710030.1
#>   [276904] ENST00000710216.1
#>   [276905] ENST00000710031.1
#>   -------
#>   seqinfo: 711 sequences (1 circular) from hg38 genome
```

```
head(seqlevels(UCSC_transcripts), n = 30)
```

```bash
#>  [1] "chr1"                "chr2"                "chr3"               
#>  [4] "chr4"                "chr5"                "chr6"               
#>  [7] "chr7"                "chr8"                "chr9"               
#> [10] "chr10"               "chr11"               "chr12"              
#> [13] "chr13"               "chr14"               "chr15"              
#> [16] "chr16"               "chr17"               "chr18"              
#> [19] "chr19"               "chr20"               "chr21"              
#> [22] "chr22"               "chrX"                "chrY"               
#> [25] "chrM"                "chr1_GL383518v1_alt" "chr1_GL383519v1_alt"
#> [28] "chr1_GL383520v2_alt" "chr1_KI270759v1_alt" "chr1_KI270760v1_alt"
```

we see strange chromosome names such as `chr1_GL383518v1_alt`.

The human reference assembly includes not just the main chromosomes (e.g., chr1, chr2…) but also alternate loci and fix patches.

“alt” contigs cover areas of the genome where population-level structural variation or highly polymorphic regions cannot be represented by a single, linear reference.

Each name describes: The primary chromosome (chr1)

The unique contig identifier (GL383518v1, KI270759v1, etc.; these are GenBank accession numbers with version)

The “_alt” suffix, indicating an alternate locus or haplotype scaffold.

Also, this transcripts annotation does not have gene symbols and you may need to map it using `org.Hs.eg.db` package.

get all the genes representation

```
genes(txdb)
```

```bash
#> GRanges object with 30733 ranges and 1 metadata column:
#>             seqnames              ranges strand |     gene_id
#>                <Rle>           <IRanges>  <Rle> | <character>
#>           1    chr19   58345178-58362751      - |           1
#>          10     chr8   18386311-18401218      + |          10
#>         100    chr20   44584896-44652252      - |         100
#>        1000    chr18   27932879-28177946      - |        1000
#>   100008586     chrX   49551278-49568218      + |   100008586
#>         ...      ...                 ...    ... .         ...
#>        9990    chr15   34229784-34338060      - |        9990
#>        9991     chr9 112217716-112333664      - |        9991
#>        9992    chr21   34364006-34371381      + |        9992
#>        9993    chr22   19036282-19122454      - |        9993
#>        9997    chr22   50523568-50526461      - |        9997
#>   -------
#>   seqinfo: 711 sequences (1 circular) from hg38 genome
```

Note this function collapse all the transcripts from the same gene into a single GenomicRanges. This may or may not be what you want. It effectively getting the longest isoform of that gene.

### Conclusion

When building workflows in Bioconductor, it’s crucial to know the origins and intentions behind annotation data packages. Whether you rely on UCSC, Ensembl, RefSeq, or MANE, your chosen dataset will shape how genes and transcripts are referenced throughout your research. Always specify your annotation sources for transparent, reproducible results.
