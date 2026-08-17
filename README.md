SUMOylation Pathway Activity and Therapy Resistance in AML

Proteomics-based analysis of whether SUMO pathway protein expression associates with ex vivo drug resistance in AML, using public proteomic and pharmacologic data from the Beat AML / PTRC cohort (Pino et al., 2024; Bottomly et al., 2022).

Background and aim

SUMOylation is a post-translational modification that has been implicated in cancer biology and therapy resistance. Recent studies suggest that inhibition of the SUMO pathway may improve treatment response in AML, highlighting the need to further investigate the role of SUMOylation in AML biology.

SUMOylation is a dynamic cellular process involving the SUMO-activating E1 enzyme (SAE1/SAE2), the SUMO-conjugating enzyme UBE2I, and several SUMO ligases. The process is reversed by deSUMOylating enzymes (SENPs).

The aim of this study was to explore whether SUMO pathway proteins are associated with ex vivo drug response in AML and to identify proteins associated with drug sensitivity or resistance.

Pathway-level analysis

We first evaluated several pathway-level scores: a composite score including all 19 SUMO pathway proteins, separate SUMOylation and deSUMOylation scores, and a net SUMO score representing the balance between the two processes.

These scores were used as exploratory measures of overall pathway state. However, because SUMO pathway proteins have distinct and sometimes opposing biological functions, pathway-level scores may obscure protein-specific effects. Therefore, the main analysis focused on individual SUMO pathway proteins.

The deSUMOylation score showed predominantly negative correlations with drug AUC, suggesting an association with greater drug sensitivity, with the strongest association observed for cytarabine. In contrast, the SUMOylation score showed associations in both directions across different drugs, highlighting the complexity of the pathway.

The net SUMO score showed a positive association with drug resistance for several drugs, suggesting that a relative predominance of SUMOylation over deSUMOylation may be associated with resistance. However, all observed correlations were modest (|Spearman r| < 0.4), so these findings should be interpreted as exploratory rather than strong predictive relationships.

Protein-level analysis

We next examined individual SUMO pathway proteins. UBA2, a component of the SUMO E1 activating enzyme complex, showed positive correlations with drug AUC for 17 drugs, indicating an association between higher baseline UBA2 abundance and greater resistance.

Because UBA2 (SAE2) forms the functional E1 complex together with SAE1, we combined SAE1 and UBA2 into an E1 complex score. The E1 complex showed its strongest associations with Idelalisib, MK-2206 and 17-AAG (Tanespimycin).

An interesting finding was the negative correlation between baseline E1 complex abundance and Venetoclax AUC, indicating that higher baseline E1 protein levels were associated with greater Venetoclax sensitivity. This association was modest and did not reach statistical significance in the analysis (r = −0.133, p = 0.129, n = 131), and therefore should not be considered conclusive.

Importantly, baseline abundance of E1 proteins does not directly measure SUMOylation activity. Therefore, this finding does not necessarily contradict previous evidence that pharmacological inhibition of SUMO E1 can enhance Venetoclax sensitivity. One possible interpretation is that high baseline E1 abundance reflects a cellular state characterized by increased SUMO-pathway stress or dependence, making these cells more vulnerable to Venetoclax while also potentially more sensitive to perturbation of the SUMO pathway. This hypothesis requires experimental validation.

Interestingly, SENP3 also showed a negative association with Venetoclax AUC, suggesting that multiple components of the SUMO regulatory system may capture aspects of the cellular state associated with Venetoclax response.

Overall, UBA2/E1 associations were predominantly positive: positive correlations were observed for 15 drugs and negative correlations for 4 drugs, although the magnitude of all correlations remained modest.

Proteome-wide analysis

Finally, we compared the proteomic profiles of patients with high and low E1 complex scores. Differential analysis identified proteins including CHD4, RCOR3, PHF14, WRNIP1, ADNP, PSIP1, SART3, SSBP4, SUPT16H, CRBN, MBD3, NUMA1 and ECI2.

PCA and UMAP suggested that patients with high and low E1 complex scores represent partially distinct proteomic states, although the groups were not completely separated.

GO enrichment analysis of proteins associated with high E1 complex abundance revealed enrichment of processes related to mRNA processing, RNA splicing, rRNA processing, ribosome biogenesis and ribonucleoprotein complex biogenesis, suggesting that high E1 abundance may be associated with a broader cellular state involving altered RNA and protein biosynthetic activity.


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
