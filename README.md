# Databio genomes overview

This repository contains necessary files to build and archive our labs's reference genome assets to serve with [`refgenieserver`](https://github.com/databio/refgenieserver) at http://refgenomes.databio.org. 

The whole process is scripted, starting from this repository. From here, we download the input data (FASTA files, GTF files etc.), use `refgenie build` to create all of these assets in a local refgenie instance, and then use `refgenieserver archive` to build the server archives, and finally serve them with a refgenieserver instance by calling `refgenieserver serve`.

Please go to the respective subdirectories to learn more about building and archiving and get the PEPs that help us automate these processes:

- [archive_pep](archive_pep)
- [build_pep](build_pep)
