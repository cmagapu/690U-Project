# 690U-Project: Operon Prediction in *E. coli* K-12

This repo contains code and data for predicting whether consecutive protein-coding genes in *E. coli* K-12 substr. MG1655 (GCF_000005845.2_ASM584v2) form operons, using features from GC content, COG assignments, and TF‐motif scans.

## 📁 Directory structure

```

├── data/                ← raw and intermediate data (FASTA, TSVs, FIMO output…)
├── graphs/              ← final figures (PR curves, SHAP barplots…)
├── Project.ipynb        ← run this to see all code & results
├── README.md            ← this file
└── Final\_report.pdf     ← compiled report

````

## Notes about the data
> – All `GCF_000005845.2_ASM584v2` files were obtained from
> `ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/`
> RegulonDB files (TF-RISet.tsv, PromoterSet.tsv, TerminatorSet.tsv) were obtained from
> `https://regulondb.ccg.unam.mx/datasets`

## 1. COG data from eggNOG-mapper:

We use the eggNOG-mapper web server:

1. Visit [http://eggnog-mapper.embl.de/](http://eggnog-mapper.embl.de/)
2. Upload **data/GCF\_000005845.2\_ASM584v2\_protein.faa.gz** (protein FASTA)
3. Set:

   * **Sequence type**: Proteins
   * **Taxonomic scope**: auto (or Bacteria)
   * **Functional categories**: COG/KOG
4. Submit and, when finished, download the “annotations” TSV into **data/eggnog\_out/** (e.g. `MM_4125ylpq.emapper.annotations.tsv`)., which is then used to create a df that merges with pairs_df in the notebook

## 2. Scan with MEME-Suite (FIMO)

We use the Docker image `memesuite/memesuite:latest`:

```bash
docker run --rm \
  -v "$(pwd)":/home/meme \
  -w /home/meme \
  memesuite/memesuite:latest \
  fimo \
    --oc data/fimo_regdb_tf \
    --thresh 1e-4 \
    combined_pwms/regulondb_tf_pwms.meme \
    data/intergenic_regions.fasta
```

This writes **data/fimo\_regdb\_tf/fimo.tsv**, which is then merged back into `pairs_df` in the notebook.


```
```
