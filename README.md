# seasonal_A-H3N2_HA
Clade and subclade nomenclature for the HA segment of seasonal A/H3N2 influenza viruses.

The defining mutations are relative using nucleotide and HA coordinates defined in
```
##gff-version 3
##sequence-region CY163680.1 1 1737
CY163680.1	feature	gene	18	65	.	+	.	gene_name="SigPep"
CY163680.1	feature	gene	66	1052	.	+	.	gene_name="HA1"
CY163680.1	feature	gene	1053	1715	.	+	.	gene_name="HA2"
```

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


## [Subclade summary](.auto-generated/subclades.md)

## [Clade--Subclade correspondence](.auto-generated/subclades.md#clade----subclade-correspondence)

## Subclade short names (aliases)
The subclade names are designed with a systematic way to shorten their names inspired by the pango-lineage system for SARS-CoV-2.
The initial part of the hierarchical names can be collapsed into a single letter (and possibly eventually double letters).
These aliases are defined in [aliases.json](aliases.json).



