# Databio genomes overview

This repository contains the files to build and archive our labs's reference genome assets to serve with [`refgenieserver`](https://github.com/databio/refgenieserver) at http://refgenomes.databio.org. 

The whole process is scripted, starting from this repository. From here, we download the input data (FASTA files, GTF files etc.), use `refgenie build` to create all of these assets in a local refgenie instance, and then use `refgenieserver archive` to build the server archives, and finally serve them with a refgenieserver instance by calling `refgenieserver serve`.

# Asset PEP

The [asset_pep](asset_pep) folder contains a [PEP](https://pepkit.github.io) with metadata for each asset. The contents are:

- `assets.csv` - The primary sample_table. Each each row is an asset. 
- `recipe_inputs.csv` - The subsample_table. This provides a way to define each individual value passed to any of the 3 arguments of the `refgenie build` command: `--assets`, `--params`, and `--files`. 
- `refgenie_build_cfg.yaml` -- config file that defines a subproject (which is used to download the input data) and additional project settings.

## Building assets using this PEP

### Step 1: Download input files

Many of the assets require some input files, and we have to make sure we have those files locally. In the `recipe_inputs.csv` file, we have entered these files as remote URLs, so the first step is to download them. We have created a subproject called `getfiles` for this: To programmatically download all the files required by `refgenie build`, run from this directory using [looper](http://looper.databio.org):

```
looper run refgenie_build_cfg.yaml --compute local --sp getfiles
```

### Step 2: Build assets

Once files are present locally, we can run `refgenie build` on each asset specified in the sample_table (`assets.csv`):

```
looper run refgenie_build_cfg.yaml
```

This will create one job for each *asset*.

### Step 3. Archive assets

Assets are built locally now, but to serve them, we must archive them using `refgenieserver`. The command is simple:

```
refgenieserver archive -c <path/to/genomes.yaml>
```

Since the archivization process is generally lengthy, it makes sense to submit this job to the cluster. Since you have [divvy](http://divvy.databio.org/en/latest/) installed (with looper), you can easily create a SLURM submission script with `divvy write`:

```
divvy write -o archive_job.sbatch --code 'refgenieserver archive -c <path/to/genomes.yaml>' ...
```
for example:
```
divvy write -o archive_job.sbatch \
  --code 'refgenieserver archive -c $PROJECT/genomes_staging/genomes.yaml' \
  --mem 12000 \ 
  --cores 8 \ 
  --logfile $HOME/refgenieserver_archive.log \
  --jobname refgenieserver_archive \
  --time 01-00:00:00
```
and submit it with:
```
sbatch archive_job.sbatch
```

### Step 4. Serve assets

```
refgenieserver serve genomes.yaml
```


## Adding an asset to this PEP

### Step 1: Add the asset to the asset table.

To add an asset, you will need to add a row in `assets.csv`. Follow these directions:

- `genome` - the human-readable genome (namespace) you want to serve this asset under
- `asset` - the human-readble asset name you want to serve this asset under. It is identical to the asset recipe. Use `refgenie list` to see [available recipes](http://refgenie.databio.org/en/latest/build/)

Your asset will be retrievable from the server with `refgenie pull {genome}/{asset_name}`.

### Step 2: Add any required inputs to the recipe_inputs table

Next, we need to add the source for each item required by your recipe. You can see what the recipe requires by using `-q` or `--requirements`, like this: `refgenie build {genome}/{recipe} -q`. If your recipe doesn't require any inputs, then you're done. If it requires any inputs (which can be one or more of the following: *assets*, *files*, *parameters*), then you need to specify these in the `recipe_inputs.csv` table.

For each required input, you add a row to `recipe_inputs.csv`. Follow these directions:
- `sample_name` - must match the `genome` and `asset` value in the `assets.csv` file. Format it this way: `<genome>-<asset>`. This is how we match inputs to assets.

Next you will need to fill in 3 columns:
- `input_type` which is one of the following: *files*, *params* or *assets*
- `intput_id` must match the recipe requirement. Again, use `refgenie build <genome>/<asset> -q` to learn the ids
- `input_value` value for the input, e.g. URL in case of *files*

### Step 3: See if you did it well!

**Validate the PEP with [`eido`](http://eido.databio.org/en/latest/)**

The command below validates the PEP aginst a remote schema. Any PEP issues will result in a `ValidationError`:

```
eido -p refgenie_build_cfg.yaml -s http://schema.databio.org/refgenie/refgenie_build.yaml
```
