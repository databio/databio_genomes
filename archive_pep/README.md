# Archive PEP description


# Usage instructions
## Preparing to serve the assets

Once some assets are built, you might want to prepare them for serving with [`refgenieserver`](https://github.com/databio/refgenieserver) so that others can [`refgenie pull`](http://refgenie.databio.org/en/latest/pull/) them. This respository inclues a [genome archive PEP](https://github.com/databio/databio_genomes/tree/master/refgenomes_archive) that will help you prepare such a servable archive for `refgenieserver`.

On Rivanna it will just require to adjust the `sample_name` column to reflect the specific name of the `genomes_*` directory.

Once you've done that, you can run it with `looper`:

```
looper run refgenomes_archive/refgenieserver_archive_cfg.yaml
``` 

This will submit a SLURM job that creates a directory with the archived assets.