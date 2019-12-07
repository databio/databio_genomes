**NOTE: requires `peppy` v0.22.3. Unreleased as of 2019-12-06.**

# Asset PEP

This is a [PEP](https://pepkit.github.io) for building our lab's reference genome assets with refgenie. The contents are:

- `assets.csv` - The primary sample_table. Each each row is an asset. 
- `subassets.csv` - The subsample_table. This provides a way to define each individual value passed to any of the 3 arguments of the `refgenie build` command: `--assets`, `--params`, and `--files`. 
- `refgenie_build_cfg.yaml` -- config file that defines the subprojects (which are used to download the input data) and additional project settings.


## Step 1: Download input files

To programmatically download all the files required by `refgenie build`, run from this directory:

```
looper run refgenie_build_cfg.yaml --compute local --sp getfiles
```

## Step 2: Build assets

To build all the assets specified in the sample_table (`assets.csv`), run:

```
looper run refgenie_build_cfg.yaml
```

This will create one job for each *asset*.
