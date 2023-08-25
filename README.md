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
subclade: B.1.1
clade: 3C.2a1a
defining_mutations:
- HA1:134K
- HA2:149E
- nuc:488A
parent: B.1
representatives: []
unaliased_subclade: A.2.1.1.1
```
The field `clade` is set to `none` when no clade corresponds to the branch demarcating the subclade.


## [Subclade summary](.auto-generated/subclades.md)
