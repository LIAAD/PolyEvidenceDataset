# PolyEvidence

**PolyEvidence: Enhancing PolyNarrative for Evidence Detection from Document-Level Dominant Narratives**

PolyEvidence is a multilingual, multi-domain dataset and benchmark for identifying the **dominant narrative of a news article** and extracting the **text spans that support that narrative**. It extends [PolyNarrative](https://aclanthology.org/2025.acl-long.1513/) with document-level dominant narrative labels and fine-grained evidence annotations, supporting research on explainable narrative analysis, multilingual NLP, and the study of polarized information environments.

**Paper:** Link to be added (ARR manuscript).  
**Dataset:** Available upon request (see [Dataset access](#dataset-access)).  
**Contact:** [nuno.r.guimaraes@inesctec.pt](mailto:nuno.r.guimaraes@inesctec.pt)

## Contents

- [Overview](#overview)
- [Dataset and access](#dataset)
- [Benchmark tasks](#benchmark-tasks)
- [Baselines and evaluation](#baselines-and-evaluation)
- [Online appendix](#online-appendix)
  - [A. Narrative taxonomies](#a-narrative-taxonomies)
  - [B. Annotation agreement](#b-annotation-agreement)
  - [C. Generative baseline prompts](#c-generative-baseline-prompts)
  - [D. mBERT fine-tuning details](#d-mbert-fine-tuning-details)
  - [E. Relaxed evaluation results](#e-relaxed-evaluation-results)
- [Limitations](#limitations)
- [Citation](#citation)

## Overview

PolyNarrative provides paragraph-level narrative annotations for multilingual news articles. PolyEvidence adds two complementary annotation layers:

1. **Document-level dominant narrative:** the narrative that best captures the article's overall framing, viewpoint, or intended interpretation. This is not necessarily the most frequent paragraph-level narrative.
2. **Narrative-supporting evidence:** the textual spans that justify the assignment of the dominant narrative to the article.

The evidence spans explain **how a narrative is conveyed in the article**; they do **not** establish that the underlying claims are factually true.

## Dataset

| Property | Value |
| --- | --- |
| Documents | **1,065** |
| Annotated evidence spans | **4,348** |
| Average evidence spans per document | **4.08** |
| Document-level dominant narratives | **21** |
| Languages | Bulgarian (BG), English (EN), Portuguese (PT), Russian (RU) |
| Domains | Climate Change (CC), Russia–Ukraine War (URW) |
| Approximate size | Over **788,000 tokens**; **740 tokens per document** on average |
| Splits | Train / validation / test, approximately 80% / 10% / 10% |

The dataset includes both domains for Bulgarian, English, and Portuguese. The Russian subset covers the Russia–Ukraine War only.

### Language-level characteristics

| Language | Mean tokens/document | Approx. evidence-token coverage |
| --- | ---: | ---: |
| Bulgarian (BG) | 980 | 6% |
| English (EN) | 779 | 6% |
| Portuguese (PT) | 632 | 5% |
| Russian (RU) | 649 | 8% |

Across the dataset, Bulgarian has the lowest mean number of evidence spans per document (**2.77**) and Portuguese the highest (**5.20**). The most frequent narratives are **Amplifying Climate Fears** in BG (64 documents) and PT (63 documents), **Discrediting the West, Diplomacy** in EN (33), and **Praise of Russia** in RU (83).

### Annotation process

Annotation was organized by language-specific teams, with expert review and curation. Annotators selected a dominant narrative for each document using the domain-specific PolyNarrative taxonomy and highlighted all text spans supporting their selection. No fixed limit was imposed on the number or length of evidence spans. Curators resolved disagreements and standardized the final evidence annotations. Articles assigned the document-level label **“Other”** were excluded from the final dataset.

The split was stratified by language and, where possible, by dominant narrative. Some underrepresented classes were retained in the test set rather than proportionally distributed across splits.

### Dataset access

> **The PolyEvidence dataset is available upon request.** It contains news articles originating from third-party sources that are subject to **intellectual property and copyright restrictions**. For this reason, the complete dataset, including the article text, is **not made available as a public download**.

Researchers interested in accessing the dataset should contact **[nuno.r.guimaraes@inesctec.pt](mailto:nuno.r.guimaraes@inesctec.pt)**, preferably indicating their affiliation and intended research use. Requests will be considered in light of the applicable rights and sharing conditions. Access to the dataset does not grant permission to republish or redistribute third-party news content.

## Benchmark tasks

PolyEvidence supports two related tasks:

### Task 1 — Narrative-Aware Evidence Extraction

**Input:** A news article and its known document-level dominant narrative.  
**Output:** The textual spans supporting that dominant narrative.

This task evaluates whether a model can locate narrative-supporting passages when the relevant narrative is already specified.

### Task 2 — Joint Narrative–Evidence Extraction

**Input:** A news article and the set of candidate dominant narratives.  
**Output:** The predicted document-level dominant narrative **and** the textual spans supporting it.

This task combines document-level narrative classification with evidence extraction, without assuming that the correct dominant narrative is known in advance.

## Baselines and evaluation

The paper evaluates two model families:

- **mBERT:** a multilingual encoder fine-tuned for evidence extraction using BIO token tagging. For Task 1, the dominant narrative is prepended to the article. For Task 2, a shared encoder supports both narrative classification and token-level evidence extraction.
- **Gemini 2.5 Flash-Lite:** a generative baseline. Task 1 uses a dynamic few-shot prompt with an example retrieved from the training set by embedding similarity. Task 2 uses a two-stage pipeline: predict the narrative, then extract supporting evidence conditioned on that prediction.

Evidence extraction is evaluated using **strict span matching** and **relaxed matching** at character-overlap thresholds from **50% to 100%**. Precision, recall, and F1-score are reported.

### Main results

The following are the overall **strict-match F1-scores** reported in the paper:

| Model | Task 1: Evidence F1 | Task 2: Narrative F1 | Task 2: Evidence F1 |
| --- | ---: | ---: | ---: |
| mBERT | 0.13 | 0.39 | 0.11 |
| Gemini 2.5 Flash-Lite | 0.14 | 0.47 | 0.13 |

At the **50% evidence-overlap threshold**, the **appendix tables** report F1-scores of **0.31 (mBERT)** versus **0.34 (Gemini)** for Task 1, and **0.26 (mBERT)** versus **0.34 (Gemini)** for Task 2. Full results at each overlap threshold are reproduced in [Appendix E](#e-relaxed-evaluation-results).

These results are intended as initial benchmarks. Identifying exact narrative-supporting spans remains challenging, especially in multilingual settings and for underrepresented narrative categories.

## Online appendix

This section reproduces the supplementary material provided in the paper's **ARR appendix (A–E)**, including the complete domain-specific taxonomies, expanded annotation-agreement analysis, generative model prompts, fine-tuning settings, and the detailed results for both benchmark tasks. Figures are referenced from the local [`figures/`](figures/) directory. The documentation assets included with this README use the following structure:

```text
README.md
figures/
  evidence_span_agreement.png
  dominant_narrative_agreement_heatmap.png
```

### A. Narrative taxonomies

The dominant narrative is the category that best describes an entire article's framing or intended interpretation. The categories below follow the coarse-grained, domain-specific [PolyNarrative](https://aclanthology.org/2025.acl-long.1513/) taxonomy used for PolyEvidence.

#### A.1. Ukraine–Russia War (URW)

| Narrative | Definition |
| --- | --- |
| **Blaming the war on others rather than the invader** | Attributes responsibility or fault to entities other than Russia in the context of Russia's invasion of Ukraine. |
| **Discrediting Ukraine** | Undermines Ukraine's or Ukrainians' legitimacy, actions, or intentions. |
| **Russia is the Victim** | Portrays Russia as unfairly targeted or victimized. |
| **Praise of Russia** | Positively highlights Russia's actions, policies, or character. |
| **Overpraising the West** | Excessively praises the virtues, accomplishments, or moral superiority of Western countries, particularly in international relations and military contexts. |
| **Speculating war outcomes** | Predicts or makes assumptions about possible results or consequences of the conflict. |
| **Discrediting the West, Diplomacy** | Criticizes Western countries or international diplomatic efforts. |
| **Negative Consequences for the West** | Highlights or predicts adverse outcomes for Western countries and their interests. |
| **Distrust towards Media** | Questions the reliability or integrity of media organizations. |
| **Amplifying war-related fears** | Evokes fear or anxiety about potential threats, dangers, or reactions. |
| **Hidden plots by secret schemes of powerful groups** | Suggests covert plots or secretive actions by powerful groups connected to the war. |

#### A.2. Climate Change (CC)

| Narrative | Definition |
| --- | --- |
| **Criticism of climate policies** | Questions the effectiveness, economic impact, or underlying motives of climate policies. |
| **Criticism of institutions and authorities** | Challenges institutions' or authorities' competence, integrity, or intentions in relation to climate change. |
| **Climate change is beneficial** | Argues that changes in climate can also have positive effects. |
| **Downplaying climate change** | Minimizes the significance or impact of climate change. |
| **Questioning the measurements and science** | Expresses doubt about scientific methods, data, or consensus concerning climate change. |
| **Criticism of climate movement** | Challenges the motives, integrity, or effects of the climate movement. |
| **Controversy about green technologies** | Expresses skepticism or criticism of environmentally friendly technologies. |
| **Hidden plots by secret schemes of powerful groups** | Proposes covert agendas or hidden schemes connected to climate change and driven by powerful actors. |
| **Amplifying Climate Fears** | Emphasizes or amplifies fear of climate-change consequences. |
| **Green policies are geopolitical instruments** | Claims that environmental initiatives are instruments of geopolitical influence rather than genuine environmental concern. |

The taxonomies comprise **21 domain-specific narratives** (11 URW and 10 CC). An additional **Other** label was available during annotation but documents with **Other** as their final dominant narrative were removed from PolyEvidence.

#### A.3. Annotation and curation protocol

1. **Document-level narrative:** Annotators reviewed the article and its pre-existing paragraph-level narrative labels, choosing the single label that best represents the *entire document*. The most common paragraph-level label need not be the dominant narrative.
2. **Evidence spans:** Annotators selected all text spans justifying that narrative. Neither span length nor the number of spans per document was restricted.
3. **Expert curation:** Language coordinators and expert curators harmonized annotations and reviewed discrepancies, including differing or overlapping evidence spans. The final resource contains curated annotations.
4. **Excluded class:** Following curation, documents whose dominant narrative was **Other** were excluded.

Each language was annotated by a dedicated team of **3–6 annotators**, with a coordinator and expert curation. Inherent interpretation differences are important to consider when using the annotations.

### B. Annotation agreement

#### B.1. Document-level dominant narrative

Krippendorff's alpha (α) measures inter-annotator agreement on the dominant narrative:

| Language | BG | EN | PT | RU |
| --- | ---: | ---: | ---: | ---: |
| **Krippendorff's α** | **0.736** | **0.471** | **0.611** | **0.416** |

Figure 4 from the ARR manuscript visualizes where two annotators selected the same narrative (diagonal) and where their labels differed (off-diagonal). The heatmap includes the **Other** category used during annotation; this category is excluded from the final PolyEvidence dataset.

![Document-level dominant narrative annotation agreement matrix](figures/dominant_narrative_agreement_heatmap.png)

*Figure 4. Agreement matrix for document-level dominant narrative annotations.*

Outside the frequent **Other** category, the clearest agreements concern **Discrediting Ukraine**, **Amplifying Climate Fears**, and **Discrediting the West, Diplomacy**. Confusions occur between semantically close labels, notably **Blaming the war on others rather than the invader**, **Discrediting Ukraine**, and **Russia is the Victim**. These overlaps illustrate the importance of expert curation.

#### B.2. Evidence-span agreement

Evidence agreement is calculated **only for article pairs for which annotators chose the same dominant narrative**, since a different narrative could legitimately motivate different evidence. Span agreement is measured by precision, recall, and F1 under character-overlap thresholds from **0.5 (partial overlap)** through **1.0 (exact match)**.

![Evidence-span annotation agreement across overlap thresholds](figures/evidence_span_agreement.png)

*Figure 3. F1 agreement of evidence spans by language and topic across overlap thresholds.*

The results are generally low. In the most permissive setting, the highest F1 is **0.44** (approximately **0.443** with unrounded values), for **Portuguese URW**. English, Portuguese, and Russian generally show higher agreement than Bulgarian. Differences in the **number of spans** each annotator selected also contributed to disagreements: **19 documents** had a difference of more than five spans, whereas only **46** had the same number of spans annotated by both annotators.

<details>
<summary><strong>Table 5 — Full evidence annotation agreement by language, topic, and overlap threshold</strong></summary>

The table reproduces the values reported in the ARR appendix. P = precision; R = recall.

| Language | Topic | Overlap threshold | Precision | Recall | F1 |
| :--- | :--- | ---: | ---: | ---: | ---: |
| BG | CC | 0.5 | 0.36 | 0.11 | 0.16 |
| BG | CC | 0.6 | 0.32 | 0.10 | 0.14 |
| BG | CC | 0.7 | 0.28 | 0.09 | 0.12 |
| BG | CC | 0.8 | 0.23 | 0.08 | 0.11 |
| BG | CC | 0.9 | 0.19 | 0.06 | 0.08 |
| BG | CC | 1 | 0.15 | 0.05 | 0.06 |
| BG | URW | 0.5 | 0.50 | 0.16 | 0.23 |
| BG | URW | 0.6 | 0.46 | 0.15 | 0.22 |
| BG | URW | 0.7 | 0.39 | 0.14 | 0.19 |
| BG | URW | 0.8 | 0.32 | 0.12 | 0.16 |
| BG | URW | 0.9 | 0.27 | 0.09 | 0.13 |
| BG | URW | 1 | 0.21 | 0.06 | 0.09 |
| EN | CC | 0.5 | 0.37 | 0.35 | 0.35 |
| EN | CC | 0.6 | 0.32 | 0.31 | 0.30 |
| EN | CC | 0.7 | 0.32 | 0.31 | 0.30 |
| EN | CC | 0.8 | 0.28 | 0.28 | 0.26 |
| EN | CC | 0.9 | 0.25 | 0.25 | 0.23 |
| EN | CC | 1 | 0.13 | 0.16 | 0.14 |
| EN | URW | 0.5 | 0.34 | 0.36 | 0.33 |
| EN | URW | 0.6 | 0.25 | 0.27 | 0.24 |
| EN | URW | 0.7 | 0.20 | 0.21 | 0.19 |
| EN | URW | 0.8 | 0.20 | 0.20 | 0.18 |
| EN | URW | 0.9 | 0.16 | 0.18 | 0.16 |
| EN | URW | 1 | 0.08 | 0.10 | 0.09 |
| PT | CC | 0.5 | 0.39 | 0.37 | 0.36 |
| PT | CC | 0.6 | 0.34 | 0.30 | 0.31 |
| PT | CC | 0.7 | 0.28 | 0.24 | 0.25 |
| PT | CC | 0.8 | 0.26 | 0.23 | 0.24 |
| PT | CC | 0.9 | 0.24 | 0.20 | 0.21 |
| PT | CC | 1 | 0.17 | 0.14 | 0.15 |
| PT | URW | 0.5 | 0.47 | 0.47 | 0.44 |
| PT | URW | 0.6 | 0.42 | 0.42 | 0.40 |
| PT | URW | 0.7 | 0.36 | 0.36 | 0.34 |
| PT | URW | 0.8 | 0.32 | 0.33 | 0.31 |
| PT | URW | 0.9 | 0.29 | 0.29 | 0.27 |
| PT | URW | 1 | 0.19 | 0.20 | 0.19 |
| RU | URW | 0.5 | 0.44 | 0.45 | 0.39 |
| RU | URW | 0.6 | 0.38 | 0.39 | 0.34 |
| RU | URW | 0.7 | 0.35 | 0.35 | 0.31 |
| RU | URW | 0.8 | 0.33 | 0.34 | 0.29 |
| RU | URW | 0.9 | 0.30 | 0.32 | 0.27 |
| RU | URW | 1 | 0.23 | 0.24 | 0.21 |

</details>

### C. Generative baseline prompts

The generative baseline uses **Gemini 2.5 Flash-Lite**. For evidence extraction, a **dynamic few-shot** example is selected from the training set according to embedding cosine similarity to the current article. The extraction procedure uses **LangExtract** (Goel, 2026) to enforce structured, machine-readable span extraction.

#### C.1. Narrative-aware evidence extraction prompt

The following is the instruction text reproduced from the ARR appendix. The manuscript does not include the complete serialized LangExtract request or the retrieved few-shot example; accordingly, this text should be read as the **reported instruction**, not a standalone executable request.

```text
Extract all text spans that support and justify the selected dominant narrative:

Dominant Narrative:
{dominant_narrative}
```

#### C.2. Document-level dominant narrative classification prompt

In Task 2, the generative pipeline first predicts the dominant narrative using this prompt, then feeds the predicted category into the evidence-extraction stage.

```text
You are an expert annotator for narrative classification in news articles. Your task is to read the article text and assign exactly one dominant narrative category from the provided list of categories related to either the Ukraine-Russia War (URW) or Climate Change (CC).

A dominant narrative is the category that best captures the article's main framing, viewpoint, or overall message. Focus on the article as a whole, not on isolated phrases or minor mentions.

Instructions:
- Use only the information in the article text.
- Do not rely on outside knowledge.
- Do not explain your answer.
- Do not include evidence, reasoning, or confidence.
- Choose the most specific applicable category from the provided list.
- If multiple categories appear, choose the one that best represents the article's overall purpose.

Output only the category name, exactly as written in the provided category list.

Example Article: {example_text}

Example Dominant Narrative: {example_narrative}

Article: {article}

Available categories: {narratives_list}
```

### D. mBERT fine-tuning details

Both mBERT baselines are fine-tuned from `bert-base-multilingual-cased` using the following hyperparameters, as reported in Appendix D:

| Setting | Value |
| --- | --- |
| Encoder | `bert-base-multilingual-cased` |
| Learning rate | `2e-5` |
| Batch size | `16` |
| Maximum sequence length | `512` tokens |
| Sliding-window stride | `128` tokens |
| Optimizer | AdamW |
| Weight decay | `0.01` |
| Checkpoint selection | Best validation F1-score |
| Joint-task loss weight | `α = 0.5` for each objective |

For **Task 1**, the dominant narrative is included in the encoder input (`[CLS] narrative [SEP] article [SEP]`), and token-level BIO labels identify evidence spans. For **Task 2**, a shared encoder feeds a narrative classification head and a BIO token classification head. The latter model uses the combined objective:

$$
\mathcal{L} = \alpha\mathcal{L}_{\mathrm{narrative}} + (1-\alpha)\mathcal{L}_{\mathrm{evidence}}, \qquad \alpha=0.5.
$$

### E. Relaxed evaluation results

The appendix reports **precision (P), recall (R), and F1** at minimum overlap thresholds **0.5–1.0**. A threshold of **1.0** denotes exact span matching. For readability, each model's three scores are grouped as **P / R / F1**, preserving every numerical result from the ARR tables.

#### E.1. Narrative-Aware Evidence Extraction (Task 1)

In Task 1, the gold document-level dominant narrative is provided as an input and the model identifies supporting text spans.

<details>
<summary><strong>Table 6 — Full Task 1 results by language and overlap threshold</strong></summary>

| Language | Overlap threshold | Gemini (P / R / F1) | mBERT (P / R / F1) |
| :--- | ---: | :---: | :---: |
| BG | 0.5 | 0.16 / 0.71 / 0.27 | 0.24 / 0.15 / 0.18 |
| BG | 0.6 | 0.13 / 0.54 / 0.20 | 0.17 / 0.10 / 0.13 |
| BG | 0.7 | 0.10 / 0.43 / 0.16 | 0.17 / 0.10 / 0.13 |
| BG | 0.8 | 0.10 / 0.41 / 0.15 | 0.15 / 0.09 / 0.11 |
| BG | 0.9 | 0.09 / 0.37 / 0.14 | 0.12 / 0.07 / 0.09 |
| BG | 1 | 0.05 / 0.21 / 0.08 | 0.07 / 0.04 / 0.06 |
| EN | 0.5 | 0.15 / 0.60 / 0.24 | 0.24 / 0.23 / 0.24 |
| EN | 0.6 | 0.14 / 0.56 / 0.22 | 0.23 / 0.22 / 0.23 |
| EN | 0.7 | 0.11 / 0.47 / 0.18 | 0.21 / 0.20 / 0.20 |
| EN | 0.8 | 0.11 / 0.43 / 0.17 | 0.18 / 0.18 / 0.18 |
| EN | 0.9 | 0.10 / 0.41 / 0.16 | 0.16 / 0.16 / 0.16 |
| EN | 1 | 0.05 / 0.21 / 0.08 | 0.09 / 0.09 / 0.09 |
| PT | 0.5 | 0.32 / 0.66 / 0.43 | 0.36 / 0.41 / 0.39 |
| PT | 0.6 | 0.28 / 0.57 / 0.37 | 0.32 / 0.36 / 0.34 |
| PT | 0.7 | 0.24 / 0.50 / 0.33 | 0.25 / 0.29 / 0.27 |
| PT | 0.8 | 0.20 / 0.42 / 0.27 | 0.22 / 0.25 / 0.24 |
| PT | 0.9 | 0.17 / 0.36 / 0.23 | 0.22 / 0.24 / 0.23 |
| PT | 1 | 0.11 / 0.23 / 0.15 | 0.13 / 0.15 / 0.14 |
| RU | 0.5 | 0.27 / 0.76 / 0.39 | 0.22 / 0.32 / 0.26 |
| RU | 0.6 | 0.26 / 0.75 / 0.39 | 0.22 / 0.32 / 0.26 |
| RU | 0.7 | 0.24 / 0.69 / 0.36 | 0.20 / 0.30 / 0.24 |
| RU | 0.8 | 0.23 / 0.65 / 0.34 | 0.20 / 0.28 / 0.23 |
| RU | 0.9 | 0.21 / 0.60 / 0.31 | 0.19 / 0.27 / 0.22 |
| RU | 1 | 0.18 / 0.51 / 0.26 | 0.16 / 0.24 / 0.19 |
| Overall | 0.5 | 0.23 / 0.67 / 0.34 | 0.29 / 0.32 / 0.31 |
| Overall | 0.6 | 0.20 / 0.60 / 0.30 | 0.26 / 0.29 / 0.27 |
| Overall | 0.7 | 0.18 / 0.52 / 0.26 | 0.23 / 0.24 / 0.23 |
| Overall | 0.8 | 0.16 / 0.46 / 0.24 | 0.20 / 0.22 / 0.21 |
| Overall | 0.9 | 0.14 / 0.42 / 0.21 | 0.19 / 0.21 / 0.20 |
| Overall | 1 | 0.09 / 0.28 / 0.14 | 0.13 / 0.14 / 0.13 |

</details>

#### E.2. Joint Narrative–Evidence Extraction (Task 2)

In Task 2, the model predicts the document-level narrative and supporting evidence. The results below are for the **evidence-extraction component**; narrative classification is a categorical prediction, not a span-overlap task.

<details>
<summary><strong>Table 7 — Full Task 2 evidence results by language and overlap threshold</strong></summary>

| Language | Overlap threshold | Gemini (P / R / F1) | mBERT (P / R / F1) |
| :--- | ---: | :---: | :---: |
| BG | 0.5 | 0.15 / 0.68 / 0.25 | 0.09 / 0.18 / 0.13 |
| BG | 0.6 | 0.12 / 0.53 / 0.20 | 0.09 / 0.18 / 0.13 |
| BG | 0.7 | 0.09 / 0.41 / 0.15 | 0.09 / 0.18 / 0.13 |
| BG | 0.8 | 0.09 / 0.38 / 0.14 | 0.08 / 0.16 / 0.11 |
| BG | 0.9 | 0.07 / 0.32 / 0.12 | 0.08 / 0.16 / 0.11 |
| BG | 1 | 0.04 / 0.18 / 0.07 | 0.05 / 0.11 / 0.07 |
| EN | 0.5 | 0.14 / 0.61 / 0.23 | 0.15 / 0.18 / 0.16 |
| EN | 0.6 | 0.12 / 0.53 / 0.20 | 0.12 / 0.14 / 0.13 |
| EN | 0.7 | 0.10 / 0.44 / 0.17 | 0.11 / 0.13 / 0.12 |
| EN | 0.8 | 0.10 / 0.41 / 0.16 | 0.10 / 0.12 / 0.11 |
| EN | 0.9 | 0.09 / 0.39 / 0.15 | 0.09 / 0.10 / 0.09 |
| EN | 1 | 0.05 / 0.21 / 0.08 | 0.03 / 0.04 / 0.04 |
| PT | 0.5 | 0.32 / 0.67 / 0.44 | 0.31 / 0.27 / 0.29 |
| PT | 0.6 | 0.29 / 0.60 / 0.39 | 0.29 / 0.26 / 0.28 |
| PT | 0.7 | 0.24 / 0.50 / 0.33 | 0.24 / 0.21 / 0.23 |
| PT | 0.8 | 0.21 / 0.42 / 0.28 | 0.21 / 0.19 / 0.20 |
| PT | 0.9 | 0.18 / 0.36 / 0.24 | 0.18 / 0.16 / 0.17 |
| PT | 1 | 0.11 / 0.23 / 0.15 | 0.11 / 0.10 / 0.11 |
| RU | 0.5 | 0.26 / 0.73 / 0.39 | 0.35 / 0.33 / 0.34 |
| RU | 0.6 | 0.26 / 0.72 / 0.38 | 0.35 / 0.33 / 0.34 |
| RU | 0.7 | 0.23 / 0.65 / 0.34 | 0.32 / 0.29 / 0.31 |
| RU | 0.8 | 0.22 / 0.61 / 0.33 | 0.31 / 0.28 / 0.30 |
| RU | 0.9 | 0.21 / 0.58 / 0.31 | 0.31 / 0.28 / 0.30 |
| RU | 1 | 0.19 / 0.52 / 0.28 | 0.24 / 0.22 / 0.23 |
| Overall | 0.5 | 0.22 / 0.67 / 0.34 | 0.25 / 0.26 / 0.26 |
| Overall | 0.6 | 0.20 / 0.60 / 0.30 | 0.24 / 0.25 / 0.24 |
| Overall | 0.7 | 0.17 / 0.51 / 0.25 | 0.21 / 0.21 / 0.21 |
| Overall | 0.8 | 0.15 / 0.45 / 0.23 | 0.19 / 0.19 / 0.19 |
| Overall | 0.9 | 0.14 / 0.40 / 0.20 | 0.17 / 0.17 / 0.17 |
| Overall | 1 | 0.09 / 0.27 / 0.14 | 0.11 / 0.11 / 0.11 |

</details>

**Key observation:** Both approaches improve under relaxed span matching. For the overall score at threshold **0.5**, Gemini reaches an evidence F1 of **0.34** in both tasks; mBERT reaches **0.31** in Task 1 and **0.26** in Task 2. Exact-match evidence F1 remains low, demonstrating the difficulty of delineating spans with precise boundaries.

## Limitations

Narrative identification and supporting-evidence selection are inherently subjective. The annotation agreement results reflect this difficulty, despite expert annotation and curation. PolyEvidence also covers only four languages and two news domains; some narrative classes have few examples. Furthermore, evidence spans identify passages supporting a document's narrative, rather than independently verifying their factual correctness.

## Citation

If you use PolyEvidence in your research, please cite the paper. The bibliographic details will be updated when the final publication record is available.

```bibtex
@misc{guimaraes2026polyevidence,
  title  = {PolyEvidence: Enhancing PolyNarrative for Evidence Detection from Document-Level Dominant Narratives},
  author = {Guimarães, Nuno and Cunha, Luís Filipe and Silvano, Purificação and Campos, Ricardo and Jorge, Alípio and Dimitrov, Dimitar Iliyanov and Nikolaidis, Nikolaos and Yangarber, Roman and Sartori, Elisa and Stefanovitch, Nicolas and Nakov, Preslav and Piskorski, Jakub and Da San Martino, Giovanni},
  year   = {2026},
  note   = {Manuscript; publication details forthcoming}
}
```

## Contact

For dataset access, questions, or research collaboration, contact **Nuno Guimarães** at [nuno.r.guimaraes@inesctec.pt](mailto:nuno.r.guimaraes@inesctec.pt).
