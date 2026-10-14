# Obtain metadata for public datasets in GEO

- **Author:** Ming Tommy Tang
- **Date:** Dec 6, 2021
- **Tags:** Bioconductor, GEO, R
- **Source:** [https://divingintogeneticsandgenomics.com/post/obtain-metadata-for-public-datasets-in-geo/](https://divingintogeneticsandgenomics.com/post/obtain-metadata-for-public-datasets-in-geo/)

---

There are so many public datasets there waiting for us to mine! It is the blessing and cursing as a computational biologist!

Metadata, or the data describing (e.g., responder or non-responder for the treatment) the data are critical in interpreting the analysis. Without metadata, your data are useless.

People usually go to [`GEO`](https://www.ncbi.nlm.nih.gov/geo/) or [`ENA`](https://www.ebi.ac.uk/ena/browser/) to download public data. I asked this question on twitter, and I will show you how to get the metadata as suggested by all the awesome tweeps. Thanks!

> 

how to download GEO metadata again? I remember there is a way to click and download a table with GSM ids and other associated metadata.

— Ming (Tommy) Tang (@tangming2005) [November 29, 2021](https://twitter.com/tangming2005/status/1465420814258479104?ref_src=twsrc%5Etfw)

### Use SRA run selector

go to [https://www.ncbi.nlm.nih.gov/Traces/study/](https://www.ncbi.nlm.nih.gov/Traces/study/) and type in the accession number

Click `Metadata` below the `Download` column, a `SraRuntable.txt` file will be downloaded.

### Use GEOquery or GEOmetadatadb

If you want to stay within `R`, take a look at [GEOmetadatadb](https://www.bioconductor.org/packages/release/bioc/html/GEOmetadb.html) and
[GEOquery](https://www.bioconductor.org/packages/release/bioc/html/GEOquery.html).

### Nextflow

A nextflow pipeline: [nf-core/fetchngs](https://nf-co.re/fetchngs#sra--ena--ddbj--geo-ids)

### Command line tool

check [ffq](https://github.com/pachterlab/ffq#fetch-information-of-one-or-more-geo-study-and-all-of-their-runs) and [pysradb](https://github.com/saketkc/pysradb)

```
pip install ffq
ffq -t GSE GSE176021

pip install pysradb
pysradb metadata SRP000002  --detailed
```

### Other resources

- [SraExplorer](https://sra-explorer.info/)

- [Recount3](http://rna.recount.bio/) summaries and queries for large-scale RNA-seq expression and splicing. paper recently published [https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02533-6](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02533-6)

- [Digital Expression Explorer 2](https://divingintogeneticsandgenomics.com/post/obtain-metadata-for-public-datasets-in-geo/dee2.io) from Mark Ziemann.

- Other databases I curated [https://github.com/crazyhottommy/RNA-seq-analysis#databases](https://github.com/crazyhottommy/RNA-seq-analysis#databases)
