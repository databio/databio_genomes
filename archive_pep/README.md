# Archive PEP description

This is a [PEP](https://pepkit.github.io) used for archiving our lab's reference genome assets, it has a few files:

- `archives.csv` -- the main sample table, with one row per archive.
- `refgenieserver_archive_cfg.yaml` -- config file that defines the project settings, e.g. points to the pipeline interface

Supplementary files:

- `refgenieserver_piface.yaml` -- configuration file linking the project to the `refgenieserver archive` command (or more generally: pipeline)

# Usage instructions
## Preparing to serve the assets

Once some assets are built, you might want to prepare them for serving with [`refgenieserver`](https://github.com/databio/refgenieserver) so that others can [`refgenie pull`](http://refgenie.databio.org/en/latest/pull/) them. The PEP described above will help you prepare such a servable archive for `refgenieserver`.

On Rivanna it will just require to adjust the `sample_name` column to reflect the specific name of the `genomes_*` directory, e.g.`staging` for `genomes_staging`.

Once you've done that, you can run it with `looper`:

```
looper run refgenomes_archive/refgenieserver_archive_cfg.yaml
``` 

This submits a SLURM job that creates a directory with the archived assets. Essentially `refgenieserver archive` command is called that produces a standard directory structure, which includes compressed assets (`*.tgz` files). This can be used by `refgenieserver serve`.