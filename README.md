![image](img/ipsn.png)

# Site masking for MPXV phylogenetics

> ## ⚠️ This repository is deprecated — 1 November 2026
>
> **Do not use `clade_Ib_mask.csv` with current versions of squirrel.** The masking
> file in this repository is expressed in `NC_003310` coordinates. Since
> [squirrel v1.3.2](https://github.com/aineniamh/squirrel/releases) (January 2026),
> squirrel aligns clade I against `PX667572`, and `--additional-mask` applies
> coordinates directly to alignment columns with no translation. Every maskable
> position in this file is displaced by 47–74 bp in that reference frame, so the
> command in the instructions below would mask the wrong positions — silently, with
> no warning. squirrel v1.3.1 and earlier are not affected.
>
> **This repository is also being retired for a more fundamental reason than stale
> coordinates.** The approach taken here conflates two kinds of problem that need
> different treatment:
>
> - **Artefacts in individual sequences** — a mutation adjacent to a tract of Ns, a
>   call to the reference base, several mutations clustered on one branch. These are
>   properties of one sequence, not of the genome. Masking them should affect that
>   sequence only.
> - **Problematic regions of the genome** — inverted terminal repeats, tandem and
>   low-complexity repeats, regions where assembly is unreliable for everyone. These
>   are properties of MPXV and should be masked across the whole alignment.
>
> Roughly two thirds of the entries in `clade_Ib_mask.csv` are of the first kind but
> are applied as the second. The rationale section below says of one category, in its
> own words, that such mutations "should be masked from that sequence" — yet the file
> masks them as whole alignment columns in every sequence. Because the sites noticed
> this way are variable sites by construction, the effect is to remove
> phylogenetically informative positions from good sequences in order to suppress
> artefacts present in a subset of poor ones.
>
> When this file was assembled in late 2024 the only released mechanism was
> whole-column masking; squirrel's per-sequence option (`--sequence-mask`) shipped in
> v1.0.12 nine days after this repository's last update. The distinction is now well
> supported by the tools and the file has not been revised to use it.
>
> **What is replacing it.** This work is moving to the WHO Collaboratory Pathogen
> Genomics Commons, which will hold MPXV masking as two separate, explicitly labelled
> tiers rather than one combined list, with each entry carrying its evidence, its
> reference frame and the date it was last reviewed:
> [WHO-Collaboratory/collaboratory-pathogen-genomics-commons](https://github.com/WHO-Collaboratory/collaboratory-pathogen-genomics-commons)
> (in development). The durable, evidence-backed entries from this repository are
> being carried across; the entries that describe one 2024 sequence set are not.
>
> **What to use in the meantime.** Both actively maintained tools already apply
> structural masking for clade I by default, in their own coordinate frames, and
> neither needs a file from here:
>
> - [squirrel](https://github.com/aineniamh/squirrel) — masks repetitive and
>   homopolymeric regions and the terminal ITR by default. Run with `-qc` to have it
>   flag sites that may warrant masking in your own data, and use `--sequence-mask`
>   for artefacts confined to particular sequences.
> - [nextstrain/mpox](https://github.com/nextstrain/mpox) —
>   `phylogenetic/defaults/clade-i/mask.bed`, a curated and actively maintained set
>   of clade I regions with per-region rationale.
>
> Note that masking is no longer cost-free: the Nextclade mpox clade-I dataset
> removed a range from its placement mask in July 2026 because it is informative for
> lineage assignment within clade Ib. Masking more is not automatically safer.
>
> `clade_Ib_mask.csv` keeps all of its 120 entries unchanged, so the record and the
> positions remain available, and it now carries a `Reference` column naming
> `NC_003310` on every row — the frame was previously stated only in prose here, which
> is part of how this went wrong. The issues raised here remain readable. The rationale
> below is retained because it is still a good statement of why MPXV needs masking at
> all, and the Commons work builds on it.

---

## Files in this repository

`clade_Ib_mask.csv` — 120 entries for MPXV clade Ib, in the coordinates of
[NC_003310](https://www.ncbi.nlm.nih.gov/nuccore/NC_003310.1), as the file's
`Reference` column now records. See the deprecation notice above before using it. The clade Ia and combined clade I lists mentioned in
earlier versions of this README were never published and are not planned here; that
scope moves to the Commons.

## Why do sites need to be masked in MPXV phylogenetics?

To accurately reconstruct the relationships between MPXV genetic sequences, we need to
distinguish real mutations from artefactual mutations. We aim to maintain a list of
problematic genome positions that likely exhibit artefactual mutations or have the
potential to be unreliable mutations either due to genomic features or bioinformatic
issues. We recommend masking these genome positions when calculating a sequence
alignment so they are not included in sequence comparisons or phylogenetic trees.

The MPXV genome contains repetitive regions and low complexity regions that can result
in inference of artefactual mutations. These genome regions were identified
[here](https://www.science.org/doi/10.1126/science.adg8116) and are included in the
masking site list.

Additional positions associated with artefactual mutations can be inferred through
identification of:

* Convergent mutations - given the large genome size and low substitution rate of
  MPXV, we would not expect the same genome position to mutate multiple times within
  closely related clades that have recent ancestry. Mutations that arise multiple
  times are therefore likely to be driven by a sequencing or bioinformatic artefact;
  we therefore recommend masking the sites of such convergent mutations.
* Reversions - reversions within a clade can be the result of bioinformatically
  calling reference nucleotides. Reversions can be identified by evaluating whether a
  mutation is shared with a reference genome or genomes from another clade.
* Clustered mutations - we would not expect multiple mutations to be acquired very
  close together on the same phylogenetic branch. These mutations are potentially due
  to assembly issues so can be masked from the alignment.
* Mutations close to Ns - Ns may indicate low coverage. Therefore mutations that are
  close to tracts of Ns may not be reliable and should be masked from that sequence
* Alignment regions with many gaps - if many sequences have gaps or Ns within a
  genomic region, mutations within the region are unlikely to be reliable. These
  regions should be masked from the alignment.

The last two bullets illustrate the distinction the deprecation notice describes: one
is a property of a single sequence, the other a property of the alignment. The Commons
will keep them apart.

## Evidence expected for a proposed site

This standard is retained here because it carried across to the Commons unchanged. A
proposal should state:

* The site(s) proposed for masking, **with the reference frame they are given in**
* Whether the problem is specific to particular sequences or affects the alignment as
  a whole
* The types of data used to identify the site — for example a genome alignment, a
  phylogenetic tree, or read data
* The reasons behind the suggestion, outlining the type of issue and the evidence
  underlying it, with a figure that clearly shows it. For a site in a gappy region,
  near a tract of Ns, or with several mutations close together, an alignment view. For
  a site that has mutated repeatedly or reverted, a phylogeny coloured by the
  nucleotide at that site.

An [example](https://github.com/WHO-Collaboratory/collaboratory-mpox-genomics-phylomasking/issues/1)
is retained in the issues of this repository. New proposals should go to the Commons
rather than here.

---

This repository was managed by the IPSN
[community of practice on specialized surveillance for emergency response](https://github.com/WHO-Collaboratory/collaboratory-mpox-genomics-community)
pathogen genomics working group.
