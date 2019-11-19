# Databio genomes overview

This repository contains necessary files to build and archive our labs's reference genome assets to serve with [`refgenieserver`](https://github.com/databio/refgenieserver) at http://refgenomes.databio.org. 

The whole process is scripted, starting from this repository. From here, we download the input data (FASTA files, GTF files etc.), use `refgenie build` to create all of these assets in a local refgenie instance, and then use `refgenieserver archive` to build the server archives, and finally serve them with a refgenieserver instance by calling `refgenieserver serve`.

Subdirectories have more information:

- [build_pep](build_pep)
- [archive_pep](archive_pep)

# How to build and serve the refgenie assets

## 1. Download the remote data

Many of the assets require some input files, and we have to make sure we have those files locally. In the `genomes.csv` file, we have entered these files as remote URLs, so the first step is to download them. We have created subprojects called `getfasta`, `getgtf`, *etc* that do these downloads. So, first run these subprojects:

```
looper run build_pep/refgenie_build_cfg.yaml --sp getfasta
looper run build_pep/refgenie_build_cfg.yaml --sp getgtf
...
```

(There are other `get...` subprojects available for the other data types). Now, all the files should be downloaded and named in the appropriate input folder. 

## 2. Build assets

Once files are present locally, we can run `refgenie build` on each genome for all assets like this:

```
looper run build_pep/refgenie_build_cfg.yaml
```

This will create one job for each genome, building all assets in order for that job.

## 3. Archive assets

Assets are built locally now, but to serve them, we must archive them using `refgenieserver`.

```
looper run refgenomes_archive/refgenieserver_archive_cfg.yaml
```

See instructions under [archive_pep](archive_pep).

## 4. Serve assets

```
refgenieserver serve genomes.yaml
```
