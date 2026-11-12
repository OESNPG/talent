# TALENT: A Pipeline for Aligning Theses and Dissertations with Strategic Themes Framework to Evaluate National Installed Human Capacity – An Empirical Evaluation on AI in Brazil

**Authors:** Marcelo Augusto Santos Turine, Solange Oliveira Rezende, Flávia Cristina Bernardini, Marcos Paulo Silva Gôlo, and Ricardo Marcondes Marcacini.


## Abstract


Geopolitical powers, including the United States, the European Union, and China, have established strategic Artificial Intelligence (AI) roadmaps aligned with their respective visions of technological sovereignty and digital competitiveness. A critical determinant of this sovereignty is the installed human capacity required to support advanced AI research, innovation, and workforce development. However, systematically measuring topic-specific research capacity across large nations or macro-regions remains challenging. To address this gap, we introduce TALENT, a specialized AI-driven data pipeline designed for the large-scale evaluation and strategic alignment of academic theses and dissertations to map national installed capacity across priority research domains. Combining Natural Language Processing (NLP), semantic embeddings, and Large Language Models (LLMs), TALENT extracts and semantically analyzes metadata and textual content from master’s dissertations and doctoral theses, aligns them with strategic thematic frameworks, and generates granular capacity mappings aggregated by macro-region, state, university, and graduate program. We evaluate TALENT through a large-scale empirical study of the Brazilian National Graduate System (SNPG), using the CAPES National Agenda for Advanced Human Capital Development and the CAPES Sucupira database of theses and dissertations. The evaluation examines both the reliability of the proposed semantic alignment pipeline and its ability to support nationwide analysis of human resource development in Artificial Intelligence. The nationwide application enables us to investigate how AI-related expertise is distributed across institutions, regions, graduate programs, and disciplinary areas within the Brazilian SNPG. Our findings reveal that a substantial portion of AI-related graduate education occurs outside traditional Computer Science disciplines, highlighting the growing interdisciplinary diffusion of AI across multiple knowledge areas. Overall, the empirical results demonstrate the potential of TALENT as a scalable analytical framework for mapping national research capacity and illustrate how AI-assisted governance can support evidence-based planning, funding policies, and strategic human capital development in emerging economies.

**Keywords:** Human-Installed Capacity; Natural Language Processing; Large Language Models; Strategic Themes.

## Study overview

TALENT aligns master's dissertations and doctoral theses with externally defined strategic research priorities. Its principal automated stages are described in **Section 5** and **Figure 1** of the manuscript:

1. **Theme selection:** select strategic themes associated with a research topic. The AI case study identifies **28 of the 470 themes** in the CAPES National Agenda, distributed across seven macro-themes (**Section 6.2, Tables 1–2**).
2. **Preprocessing:** standardize graduate and research metadata, including titles, abstracts, keywords, knowledge areas, and graduate programs.
3. **Semantic filtering:** generate embeddings, compute candidate theme–graduate similarity scores, and rank candidate associations.
4. **Semantic matching:** apply a smaller language model for screening, followed by a larger language model for selective reassessment, producing affinity labels and explanations.

The study considers the **2023 and 2024** cohorts of graduates recorded in the [CAPES Sucupira Platform](https://sucupira.capes.gov.br/) and the strategic priorities of the [CAPES National Agenda](https://agendanacional.capes.gov.br/). The paper reports **11,830 graduates with AI-related thematic affinity in 2023** and **12,459 in 2024** (**Section 6.3, Figures 5–6**). These are counts of *distinct graduates*, not counts of theme–graduate pairs.

## Repository contents

All files are kept in the **repository root**. Links below are relative paths so they work directly on GitHub.

### Mapping data and the semantic pipeline

| File | Contents | Connection to the paper |
| --- | --- | --- |
| [df_mapeamento_titulados_2023_filtrado.csv](df_mapeamento_titulados_2023_filtrado.csv) | Theme–graduate candidate mappings for 2023; **29,936 rows** and 28 distinct theme IDs. Includes similarity scores, ranks, LLM responses, and final affinity labels. | **Sections 5 and 6.2**; data used for the 2023 national AI-capacity analysis and downstream aggregations. |
| [df_mapeamento_titulados_2024_filtrado.csv](df_mapeamento_titulados_2024_filtrado.csv) | Corresponding 2024 theme–graduate mappings; **32,852 rows** and 28 distinct theme IDs. | **Sections 5 and 6.2**; 2024 national analysis and downstream aggregations. |
| [ablation.ipynb](ablation.ipynb) | Notebook comparing embedding retrieval, small-LLM screening, and large-LLM refinement, including a label-transition matrix. | **Section 6.4, Table 5 and Figure 7**. |
| [matriz_confusao_mistral_vs_qwen3.png](matriz_confusao_mistral_vs_qwen3.png) | Saved visualization of label transitions between the smaller and larger language models. | **Section 6.4, Figure 7**, ablation of the semantic matching stages. |

The two `df_mapeamento_titulados_*_filtrado.csv` files retain the following **nine** fields:

| Column | Meaning |
| --- | --- |
| `HASH_ID` | Identifier for a graduate/research record, represented by a hash. |
| `TEMA_ID` | Strategic theme identifier (see **Table 1** of the paper for the 28 AI-related themes). |
| `UF_TEMA` | Brazilian federative unit associated with the strategic theme. |
| `UF_PESQUISADOR` | Federative unit associated with the graduate/research record. |
| `SCORE_FINAL` | Combined semantic affinity score used in candidate filtering. |
| `RANK` | Candidate ranking. |
| `MISTRAL_NEMO` | Structured response from the smaller language model, including its affinity assessment and justification. |
| `Qwen3` | Structured response from the larger language model, including its affinity assessment and justification. |
| `nivel` | Final recorded affinity label: `ALTA`, `MEDIA`, `BAIXA`, or `NENHUMA` (High, Medium, Low, or None). |

**Unit of analysis:** The mapping files contain **theme–graduate pairs**; an individual graduate may be associated with more than one theme. To count graduates, deduplicate by `HASH_ID` within the population of interest, following the notebook's analysis logic. The intermediate counts reported in **Table 5** correspond to the article's specified ablation stages and should not be inferred directly from the total rows of the exported mapping files.

### National distribution and research questions

| File | Contents | Connection to the paper |
| --- | --- | --- |
| [formandos_por_uf_2023.csv](formandos_por_uf_2023.csv) | Distinct graduates associated with AI-related themes in 2023, by state, region, and degree (master's/PhD), including regional totals. | **RQ2**, **Section 6.3**, **Figure 5**. |
| [formandos_por_uf_2024.csv](formandos_por_uf_2024.csv) | Equivalent geographic and degree-level summary for 2024. | **RQ2**, **Section 6.3**, **Figure 6**. |
| [programas_Computacao_2023.csv](programas_Computacao_2023.csv) | Graduate-program name, CAPES program code, evaluation area, and institution acronym for Computer Science programs in 2023. | **RQ3**, **Section 6.3**, **Table 3**; program identification for comparing Computer Science with other areas. |
| [programas_Computacao_2024.csv](programas_Computacao_2024.csv) | Equivalent four-field Computer Science graduate-program listing for 2024. | **RQ3**, **Section 6.3**, **Table 3**. |
| [grandes_areas_grau_2023.csv](grandes_areas_grau_2023.csv) | AI-related graduates by major knowledge area and degree, plus the number of graduate programs, for 2023. | **RQ4**, **Section 6.3**, **Table 4**. |
| [grandes_areas_grau_2024.csv](grandes_areas_grau_2024.csv) | Equivalent summary by knowledge area and degree for 2024. | **RQ4**, **Section 6.3**, **Table 4**. |
| [tabela_crosstab_uf_tema.csv](tabela_crosstab_uf_tema.csv) | Number of distinct graduates for each strategic theme × graduate's state, constructed from retained High/Medium mappings across 2023–2024. | Supplementary thematic/territorial cross-tabulation supporting **RQ1–RQ2**, **Section 6.2**. |
| [MAPEAMENTOS GERAIS - POR UF.xlsx](MAPEAMENTOS%20GERAIS%20-%20POR%20UF.xlsx) | Spreadsheet presentation of the theme × state cross-tabulation (same numerical cells as the CSV). | Supplementary results for **RQ1–RQ2**, **Section 6.2**. |
| [artigo-AI-capacity.ipynb](artigo-AI-capacity.ipynb) | Analysis notebook with aggregation functions for graduate counts by state, program comparisons, knowledge areas, selected examples, and the 2023–2024 theme × state table. | Analyses underlying **Section 6.2–6.3**, notably **Figures 5–6** and **Tables 3–4**. |

**RQ1** (selection of AI-related strategic themes and macro-themes) is documented directly in **Tables 1–2** of the paper. The selected `TEMA_ID` values are available in the mapping datasets.

## Citation

If you use these materials, please cite the associated article. Until final bibliographic metadata is available, refer to the manuscript:

> Turine, M. A. S., Rezende, S. O., Bernardini, F. C., Gôlo, M. P. S., and Marcacini, R. M. *TALENT: A Pipeline for Aligning Theses and Dissertations with Strategic Themes Framework to Evaluate National Installed Human Capacity – An Empirical Evaluation on AI in Brazil*. Manuscript, Journal of the Brazilian Computer Society.

## Contact

Questions about the paper, associated data, or experiments: **[agendanacional@ufms.br](mailto:agendanacional@ufms.br)**.
