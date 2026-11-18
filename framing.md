# Project framing: lineage vs within-lineage sperm DMRs

Last updated: 2026-11-17. This file is the reference for the hypothesis, roles and study metadata. Agents should read it before working in the repo.

## 1. Hypothesis

**H (main).** In transgenerational sperm MeDIP-seq studies on outbred rats, the excess of DMRs in tandem, simple and low-complexity repeats is a property of comparing **separately bred lineages**. It is not driven by the exposure, by disease status, by the MEDIPS window size, or by statistical power in high-coverage regions.

### Predictions 

1. **P1, design.** Every between-lineage DMR set (control vs exposed lineage, any compound, any generation) shows a repeat excess above the testable, coverage-matched background.
2. **P2, within lineage.** Within-lineage DMR sets (disease vs no disease inside one exposed F3 lineage) stay near 1x under the same background.
3. **P3, method.** The between vs within contrast holds when the window size is the same in both designs.
4. **P4, generation and compound.** The excess is already present in F1 and does not depend on the compound.

### What would refute H

| Result | Conclusion instead |
| --- | --- |
| Between-lineage sets at 1000 bp windows show no excess, while 100 bp sets do | Window size drives the pattern (A1) |
| Within-lineage sets show an excess as large as between-lineage sets | A generic MeDIP property, not lineage divergence |
| The excess disappears under the testable, coverage-matched background | Statistical power and coverage (A2) |
| The excess tracks one compound, or grows from F1 to F3 | An exposure-specific or accumulating signal (A3) |
| Inbred designs show the same excess | Not genetic divergence between lineages (A4) |

### Status 

| Background for McBirney 2017 F3 (876 DMRs) | Artifact-prone repeats: DMRs vs background | Fold |
| --- | --- | --- |
| Whole genome (pilot) | 72% vs 14% | 5.2x |
| Matched on testable fraction | 74% vs 35% | 2.1x |
| Testable fraction + MeDIP coverage | 74% vs 45% | 1.6x (p <= 0.002) |

- Most of the pilot excess reflects where MeDIP can call DMRs (only 6.9% of rn6 is testable). A significant 1.6x remains; the hypothesis now concerns this residual excess.
- Within-lineage sets (Thorson 2020) have only been tested against the whole-genome background (1.0x to 1.5x). **Next decisive test:** the same testable, coverage-matched control on Thorson raw reads (GEO GSE156530).
- Secondary result: no DMR set is enriched in regions that stay methylated at GD18 (escape from germline reprogramming). Re-tested automatically for every rat set (`--gd18`, `--sperm`); not expanded further.

### Alternatives

| ID | Alternative | Test |
| --- | --- | --- |
| A1 | MEDIPS window size (100 vs 1000 bp) | Studies that break the design x window confound |
| A2 | Statistical power and coverage | Testable, coverage-matched background (partly supported: 5.2x to 1.6x) |
| A3 | Real exposure effect on repeat methylation | Compare compounds and generations |
| A4 | Genetic divergence between outbred lineages | Inbred control (Iqbal 2015), one-generation outbred (Radford 2014) |
| A5 | Stricter DMR p thresholds select repeat DMRs | Sets reported at several thresholds |
| A6 | Collapsed repeats in rn6 | LiftOver to mRatBN7.2 and compare repeat copy number |

## 2. Roles

Full task descriptions are in `docs/project_plan.md`. Each person owns complete DMR sets end to end (metadata, extraction, QC, analysis, methods and results text); a reviewer approves each set's pull request before it enters the comparison.

| Person | Owns (data) | Cross-cutting role | Reviewer |
| --- | --- | --- | --- |
| Thais (`thr0012`) | Atrazine (McBirney 2017, Thorson 2020), methoxychlor |  pipeline and shared environment; raw-read controls (Thorson 2020, harmonized window comparison); integrated figures | Somraj |
| Samitha | Glyphosate, vinclozolin | Pairing between- and within-lineage sets per compound | Nabin |
| Nabin | Permethrin + DEET; search for additional compounds | Threshold sensitivity (A5); negative controls | Samitha |
| Somraj | DDT; genetic controls (Iqbal 2015, Radford 2014) | Assay comparability; assembly check (A6) | Thais |

**Reviewer rule:** the reviewer re-runs the extractor and compares the output checksum before approving the pull request.

## 3. Study metadata: what to fill

One row per **DMR set** (a study with F1, F2 and F3 tables, or five disease tables, has several rows). The machine-readable copy is `metadata/studies.tsv`, same columns.

### Metadata

| Column | What to write | Example (McBirney 2017 F3) | Where to find it |
| --- | --- | --- | --- |
| `set_id` | `<FirstAuthor><Year>_<set>` | `McBirney2017_F3` | |
| `compound` | Exposure | atrazine | Abstract |
| `design` | `between_lineage`, `within_lineage` or `inbred_control` | between_lineage | Methods: what is compared |
| `comparison` | Groups compared | control vs atrazine lineage | Methods |
| `generation` | F1, F2, F3 (or G1, G2 in mouse) | F3 | Methods |
| `species_strain` | Species and strain; outbred or inbred | rat, Sprague Dawley (outbred) | Methods |
| `tissue` | Sperm, prospermatogonia, other | sperm | Methods |
| `method` | MeDIP-seq, MeDIP-chip (array), WGBS, RRBS | MeDIP-seq | Methods |
| `window_bp` | MEDIPS window size | 100 | Methods ("window size"); DMR starts on a 100 or 1000 bp grid also tell |
| `p_threshold` | p value used for the reported DMRs | 1e-9 | Results or table legend |
| `n_dmrs_paper` | DMR count reported in the paper | 958 | Results or abstract |
| `dmr_table_source` | Supplement file and format | PDF supplement | Supporting information |
| `assembly` | Reference genome | rn6 | Methods; confirm with `03_check_dmr_assembly.py` |
| `geo` | GEO series | GSE98683 | Data availability |
| `sra_project` | SRA BioProject | PRJNA385883 | GEO page |
| `n_samples` | Samples per group | 18 control, 50 atrazine | GEO sample list |
| `sample_type` | `pool` or `individual` | individual | GEO characteristics |
| `read_layout` | single or paired; read length | paired, 98 bp | SRA run table |
| `raw_data` | yes / no | yes | GEO / SRA |
| `extracted` | yes / no; path in `data/dmrs/` | yes, `McBirney2017_F3_DMRs.tsv` | |
| `qc_pass` | yes / no (see section 4) | yes | `qc/qc_log.tsv` |
| `owner` | GitHub handle | `thr0012` | |
| `checker` | GitHub handle | | |
| `notes` | Anything unusual | DMR spans merge windows up to 1 kb apart | |

### Studies to fill

Rows marked "to identify" are open: find the study. Values in the pilot rows are verified; everything else is to be filled.

| set_id | compound | design | study | owner | window_bp | p_threshold | n_dmrs_paper | assembly | geo | raw_data | qc_pass |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| McBirney2017_F1 | atrazine | between_lineage | McBirney 2017, PLoS ONE | Thais | 100 | 1e-6 | 519 | rn6 | GSE98683 | yes | yes |
| McBirney2017_F2 | atrazine | between_lineage | McBirney 2017, PLoS ONE | Thais | 100 | 1e-5 | 431 | rn6 | GSE98683 | yes | yes |
| McBirney2017_F3 | atrazine | between_lineage | McBirney 2017, PLoS ONE | Thais | 100 | 1e-9 | 958 | rn6 | GSE98683 | yes | yes |
| Thorson2020_S2_lean | atrazine | within_lineage | Thorson 2020, PLoS ONE | Thais | 1000 | 1e-4 | 301 | rn6 | GSE156530 | | yes |
| Thorson2020_S3_kidney | atrazine | within_lineage | Thorson 2020, PLoS ONE | Thais | 1000 | 1e-4 | 261 | rn6 | GSE156530 | | yes |
| Thorson2020_S4_testis | atrazine | within_lineage | Thorson 2020, PLoS ONE | Thais | 1000 | 1e-4 | 693 | rn6 | GSE156530 | | yes |
| Thorson2020_S5_puberty | atrazine | within_lineage | Thorson 2020, PLoS ONE | Thais | 1000 | 1e-4 | 322 | rn6 | GSE156530 | | yes |
| Thorson2020_S6_multiple | atrazine | within_lineage | Thorson 2020, PLoS ONE | Thais | 1000 | 1e-4 | 336 | rn6 | GSE156530 | | yes |
| Kubsad2019_F1..F3 | glyphosate | between_lineage | Kubsad 2019, Sci Rep | Samitha | | | | | | | |
| BenMaamar2021_<disease> | glyphosate | within_lineage | Ben Maamar 2021, Epigenetics | Samitha | | | | | | | |
| to identify | vinclozolin | between_lineage | | Samitha | | | | | | | |
| Nilsson2018_<disease> | vinclozolin | within_lineage | Nilsson 2018, PLoS ONE | Samitha | | | | | | | |
| to identify | permethrin + DEET | between_lineage | | Nabin | | | | | | | |
| Thorson2020EH_<disease> | permethrin + DEET | within_lineage | Thorson 2020, Environ Health | Nabin | | | | | | | |
| to identify | methoxychlor | between_lineage | | Thais | | | | | | | |
| Nilsson2020_<disease> | methoxychlor | within_lineage | Nilsson 2020, Environ Epigenet | Thais | | | | | | | |
| Skinner2013_F3 | DDT | between_lineage | Skinner 2013, BMC Med (check: array or sequencing) | Somraj | | | | | | | |
| to identify | DDT | within_lineage | | Somraj | | | | | | | |
| to identify | additional compounds | both | | Nabin | | | | | | | |
| Iqbal2015_<set> | vinclozolin, BPA, DEHP | inbred_control | Iqbal 2015, Genome Biol (mouse) | Somraj | | | | | | | |
| Radford2014_F1 | undernutrition | between groups, 1 generation | Radford 2014, Science (outbred mouse) | Somraj | | | | | | | |

## 4. QC gates (a set enters the comparison only if all pass)

| Gate | Check | Pass |
| --- | --- | --- |
| Count | Extracted rows vs `n_dmrs_paper` | Equal |
| Names | DMR name matches chr:start | 100% |
| Duplicates | Duplicate DMR names | 0 |
| CpG density | `cpg_num / length` vs reported density | Differences only from rounding |
| Window grid | Starts on the `window_bp` grid (1-based) | > 80% |
| Assembly | `03_check_dmr_assembly.py`: CpG recount vs paper | >= 90% within 1 CpG, else liftOver first |
| Checker | Second person re-runs the extractor; checksums match | Identical |

Each gate writes one line to `qc/qc_log.tsv`: `set_id`, `gate`, `value`, `pass`, `date`, `who`.

## 5. Sources

- McBirney et al. 2017, PLoS ONE: https://doi.org/10.1371/journal.pone.0184306
- Thorson et al. 2020, PLoS ONE: https://doi.org/10.1371/journal.pone.0239380
- El Omri-Charai et al. 2023, Development (GD18 germ cell methylome, GSE220440): https://doi.org/10.1242/dev.201606
- Kubsad et al. 2019, Scientific Reports: https://doi.org/10.1038/s41598-019-42860-0
- Ben Maamar et al., glyphosate EWAS, Epigenetics: https://doi.org/10.1080/15592294.2020.1853319
- Nilsson et al. 2018, PLoS ONE: https://doi.org/10.1371/journal.pone.0202662
- Thorson et al. 2020, Environmental Health: https://doi.org/10.1186/s12940-020-00666-y
- Nilsson et al. 2020, Environmental Epigenetics (methoxychlor), PMID 33391823
- Skinner et al. 2013, BMC Medicine (DDT): https://doi.org/10.1186/1741-7015-11-228
- Iqbal et al. 2015, Genome Biology: https://doi.org/10.1186/s13059-015-0619-z
- Radford et al. 2014, Science: https://doi.org/10.1126/science.1255903
