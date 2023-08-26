# Clade and subclade nomenclature for the HA segment of seasonal A/H3N2 influenza viruses

This repository defines clades and subclades of the hemagglutinin segment of seasonal A/H3N2 influenza viruses.
These clades don't necessarily correspond to groups of viruses with distinct phenotypes but are meant to facilitate discussion of viral genetic diversity.
In particular subclades are used to capture the frequency dynamics of co-circulating viral variants that often don't have distinct properties.


## Designations

Each subclade is defined by a machine readable `yaml` file in the subdirectory `subclades`.
The yaml-files have the following structure:
```
name: B.1.1
unaliased_name: A.2.1.1.1
parent: B.1
representatives: []
defining_mutations:
- locus: nuc
  position: 488
  state: A
- locus: HA1
  position: 134
  state: K
- locus: HA2
  position: 149
  state: E
clade: 3C.2a1a
```
The field `clade` is set to `none` when no clade corresponds to the branch demarcating the subclade.

The defining mutations are relative using nucleotide and HA coordinates defined in
```
##gff-version 3
##sequence-region CY163680.1 1 1737
CY163680.1	feature	mat_peptide	18	65	.	+	.	name="SigPep"
CY163680.1	feature	mat_peptide	66	1052	.	+	.	name="HA1"
CY163680.1	feature	mat_peptide	1053	1715	.	+	.	name="HA2"
```
Note that these defining mutations are not exhaustive. They represent a genotypic constellation sufficient to distinguish the clade from its parent.

## [Subclade summary](.auto-generated/subclades.md)

## [Clade--Subclade correspondence](.auto-generated/subclades.md#clade----subclade-correspondence)

## Subclade short names (aliases)
The subclade names are designed with a systematic way to shorten their names inspired by the pango-lineage system for SARS-CoV-2.
The initial part of the hierarchical names can be collapsed into a single letter (and possibly eventually double letters).
These aliases are defined in [config/aliases.json](config/aliases.json).


## Configuration
Parameters and mutation weights for the automated subclade suggestion algorithm can be found in the directory `config`.