# Databio genomes

## Metadata description

This is a [PEP](https://pepkit.github.io) for building our lab's reference genome assets with refgenie. The PEP has a few files:

- `genomes.csv` -- the main sample table, with one row per genome.
- `assets.csv` -- the subsample table, with one row per asset, with each one tied to a row from the genomes.csv sample table.

To build a new asset, just add a row in the `assets.csv` file. Make sure to include any input files as columns in the main `genomes.csv` file.

## Downloading the remote data

Many of the assets require some input files, and we have to make sure we have those files locally. In the `genomes.csv` file, we have entered these files as remote URLs, so the first step is to download them. We have created subprojects called `getfasta` and `getgtf` that do these downloads. So, first run these subprojects:

```
looper run genomes_pep.yaml --sp getfasta
looper run genomes_pep.yaml --sp getgtf
```
Now, all the files should be downloaded and named in the appropriate input folder. 

## Building assets

Once files are present locally, we can run `refgenie build` on each genome for all assets like this:

```
looper run genomes_pep.yaml
```

This will create one job for each genome, building all assets in order for that job.
