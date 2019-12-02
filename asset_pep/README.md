# asset PEP

Here we use a new way to do this, where each row in the primary sample_table is an asset (rather than a genome). The subsample_table provides a way to define each individual value passed to any of the 3 arguments of the `refgenie build` command: `--assets`, `--params`, and `--files`. 

**NOTE: requires `peppy` v0.22.3. Unreleased as of 2019-12-02.**

## Step 1: download input files

To programmatically download all the files required by `refgenie build`, run from this directory:

```
looper run refgenie_build_cfg.yaml --compute local --sp getfiles
```

## Step 2: build assets

To build all the assets specified in the sample_table (`assets.csv`), run:

```
looper run refgenie_build_cfg.yaml
```
