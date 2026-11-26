# Clade and subclade nomenclature for the HA segment of seasonal A/H3N2 influenza viruses

This repository defines clades and subclades of the hemagglutinin segment of seasonal A/H3N2 influenza viruses.
These clades don't necessarily correspond to groups of viruses with distinct phenotypes but are meant to facilitate discussion of viral genetic diversity.
In particular subclades are used to capture the frequency dynamics of co-circulating viral variants that often don't have distinct properties.

## Citation
The nomenclature system is described in the following paper

[**Nomenclature for Tracking of Genetic Variation of Seasonal Influenza Viruses**](https://dx.doi.org/10.1111/irv.70230)

_Neher et al. Influenza and Other Respiratory Viruses, vol 20(2), 2026_

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
The reference sequence and annotation are available in [config/reference.fasta](config/reference.fasta) and [config/annotation.gff](config/annotation.gff).

## Founder sequences and representatives
In addition to the defining mutations, each subclade yaml carries a `founder` block that specifies the full nucleotide sequence of the founder of the subclade as differences to the founder of its parent (or to the reference for the root):
```
founder:
  method: representative-consensus
  provenance: "consensus of A/WesternAustralia/2534415989/2025, A/Georgia/38/2026"
  mutations:
  - {position: 300, state: 'A'}
```
Positions are 1-based nucleotide positions in the reference.
Founders of new subclades are computed from at least two representative sequences: a position differs from the parent founder only if all representatives with coverage at this position agree on a state different from the parent founder.
Representatives should therefore be well-covered sequences that sample the basal diversity of the subclade.
Positions a representative does not cover are ignored, as are gaps beyond the ends of its aligned range, so poorly covered representatives weaken a founder rather than corrupting it.
When fewer than two representatives cover a position, the founder keeps the state of its parent.
Founders seeded from ancestral sequence reconstruction are marked with `method: ancestral-inference`.
Every founder is checked to carry its own defining mutations; `assemble_founders.py --strict` fails if it does not.
The assembled founder sequences and the list of representatives are available in [.auto-generated/founders.fasta](.auto-generated/founders.fasta) and [.auto-generated/representatives.tsv](.auto-generated/representatives.tsv).

## [Subclade summary](.auto-generated/subclades.md)

## [Clade--Subclade correspondence](.auto-generated/subclades.md#clade----subclade-correspondence)

## Subclade short names (aliases)
The subclade names are designed with a systematic way to shorten their names inspired by the pango-lineage system for SARS-CoV-2.
The initial part of the hierarchical names can be collapsed into a single letter (and possibly eventually double letters).
These aliases are defined in [config/aliases.json](config/aliases.json).


## Configuration
Parameters and mutation weights for the automated subclade suggestion algorithm can be found in the directory `config`.

## Adding a new subclade
Detailed instructions are in the [helper-scripts repository](https://github.com/influenza-clade-nomenclature/helper-scripts#adding-a-new-lineage). In short:
1. Add `subclades/<name>.yml` with `name`, `unaliased_name`, `parent`, `defining_mutations`, `clade`, and at least two (better three or more) `representatives`: well-covered sequences that sample the basal diversity of the new subclade. New letter aliases also need `alias_of` and an entry in [config/aliases.json](config/aliases.json).
2. Compute the founder from the representatives, e.g. using the sequences of a [nextstrain/seasonal-flu](https://github.com/nextstrain/seasonal-flu) build (strain names without spaces). Check the report with `--dry-run`, replace representatives that are missing, poorly covered, or inconsistent with the defining mutations, then rerun without `--dry-run` to write the `founder` block:
   ```
   python ../helper-scripts/compute_founders.py --input-dir subclades --reference config/reference.fasta --annotation config/annotation.gff \
          --all-sequences ../../seasonal-flu/data/h3n2/ha.fasta --lineages <new subclades> --dry-run
   ```
   This requires `seqkit` and `nextclade`.
3. Validate the founders and update the auto-generated files as described below, then commit the new yaml together with the auto-generated files.

## Update auto-generated files
After adding new clades, run the following commands to update the files generated from the individual yamls.
```
# generate the markdown summary of the clade definitions
python ../helper-scripts/generate_markdown_summary.py --input-dir subclades --lineage h3n2 --segment ha

# generate the tsv file with clade-defining info that can be used to annotate clades in augur
python ../helper-scripts/construct_tsv.py --input-dir subclades --add-unassigned-root  \
       --output-tsv .auto-generated/subclades.tsv --output-alias-tsv .auto-generated/subclade-summary.tsv --output-newick .auto-generated/subclade.nwk
python ../helper-scripts/construct_tsv.py --input-dir subclades subclade-proposals --output-tsv .auto-generated/subclade-proposals.tsv
python ../helper-scripts/construct_tsv.py --input-dir clades --aux-input-dir subclades --flat-output --use-short-name --output-tsv .auto-generated/clades.tsv
python ../helper-scripts/construct_tsv.py --input-dir clades --aux-input-dir subclades --flat-output --output-tsv .auto-generated/clades-long.tsv

# compute founders of new subclades from their representatives: they are extracted from a fasta with
# strain names without spaces (e.g. data/<lineage>/<segment>.fasta of nextstrain/seasonal-flu) and
# aligned with nextclade --gap-alignment-side right. Check the report with --dry-run first.
python ../helper-scripts/compute_founders.py --input-dir subclades --reference config/reference.fasta --annotation config/annotation.gff \
       --all-sequences sequences.fasta --lineages <new subclades> --dry-run

# assemble and validate founder sequences, and list representatives
python ../helper-scripts/assemble_founders.py --input-dir subclades --reference config/reference.fasta --annotation config/annotation.gff --strict \
       --output-fasta .auto-generated/founders.fasta --output-representatives .auto-generated/representatives.tsv

# To add the result to the repo, do:
git add .auto-generated/subclades.tsv  .auto-generated/subclade-proposals.tsv  .auto-generated/clades.tsv .auto-generated/clades-long.tsv .auto-generated/subclades.md .auto-generated/subclade-summary.tsv .auto-generated/subclade.nwk .auto-generated/founders.fasta .auto-generated/representatives.tsv
git commit -m "update auto-generated files"
```
