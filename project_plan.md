# Lineage vs. Within-Lineage DMRs

## Group Research Project Plan

**Team:** Thais, Samitha, Nabin, Somraj

---

## 1. Background

Our pilot examined sperm differentially methylated regions (DMRs) from transgenerational epigenetic studies. The original question was whether these DMRs overlap regions that may escape DNA methylation reprogramming.

A different pattern emerged: **DMRs from comparisons between independently bred lineages were more enriched in repetitive regions than DMRs from comparisons within a single lineage.**

The studies, however, differed in analytical settings, especially DMR window size (100 bp vs 1,000 bp). Repetitive regions also have coverage and mappability properties that affect MeDIP-seq DMR detection.

## 2. Research Question and Hypothesis

**Research question:** Is repeat enrichment among sperm DMRs associated with comparing independently bred lineages, or is it explained by methodological factors, genetic divergence, or exposure-related biology?

**Main hypothesis:** Between-lineage comparisons produce more enrichment of sperm DMRs in repetitive regions than within-lineage comparisons, even after accounting for window size, MeDIP-testable regions and read coverage.

### Testable predictions

1. **Design:** Between-lineage DMR sets show repeat enrichment across exposures and generations.
2. **Within lineage:** Within-lineage DMR sets show little or no enrichment under the same controls.
3. **Method:** The difference persists after harmonizing window size and matching testable regions and coverage.
4. **Generation and exposure:** The pattern is not restricted to one compound or to F3.
5. **Genetic background:** If lineage divergence contributes, inbred designs show no repeat excess under the same controls.

### Alternative hypotheses

| Alternative | Supported if | Refuted if |
| --- | --- | --- |
| A1. Window size (100 vs 1,000 bp) | Enrichment follows window size, not design | Between-lineage sets at 1,000 bp are still enriched |
| A2. Statistical power and coverage | Enrichment disappears with a testable, coverage-matched background | Enrichment remains after that matching |
| A3. Real exposure effect | Enrichment differs by compound or grows from F1 to F3 | Similar across compounds and already in F1 |
| A4. Genetic divergence between lineages | Enrichment in outbred between-lineage designs only | Inbred designs show the same enrichment |
| A5. DMR significance threshold | Enrichment rises with stricter p cutoffs | Stable across cutoffs |
| A6. Reference assembly (collapsed repeats in rn6) | Enriched DMRs sit in regions collapsed in rn6 but expanded in mRatBN7.2 | No link to assembly differences |

**The main hypothesis needs revision if** enrichment follows window size rather than design (A1), disappears under coverage-matched backgrounds (A2), or is equally strong within a lineage.

## 3. Preliminary Results and Current Status

| Study | Design | Generations | Status |
| --- | --- | --- | --- |
| McBirney et al. 2017, atrazine | Between lineages: control vs exposed lineage | F1 to F3 | Done, including raw-read control for F3 |
| Thorson et al. 2020, atrazine | Within lineage: disease vs no disease | F3 | Published-DMR analysis done; raw-read control pending |

- **Genome-wide background:** artifact-prone repeat enrichment of 3.7x (McBirney F1), 2.8x (F2) and 5.2x (F3); Thorson sets 1.0x to 1.5x.
- **McBirney F3 with the strict background:** enrichment fell from **5.2x to 1.6x** when the background was restricted to MeDIP-testable regions and matched for read coverage. The remaining 1.6x is significant. A large part of the original signal is methodological; the hypothesis now concerns the remaining excess.
- **Germline reprogramming (secondary):** no consistent enrichment of DMRs in GD18-methylated regions (one marginal result in F3 at the 30% threshold, not robust to higher thresholds or multiple testing). Kept as a secondary, negative finding.

**Immediate priority:** process Thorson 2020 raw reads (GEO **GSE156530**) with the same testable-window and coverage-matched controls.

## 4. Studies

### Main comparison studies

| Exposure | Between-lineage study | Within-lineage study | Assigned |
| --- | --- | --- | --- |
| Atrazine | McBirney et al. 2017, F1 to F3 | Thorson et al. 2020, F3 disease EWAS | **Thais** (nearly done) |
| Methoxychlor | To identify | Nilsson et al. 2020 | **Thais** |
| Glyphosate | Kubsad et al. 2019, F1 to F3 | Ben Maamar et al. 2021 | **Samitha** |
| Vinclozolin | To identify | Nilsson et al. 2018 | **Samitha** |
| Permethrin + DEET | To identify | Thorson et al. 2020, *Environmental Health* | **Nabin** |
| Additional compounds | To search (other Skinner lab exposures) | To search | **Nabin** |
| DDT | Skinner et al. 2013, F3 (verify assay type) | Disease EWAS to identify | **Somraj** |

Pairings, DMR coordinates, GEO accessions, window sizes and assay types must be verified before a study is included.

### Optional genetic controls

- **Iqbal et al. 2015:** inbred mice exposed to endocrine disruptors; tests the role of genetic divergence (A4).
- **Radford et al. 2014:** outbred mice, maternal undernutrition, one generation; a different experimental setting.

Added only if their coordinates or raw data are usable and the core comparisons are on track. Owner: **Somraj**.

### Inclusion criteria

- Sperm methylation data (or appropriate germ-cell data for controls).
- DMR coordinates in a supplement or repository, or raw reads that allow DMR calling.
- Known or verifiable reference assembly.
- Classifiable comparison design.
- Recorded window size, statistical threshold, sample type and pooling.

### How much of each study to use

- **Every included DMR set:** published-table analysis (extraction, QC, enrichment against the genome-wide background). Cheap, hours per study.
- **Raw-read realignment** (testable windows, coverage matching): only for the pairs needed for the decisive comparisons, starting with Thorson 2020. About one day of cluster time per study.

## 5. Main Analyses

### A. Repeat enrichment

For each DMR set:

1. Extract and validate DMR coordinates.
2. Calculate overlap with tandem repeats, simple repeats, low-complexity regions and other artifact-prone features.
3. Evaluate 10% and 50% overlap thresholds.
4. Compare with matched random regions; record fold enrichment and empirical p values.
5. Use a fixed random seed and 1,000 permutations.

### B. Methodological controls

| Factor | How we test it |
| --- | --- |
| Window size (100 vs 1,000 bp) | Compare studies across the design x window table; re-call DMRs from raw reads with one window size for one pair |
| Testable regions | Restrict the background to windows where MeDIP had enough reads |
| Read coverage | Match background regions to DMR coverage |
| Significance threshold | Compare sets reported at several p cutoffs |
| Genome assembly | Compare repeat copy number between rn6 and mRatBN7.2 |

A genome-wide background is useful descriptively but **is not the decisive control** for MeDIP-seq repeat enrichment.

### C. Cross-study comparison

Compare fold enrichment by design, exposure, generation, window size and assay, and genetic background when controls are available.

**Key limitation:** if design and window size stay confounded, the difference cannot be attributed to design alone; a harmonized raw-read comparison is then required.

### D. Secondary reprogramming analysis

For rat data, re-test DMR overlap with GD18 methylation (escape threshold 30%, also 50% and 70%) automatically with the shared pipeline. Report negative results; GD18 methylation is not direct proof of germline escape.

## 6. Team Responsibilities

### Thais: atrazine, methoxychlor, and raw-read controls

**Studies:** McBirney 2017 and Thorson 2020 (atrazine, nearly done); methoxychlor (Nilsson 2020 and a lineage study if one exists).

**Tasks:**

1. Run the raw-read control on Thorson 2020 (GSE156530) and compare it with McBirney F3 under the same background.
2. Verify, extract and analyze the methoxychlor sets.
3. Run the harmonized raw-read comparison (same window size) for one between-lineage and one within-lineage study, once the metadata table shows which pair to use.
4. Maintain the repository, shared environment, scripts and conventions; integrate results into the cross-study table, heatmap and final figures.

**Question:** Does the between-lineage enrichment remain after the strictest methodological controls?

### Samitha: glyphosate and vinclozolin

**Studies:** Kubsad 2019 and Ben Maamar 2021 (glyphosate); Nilsson 2018 and a vinclozolin lineage study, if one exists.

**Tasks:**

1. Verify papers, designs and data availability; fill the metadata rows.
2. Extract, validate (counts, assembly, coordinates) and run the shared pipeline.
3. Compare glyphosate between-lineage and within-lineage results.
4. Determine whether a valid vinclozolin pair exists; if not, report the within-lineage set alone.

**Question:** Is the pattern consistent across exposures?

### Nabin: permethrin + DEET, additional compounds, statistical controls

**Studies:** Thorson 2020 (*Environmental Health*) and a permethrin + DEET lineage study, if one exists; search for additional Skinner lab compounds with sperm MeDIP-seq DMRs (for example jet fuel, plastics, dioxin, hydrocarbons) and verify their assay type.

**Tasks:**

1. Verify, extract, validate and analyze the permethrin + DEET sets.
2. Report candidate additional studies with their design, assay and data availability.
3. Test sensitivity to significance thresholds where studies report several cutoffs (A5).
4. Run negative controls with random region sets through the shared pipeline (expected about 1x).

**Question:** How much of the enrichment depends on thresholds or background choice?

### Somraj: DDT, assay comparability, genetic controls

**Studies:** Skinner 2013 (DDT) and a DDT disease EWAS, if one exists; optional genetic controls (Iqbal 2015, Radford 2014).

**Tasks:**

1. Confirm whether the DDT study used sequencing or arrays; find a within-lineage DDT set.
2. Extract, validate and analyze available DDT sets; document what limits comparability with MeDIP-seq.
3. Check whether Iqbal 2015 and Radford 2014 provide usable coordinates or raw reads; if yes, run them (A4).
4. Compare repeat copy number between rn6 and mRatBN7.2 for enriched regions (A6).

**Question:** Do other assays and genetic backgrounds support the same pattern?

### Shared responsibilities

Every member documents sources and accessions, uses the shared workflow and QC criteria, reviews another member's work, writes a short methods and results summary, and takes part in interpretation and the presentation.

## 7. Workflow and Quality Control

1. **Metadata:** one row per DMR set in `metadata/studies.tsv` (fields described in `docs/framing.md`).
2. **Extraction:** coordinates exported to the standard TSV format in `data/dmrs/`.
3. **QC:** counts vs paper, coordinate format, duplicates, CpG counts, assembly check (thresholds in `docs/framing.md`).
4. **Enrichment:** shared pipeline, both backgrounds where raw reads were processed.
5. **Integration:** one comparison table and heatmap grouped by design and compound.
6. **Review:** the reviewer approves each set before it enters the integrated results.

Each QC check is logged with pass/fail and a short note in `qc/qc_log.tsv`. Failed sets do not enter the integrated results.

## 8. Repository, Collaboration and Agents

One GitHub repository and a shared conda environment on Easley.

- **In Git:** metadata, scripts, configuration, environment files, derived DMR tables, QC logs, results, figures, documentation.
- **Outside Git:** FASTQ, BAM, reference genomes, large intermediate files.
- Each member works on their own branch; changes go through pull requests
- A shared `CLAUDE.md` holds project goals, file conventions, QC requirements and git rules, and points to `docs/framing.md` and this plan. Personal paths and account names stay in an untracked `CLAUDE.local.md`.
- We note what works and what fails with coding agents during the project, for the weekly reports.

## 9. Final Deliverables

1. Verified study metadata and standardized DMR datasets.
2. Reproducible scripts and QC logs.
3. Repeat enrichment results with methodological controls.
4. A cross-study table and heatmap comparing designs.
5. An assessment of the main hypothesis and each alternative.
6. A final group presentation and a manuscript outline.

### One-sentence summary

**We test whether the excess of repetitive-region DMRs in transgenerational sperm methylation studies reflects comparisons between independently bred lineages, beyond what technical detection biases explain.**
