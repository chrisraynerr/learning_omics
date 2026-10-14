# How to make a transcript to gene mapping file

- **Author:** Ming Tommy Tang
- **Date:** Jan 29, 2019
- **Tags:** gff, python, unix
- **Source:** [https://divingintogeneticsandgenomics.com/post/how-to-make-a-transcript-to-gene-mapping-file/](https://divingintogeneticsandgenomics.com/post/how-to-make-a-transcript-to-gene-mapping-file/)

---

I need a transcript to gene mapping file for `Salmon`. I am aware of annotation `bioconductor` packages that can do this job. However, I was working on a species which does not have the annotation in a package format (I am going to use Drosphila as an example for this blog post). I had to go and got the gtf file and made such a file from scratch.

Please read the [specifications](https://useast.ensembl.org/info/website/upload/gff.html) of those two file formats.

### Download drosophila gtf file from ENSEMBLE and gff file from NCBI

Find the `gff` file at [https://www.ncbi.nlm.nih.gov/genome/?term=drosophila+melanogaster](https://www.ncbi.nlm.nih.gov/genome/?term=drosophila+melanogaster)
Find the `gtf` file at [ftp://ftp.ensembl.org/pub/release-95/gtf/drosophila_melanogaster/](ftp://ftp.ensembl.org/pub/release-95/gtf/drosophila_melanogaster/)

```bash
#gtf file
zless -S ~/Downloads/Drosophila_melanogaster.BDGP6.95.gtf.gz | grep -v "#" | cut -f3 | sort | uniq -c
```

```bash
## 160859 CDS
##    4 Selenocysteine
## 187373 exon
## 46299 five_prime_utr
## 17737 gene
## 30492 start_codon
## 33892 three_prime_utr
## 34767 transcript
```

```bash
#gff file
zless -S ~/Downloads/GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.gff.gz| grep -v "#" | cut -f3 | sort | uniq -c
```

```bash
## 160949 CDS
##    1 RNase_MRP_RNA
##    2 RNase_P_RNA
##    2 SRP_RNA
##  584 antisense_RNA
## 187809 exon
## 17421 gene
## 2275 lnc_RNA
## 30480 mRNA
##  479 miRNA
## 5416 mobile_genetic_element
##   77 ncRNA
##  263 primary_transcript
##  308 pseudogene
##  134 rRNA
## 1870 region
##    1 sequence_feature
##   32 snRNA
##  289 snoRNA
##  319 tRNA
```

### Use unix command to make a transcripts to gene mapping file from gtf file

We see the feature types are quite different although they are both annotation files for the same species.
The `gtf` file is relatively well formatted, and we can make a transcripts to gene mapping file easily using
unix command line.

```
zless -S ~/Downloads/Drosophila_melanogaster.BDGP6.95.gtf.gz | grep -v "#" | awk '$3=="transcript"' | cut -f9 | tr -s ";" " " | awk '{print$4"\t"$2}' | sort | uniq |  sed 's/\"//g' | tee tx2gene_ensemble.tsv| head
```

```bash
## FBgn0013687  FBgn0013687
## FBtr0005088  FBgn0260439
## FBtr0006151  FBgn0000056
## FBtr0070000  FBgn0031081
## FBtr0070001  FBgn0052826
## FBtr0070002  FBgn0031085
## FBtr0070003  FBgn0062565
## FBtr0070006  FBgn0031089
## FBtr0070007  FBgn0031092
## FBtr0070008  FBgn0031094
```

hmm…why the first line has both genes in the two columns?…
sanity check:

```
zless -S ~/Downloads/Drosophila_melanogaster.BDGP6.95.gtf.gz | grep "FBgn0013687" | less -S
```

```bash
## mitochondrion_genome FlyBase gene    14917   19524   .   +   .   gene_id "FBgn0013687"; gene_name "mt:ori"; gene_source "FlyBase"; gene_biotype "pseudogene";
## mitochondrion_genome FlyBase transcript  14917   19524   .   +   .   gene_id "FBgn0013687"; transcript_id "FBgn0013687"; gene_name "mt:ori"; gene_source "FlyBase"; gene_biotype "pseudogene"; transcript_source "FlyBase"; transcript_biotype "pseudogene";
## mitochondrion_genome FlyBase exon    14917   19524   .   +   .   gene_id "FBgn0013687"; transcript_id "FBgn0013687"; exon_number "1"; gene_name "mt:ori"; gene_source "FlyBase"; gene_biotype "pseudogene"; transcript_source "FlyBase"; transcript_biotype "pseudogene"; exon_id "FBgn0013687-E1";
```

Indeed it is in the original gtf file.

### Use `gffutils`to make a transcripts to gene mapping file from gff file

The `gff` file is not that well defined. One may still be able to use some unix tricks to get the tx2gene.tsv file from a gff file, but it can be rather awkward especially for gff files from other not well annotated species. Instead, let’s use `gffutils`, a python package to do the same.

install `gffutils` in terminal:

`source activate snakemake`
`conda install gffutils`

Note, I am running python through Rsutdio/ First read how to set python path for `reticulate` at [https://rstudio.github.io/reticulate/articles/versions.html](https://rstudio.github.io/reticulate/articles/versions.html)
read more on [https://cran.r-project.org/web/packages/reticulate/vignettes/versions.html](https://cran.r-project.org/web/packages/reticulate/vignettes/versions.html)

Somehow, I have to create a `.Rprofile` in the same folder of `.Rproj` file with the following line to use my snakemake conda environment which is python3:

`Sys.setenv(PATH = paste("/anaconda3/envs/snakemake/bin/", Sys.getenv("PATH"), sep=":"))`

```r
library(reticulate)

# check which python I am using
py_discover_config()
```

```bash
## python:         /anaconda3/envs/snakemake/bin//python
## libpython:      /anaconda3/envs/snakemake/lib/libpython3.6m.dylib
## pythonhome:     /anaconda3/envs/snakemake:/anaconda3/envs/snakemake
## version:        3.6.7 |Anaconda, Inc.| (default, Oct 23 2018, 14:01:38)  [GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)]
## numpy:          /anaconda3/envs/snakemake/lib/python3.6/site-packages/numpy
## numpy_version:  1.15.3
## 
## python versions found: 
##  /anaconda3/envs/snakemake/bin//python
##  /usr/bin/python
##  /anaconda3/envs/py27/bin/python
##  /anaconda3/envs/snakemake/bin/python
```

```bash
# these did not work for me...
# use_condaenv("snakemake", required = TRUE)
# use_python("/anaconda3/envs/snakemake/bin/python")
```

```python
import sys
print(sys.version)
```

```bash
## 3.6.7 |Anaconda, Inc.| (default, Oct 23 2018, 14:05:31) 
## [GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)]
```

```python
import gffutils
import itertools
import os
os.listdir()
db = gffutils.create_db("GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.gff.gz", ":memory:", force = True,merge_strategy="merge", id_spec={'gene': 'Dbxref'})
list(db.featuretypes())
# one can do it for one type of features, say mRNA
for mRNA in itertools.islice(db.features_of_type('mRNA'), 10):
        print(mRNA['transcript_id'][0], mRNA['gene'][0])
        #print(mRNA.attributes.items())
        
## but I then have to do the same for lnc_RNA and others.        
## instead, loop over all features in the database
```

```bash
## NM_001103384.3 CG17636
## NM_001258513.2 CG17636
## NM_001258512.2 CG17636
## NM_001297796.1 RhoGAP1A
## NM_001297795.1 RhoGAP1A
## NM_001103385.2 RhoGAP1A
## NM_001103386.2 RhoGAP1A
## NM_001169155.1 RhoGAP1A
## NM_001297797.1 RhoGAP1A
## NM_001297801.1 tyn
```

```
tx_and_gene=[]
with open("tx2gene_NCBI.tsv", "w") as f:
        for feature in db.all_features():
                transcript = feature.attributes.get('transcript_id', [None])[0]
                gene = feature.attributes.get('gene', [None])[0]
                if gene and transcript and ([transcript, gene] not in tx_and_gene):
                        tx_and_gene.append([transcript, gene])
                        f.write(transcript + "\t" + gene + "\n")
```

These lines of codes are not hard to write. It takes more time to read the package documentation and understand how to use the package. One problem with bioinFORMATics is that there are so many different file formats. To make things worse, even for gff file format, many files do not follow the exact specification. You can have a taste of that at [http://daler.github.io/gffutils/examples.html](http://daler.github.io/gffutils/examples.html).
