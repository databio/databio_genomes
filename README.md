# Databio genomes

This is a PEP for building our lab's reference genome assets with refgenie.

Since most of the assets require the `fasta` asset one needs to make sure the `fasta` asset is built before the jobs building other assets are submitted. This one approach to do it:

1. Run the `fasta` subproject first (points to `genomes_fasta.csv` instead of `genomes.csv`) to build just the `fasta` assets
	```
	looper run genomes_pep.yaml --subproject fasta
	```
1. Build the rest of the assets:
	```
	looper run genomes_pep.yaml
	```
