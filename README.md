![image](img/ipsn.png) 

# Site masking for MPXV phylogenetics

This repository is managed by the  [community of practice for specialized surveillance for emergency response](https://github.com/WHO-Collaboratory/collaboratory-mpox-genomics-community) pathogen genomics working group.

## Files in this repository

In this repository, we maintain a list of the genomic positions that we recommend to mask when reconstructing phylogenetic trees for monkeypox virus (MPXV). The rationale behind site masking is discussed [below](#why-do-sites-need-to-be-masked-in-MPXV-phylogenetics)

We currently maintain a list of sites for clade Ib named clade_Ib_mask.csv. Lists for clade Ia and clade I combined will follow soon

Sites within these masking files use the coordinates of [NC_003310](https://www.ncbi.nlm.nih.gov/nuccore/NC_003310.1)

The masking files are in a format that is compatible with [squirrel](https://github.com/aineniamh/squirrel). To use these files with squirrel, when running an alignment add the --additional-mask flag followed by the masking file. For example, to mask the recommended sites within clade Ib when aligning a set of sequences in a file called clade_Ib_unaligned.fasta, run:

```
squirrel --clade cladei clade_Ib_unaligned.fasta --additional-mask clade_Ib_mask.csv
```

The masking site lists are not fixed documents and we anticipate that more sites will be added moving forwards. If you think you have identified a site that should be masked, please raise an issue as outlined in the next section

## Raising an issue for a new site(s)

We expect that the masking site lists will need to be updated moving forwards. If you think you have identified one or more sites that should be masked, please let us know by raising an issue within this repository. To open a new issue, click on "Issues" at the top of this repository page and then click on "New issue"

Within this issue outline:
* The site(s) that are suggested to be masked - please use coordinates relative to the NC_003310 reference genome
* The types of data that have been used to identify this site as problematic, for example a genome alignment, phylogenetic tree or read data
* The reasons behind this suggestion. Outline the type of issue with the site and the evidence underlying this. Include a screenshot of the data that clearly shows this issue. These screenshots may include an alignment region (if the site is within a gappy region, is shortly upstream/downstream of a tract of Ns or there are several mutations close together) and/or a phylogenetic tree coloured by the nucleotide at the site (if the site has mutated multiple times within a clade or reverted to the nucleotide in other clades)

We have provided an example issue structure [here](https://github.com/WHO-Collaboratory/collaboratory-mpox-genomics-phylomasking/issues/1)

The issue will be evaluated by a member of the sites maintenance team and the site added if it meets the criteria

## Why do sites need to masked in MPXV phylogenetics?

To accurately reconstruct the relationships between MPXV genetic sequences, we need to distinguish real mutations from artifactual mutations. We aim to maintain a list of problematic genome positions that likely exhibit artifactual mutations or have the potential to be unreliable mutations either due to genomic features or bioinformatic issues. We recommend masking these genome positions when calculating a sequence alignment so they are not included in sequence comparisons or phylogenetic trees.

The MPXV genome contains repetitive regions and low complexity regions that can result in inference of artifactual mutations. These genome regions were identified [here](https://www.science.org/doi/10.1126/science.adg8116) and are included in the masking site list.

Additional positions associated with artifactual mutations can be inferred through identification of:
* Convergent mutations - given the large genome size and low substitution rate of MPXV, we would not expect the same genome position to mutate multiple times within closely related clades that have recent ancestry. Mutations that arise multiple times are therefore likely to be driven by a sequencing or bioinformatic artefact; we therefore recommend masking the sites of such convergent mutations.
* Reversions - reversions within a clade can be the result of bioinformatically calling reference nucleotides. Reversions can be identified by evaluating whether a mutation is shared with a reference genome or genomes from another clade.
* Clustered mutations - we would not expect multiple mutations to be acquired very close together on the same phylogenetic branch. These mutations are potentially due to assembly issues so can be masked from the alignment.
* Mutations close to Ns - Ns may indicate low coverage. Therefore mutations that are close to tracts of Ns may not be reliable and should be masked from that sequence
* Alignment regions with many gaps - if many sequences have gaps or Ns within a genomic region, mutations within the region are unlikely to be reliable. These regions should be masked from the alignment.

