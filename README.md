# silva138.2_classifier
Date: **15-04-2025**

A prokaryotic classifier generated from Silva 138.2 database, to perform taxonomic classification in QIIME2 pipeline.

Full length 16S rRNA genes classifier

QIIME2 version: [2024.10](https://raw.githubusercontent.com/qiime2/distributions/refs/heads/dev/2024.10/amplicon/released/qiime2-amplicon-ubuntu-latest-conda.yml)  
Scikit-learn version: **1.4.2**

Compatible upto QIIME2 version [2026.1](https://raw.githubusercontent.com/qiime2/distributions/refs/heads/dev/2026.1/amplicon/released/qiime2-amplicon-ubuntu-latest-conda.yml)

## Steps
```bash
# Download data with Species info
(qiime2)$ qiime rescript get-silva-data --p-version '138.2' --p-include-species-labels --o-silva-sequences silva_138.2_ssu_nr99_rna_seqs_wSp.qza --o-silva-taxonomy silva_138.2_ssu_nr99_tax_wSp.qza

# Convert to DNA
(qiime2)$ qiime rescript reverse-transcribe --i-rna-sequences silva_138.2_ssu_nr99_rna_seqs_wSp.qza --o-dna-sequences silva_138.2_ssu_nr99_seqs_wSp.qza

# Remove low quality seqs
(qiime2)$ qiime rescript cull-seqs --i-sequences silva_138.2_ssu_nr99_seqs_wSp.qza --o-clean-sequences silva_138.2_ssu_nr99_seqs_wSp_cleaned.qza

# Discard seqs with less bp
(qiime2)$ qiime rescript filter-seqs-length-by-taxon --i-sequences silva_138.2_ssu_nr99_seqs_wSp_cleaned.qza --i-taxonomy silva_138.2_ssu_nr99_tax_wSp.qza --p-labels Archaea Bacteria Eukaryota --p-min-lens 900 1200 1400 --o-filtered-seqs silva_138.2_ssu_nr99_seqs_wSp_filt.qza --o-discarded-seqs silva_138.2_ssu_nr99_seqs_wSp_discard.qza

# Remove Eukaryotes (optional)
(qiime2)$ qiime taxa filter-seqs --i-sequences silva_138.2_ssu_nr99_seqs_wSp_filt.qza --i-taxonomy silva_138.2_ssu_nr99_tax_wSp.qza --p-exclude 'd__Eukaryota' --p-mode 'contains' --o-filtered-sequences silva_138.2_ssu_nr99_seqs_wSp_noEuk.qza

# Dereplicate noEuk seqs
(qiime2)$ qiime rescript dereplicate --i-sequences silva_138.2_ssu_nr99_seqs_wSp_noEuk.qza --i-taxa silva_138.2_ssu_nr99_tax_wSp.qza --p-mode 'uniq' --o-dereplicated-sequences silva_138.2_ssu_nr99_seqs_wSp_noEuk_uniq.qza --o-dereplicated-taxa silva_138.2_ssu_nr99_tax_wSp_noEuk_uniq.qza --p-threads 24

# noEuk classifier
(qiime2)$ qiime rescript evaluate-fit-classifier --i-sequences silva_138.2_ssu_nr99_seqs_wSp_noEuk_uniq.qza --i-taxonomy silva_138.2_ssu_nr99_tax_wSp_noEuk_uniq.qza --o-classifier silva_138.2_ssu_nr99_wSp_noEuk_classifier.qza --o-observed-taxonomy silva_138.2_ssu_nr99_wSp_noEuk_predicted_taxonomy.qza --o-evaluation silva_138.2_ssu_nr99_wSp_noEuk_classifier_eval.qzv --p-n-jobs 24
# Took nearly 2.30 hrs
```

[Downloads directory](https://mega.nz/folder/gO5gAZIK#cTAwUqvAdnfoHzvYINSU-g)

[**Classifier**](https://mega.nz/file/gfAhVJCC#ChyqPmJNg55yQ4pE6RBLBFNJaFG_-1mf9K_VkLlIjqs)

[Dropbox](https://www.dropbox.com/scl/fo/fryg5ak0dhu17njt4ivxy/ACFlCCJ8RFoKApBvFrZIby0?rlkey=bmzf34qe0on4xsr0t5k2omd5k&st=xss3uaz1&dl=0)


## System configuration

- RAM: 250 GB
- Processor: AMD EPYC 7452 (Dual CPUs)
- Core/Threads: 32/64 x 2: 128 threads
- OS: Ubuntu 24.04.02 LTS

## Other classifiers

- Older version - [silva 138](https://github.com/anw-sh/silva-138_classifiers) - using [scikit-learn **0.24.1**](https://raw.githubusercontent.com/qiime2/environment-files/master/2021.4/release/qiime2-2021.4-py38-linux-conda.yml)
- [NCBI](https://github.com/anw-sh/ncbi_classifier)

## References

1. [RESCRIPt tutorial](https://forum.qiime2.org/t/processing-filtering-and-evaluating-the-silva-database-and-other-reference-sequence-data-with-rescript/15494)
2. [silva](https://www.arb-silva.de/)
3. [Classifiers by silva](https://www.arb-silva.de/current-release/QIIME2)
4. [QIIME2 resources](https://library.qiime2.org/data-resources)