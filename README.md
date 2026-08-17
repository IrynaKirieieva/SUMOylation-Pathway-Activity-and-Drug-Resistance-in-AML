SUMOylation Pathway Activity and Therapy Resistance in AML

Proteomics-based analysis of whether SUMO pathway protein expression associates with ex vivo drug resistance in AML, using public proteomic and pharmacologic data from the Beat AML / PTRC cohort (Pino et al., 2024; Bottomly et al., 2022).

Summary
Composite scoring across all 19 canonical SUMO pathway genes showed no clear resistance signal, motivating a gene-level and functional-arm (conjugation vs deconjugation) decomposition.
UBA2 (E1 activating enzyme subunit) shows the strongest, most consistent association with drug resistance across the panel, independent of NPM1 mutation status.
A "net SUMOylation activity" score (conjugation − deconjugation z-scores) shows an FLT3-ITD-specific association with FLT3-pathway-relevant inhibitors (Gilteritinib, MGCD-265), confirmed by a formal interaction test (Fisher's z), not just subgroup significance comparison.
Genome-wide limma + GSEA (independent of the 19-gene panel used to build the scores above) shows significant enrichment of the GO term "protein sumoylation" among proteins associated with high UBA2, along with biologically related processes (mRNA splicing, ribosome biogenesis, double-strand break repair).


Data sources
File	Source	Link
CPTAC3_PTRC_PNNL_BEAT_AML_Proteome.tmt11.tsv	PDC000477, "Beat AML Baseline Clinical - Proteome"	Proteomic Data Commons link: https://biodev.github.io/BeatAML2/
PDC_study_biospecimen_*.tsv	PDC000477 biospecimen export	Same study page as above, "Biospecimen" tab link: https://pdc.cancer.gov/pdc/study/PDC000477
beataml_waves1to4_sample_mapping.xlsx	BeatAML2 harmonized data	github.com/biodev/beataml2.0_data link: https://biodev.github.io/BeatAML2/ 
beataml_probit_curve_fits_v4_dbgap.txt	BeatAML2 drug AUC values	github.com/biodev/beataml2.0_data link: https://biodev.github.io/BeatAML2/
beataml_wv1to4_clinical.xlsx	BeatAML2 clinical/mutation annotations	github.com/biodev/beataml2.0_data link: https://biodev.github.io/BeatAML2/

Repository structure:
01_data_preparation.R — load proteomics, clinical, and drug-response data; resolve sample IDs between PDC and BeatAML; filter unmatched samples; construct the merged proteomics–drug-response dataset.
02_pathway_scores.R — calculate SUMO pathway scores, including composite, SUMOylation, deSUMOylation, net SUMO, and E1 complex (SAE1 + UBA2) scores; perform pathway-level and gene-level correlations with drug AUC; identify key gene–drug associations; evaluate the E1 complex as the primary downstream score and generate corresponding drug-association plots.
03_subtype_stratification.R — stratify E1 complex–drug associations by NPM1 and FLT3-ITD mutation status; handle discordant mutation records; calculate subgroup-specific Spearman correlations; formally test differences between subgroup correlations using Fisher’s z-transformation.
04_dea_enrichment.R — define high- and low-E1-complex groups; perform genome-wide differential protein abundance analysis using limma; generate volcano plots; perform GO Biological Process GSEA to identify biological processes associated with E1 complex activity.
05_unsupervised_analysis.R — assess the global proteomic structure of E1 complex groups using PCA and exploratory UMAP.

Requirements
R >= 4.3, packages: tidyverse, data.table, readxl, limma, clusterProfiler, org.Hs.eg.db, uwot, ggrepel (see individual scripts for install commands).
Proteomics-clinical linkage recovered 152/211 patients (72%); the remainder are absent from the harmonized BeatAML sample mapping release, not a processing error (see 01_data_preparation.R for the diagnostic).
All expression values are relative to an in-cohort reference pool, not a healthy control — findings describe inter-patient variability within AML, not AML-vs-healthy differences.
Composite/subgroup scores are z-score averages across genes annotated by canonical pathway role; empirically, not every gene's behavior matched its canonical role (e.g. RANBP2 trended with deconjugases despite being an E3 ligase) — see 02_pathway_scores.R for the gene-level breakdown that motivated this.
