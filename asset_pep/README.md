**NOTE: requires `peppy` v0.22.3. Unreleased as of 2019-12-06.**

# Asset PEP

This is a [PEP](https://pepkit.github.io) for building our lab's reference genome assets with refgenie. The contents are:

- `assets.csv` - The primary sample_table. Each each row is an asset. 
- `subassets.csv` - The subsample_table. This provides a way to define each individual value passed to any of the 3 arguments of the `refgenie build` command: `--assets`, `--params`, and `--files`. 
- `refgenie_build_cfg.yaml` -- config file that defines the subprojects (which are used to download the input data) and additional project settings.

## Building assets using this PEP

### Step 1: Download input files

To programmatically download all the files required by `refgenie build`, run from this directory:

```
looper run refgenie_build_cfg.yaml --compute local --sp getfiles
```

### Step 2: Build assets

To build all the assets specified in the sample_table (`assets.csv`), run:

```
looper run refgenie_build_cfg.yaml
```

This will create one job for each *asset*.

## Adding an asset to this PEP

### Step 1: Add the asset to the asset table.

To add an asset, you will need to add a row in `assets.csv`. Follow these directions:

- `sample_name` - just use `{genome}-{asset_name}` for now
- `genome` - the human-readable genome (namespace) you want to serve this asset under
- `asset_name` - the human-readble asset name you want to serve this asset under. It is often, but not necessarily, identical to the `asset_recipe`.
- `asset_recipe` - the unique identifier for the recipe you want to build (use `refgenie list` to see [available recipes](http://refgenie.databio.org/en/latest/build/))

Your asset will be retrievable from the server with `refgenie pull {genome}/{asset_name}`.

### Step 2: Add any required inputs to the subasset table

Next, we need to add the source for each item required by your recipe. You can see what the recipe requires by using `-q` or `--requirements`, like this: `refgenie build {genome}/{recipe} -q`. If your recipe doesn't require any inputs, then you're done. If it requires any inputs (which can be either *assets*, *files*, or *parameters*, then you need to specify these in the `subassets.csv` table.

For each required input, you add a row to `subassets.csv`. Follow these directions:
- `sample_name` - must match the row name in `assets.csv`. This is how we match inputs to assets.

Next you will need to fill in one of the 3 types, either: 
- `files_id` and `files_value`, if your recipe requires a files input. The `id` field must match the recipe requirement.
- `assets_id` and `assets_value`,tif your recipe requires assets input. The `id` field must match the recipe requirement.
- `params_id` and `params_value`, if your recipe requires a params input. The `id` field must match the recipe requirement.


