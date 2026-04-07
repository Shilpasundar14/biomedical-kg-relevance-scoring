# Manual Scoring Rubric (1–3) — Simple, Reproducible

## How to Use

1. For each source, skim docs/schema, a sample of records/edges, and note what you **see**.
2. For A/B/C, pick 1, 2, or 3 using the definitions below. Keep notes short but concrete (IDs you saw, example edge).
3. Apply D noise penalty if the source has substantial weaknesses.
4. The "Suggested Label (auto)" is just a helper — you still choose the final decision.

**Scale (all three criteria):**
- `1` = Low (bare minimum / indirect / hard to use)
- `2` = Medium (workable / decent / needs some guardrails)
- `3` = High (strong / directly useful / easy to plug in)

> The D penalty (−1, −2, or 0) can reduce a score, but scores cannot go below 1.

---

## A) Domain Coverage (1–3)
*What's in there? (entities + relation families + basic descriptors)*

Judge only the **types** of things and link types present inside the source, plus whether records carry basic IDs/xrefs. Do **not** consider modeling effort or mapping friction here — that belongs in C.

### Give **1** when:
- You mainly see a single entity type (just genes OR just diseases OR just drugs), or a vocabulary without useful links.
- Relations relevant to repurposing are missing or very thin (e.g., a handful of D–T only, or generic "associated with" everywhere).
- Descriptors are sparse: IDs/xrefs/synonyms are largely missing, limiting linkage potential across sources.

*Exclude from A: any mention of direction/effect, citations/confidence, or "hard to map" (that goes in C).*

**Examples likely to be 1:** A bare term list; a taxonomy; a small expression table without links.

**What to write:** `"Entity seen: genes only; no D–T/T–D/D–D. Xrefs rare."`

---

### Give **2** when:
- You see at least two core entities (e.g., drugs + targets, or targets + diseases).
- There is at least one consistent relation family at a workable scale (e.g., D–T or T–D) that shows up consistently across many records.
- Descriptors exist and are usable (IDs/xrefs on many entries).

*Exclude from A: counts/size ("how big") → that's B. Evidence/direction/mapping → C.*

**Examples likely to be 2:** IntAct (PPI) with usable mappings; DisGeNET gene–disease associations; an ontology with rich xrefs but few actionable relations.

**What to write:** `"Entities: drug, target; Relation: D–T consistently present; IDs: DrugBank/UniProt present."`

---

### Give **3** when:
- You clearly see all three core entities (drug, target/gene, disease) **and** multiple relation families (e.g., D–T + T–D, or D–D + AE).
- Descriptors are rich (IDs/synonyms/xrefs across most records).
- Multi-hop paths (D→T→D) are clearly supported by what's present.

*Exclude from A: do not cite PMIDs, direction, or mapping ease — those are C.*

**Examples likely to be 3:** DrugBank/ChEMBL/BindingDB + robust xrefs; Reactome/SIGNOR with directed relations and mappings; aggregate KGs that genuinely stitch drug–target–disease at scale.

**What to write:** `"Entities: drug, target, disease; Relations: D–T, T–D, D–D/AE; xrefs common."`

---

## B) Source Scope (1–3)
*Topical breadth & scale (small → large)*

How broad and large the source is across diseases/targets/drugs/species/modalities. Do **not** judge relation quality or edge direction/quality here.

### Give **1** when:
- Niche scope (single disease/phenotype/organism) or small scale (roughly a few hundred core entities / <~10k edges).
- Covers a limited therapeutic area or one pathway family; relation types are one-note.
- Mostly human-only or a single model; single modality (e.g., small molecules only).

*Exclude from B: whether D–T exists (that's A) or whether edges have evidence/direction (that's C).*

**Examples likely to be 1:** A disease-specific KG (e.g., Alzheimer's-only), a small rare-disease annotation set, a narrowly focused pathway slice.

**What to write:** `"Niche scope: Alzheimer-focused; ~2k edges; human-only; one relation family."`

---

### Give **2** when:
- Multi-disease or multi-organism coverage at mid-scale (tens of thousands of edges; hundreds–thousands of drugs/targets/diseases).
- Relation types cover more than one family but not exhaustive; some modality breadth.
- Useful across several areas but not a backbone resource.

**Examples likely to be 2:** DisGeNET, IntAct (human+model PPIs), GOA, CTD, many pathway DBs (Reactome subsets, KEGG modules, Panther).

**What to write:** `"Multi-disease, multi-organism; ~150k edges; thousands of proteins; moderate drug/disease breadth."`

---

### Give **3** when:
- Broad, general-purpose resource spanning many diseases and targets (often multi-species); hundreds of thousands to millions of edges; thousands–tens of thousands of entities.
- Multiple relation subtypes within and across domains are present (broad domain coverage — but do not judge quality/direction, that's C).
- Covers multiple modalities (small molecules, biologics, peptides) and serves as a backbone.

*Exclude from B: don't justify with "has D–T/T–D" (that's A) or "has PMIDs/'inhibits'" (that's C).*

**Examples likely to be 3:** BindingDB, ChEMBL, DrugBank, STRING (full), Reactome (full), UMLS/MeSH/NCIt, RxNorm, Monarch, Ubergraph.

**What to write:** `"Broad, cross-disease; human + model species; 1.2M interactions; 20k proteins; 6k drugs; multiple relation subtypes."`

---

## C) Utility for Drug↔Disease Modeling (1–3)
*How directly does it power D→D right now?*

Direct edges like D–T, T–D, D–D/indication, AE/contra; plus direction/effect, evidence/confidence, negatives/clinical context, and mapping ease to canonical IDs.

### Give **1** when:
- It's mainly indirect context (pure terminology, undirected pathways, generic phenotypes) with no clear D–T/T–D/D–D/AE edges.
- Or there are some edges but they're too sparse/noisy/poorly mapped to rely on without heavy filtering.

*Exclude from C: statements about breadth/size (that's B) or mere presence of entity types (that's A).*

**Examples likely to be 1:** Ontologies alone (HPO/NCIt) without annotated relations; raw text-mined triples without citations/confidence.

**What to write:** `"Indirect only; no D–T/T–D; edges generic/undirected; mapping unclear."`

---

### Give **2** when:
- There is at least one direct edge family (D–T or T–D or D–D/indication or AE) used at scale in practice.
- IDs align reasonably; many edges have citations or confidence scores.
- Direction/effect is present sometimes, or negatives/clinical context exist in parts (e.g., AEs).

**Examples likely to be 2:** DisGeNET (T–D), IntAct/STRING (interactions), CTD (chem–disease), SIDER (AEs) when mappings are OK.

**What to write:** `"Direct edges: D–T ~40k; PMIDs present often; direction partial; RxNorm/UniProt mapped."`

---

### Give **3** when:
- **Multiple** direct edge families are strong (e.g., D–T + T–D, or D–D + AE).
- Direction/effect is clear for most edges (activates/inhibits/causes/protects) and evidence/confidence is routine.
- Negatives and clinical context exist (AEs/contraindications, indications, subpopulations); IDs are cleanly mapped — plug-and-play for repurposing.

**Examples likely to be 3:** DrugBank/ChEMBL/BindingDB/DrugCentral; Reactome/SIGNOR when relations are directed with effect; high-quality AE sources mapped to RxNorm/MeSH.

**What to write:** `"Direct edges: D–T, T–D, D–D/AE; effect='inhibits'; PMIDs/confidence standard; clean RxNorm/HGNC/UniProt/MONDO."`

---

## D) Noise Penalty (0, −1, or −2)

A source can look "big" (B=3) or "rich" (A=2) but still be dominated by irrelevant/generic/noisy associations. The noise penalty captures this.

### Baseline precision thresholds
| Precision | Penalty |
|-----------|---------|
| ≥ 85% (curated or benchmarked) | 0 |
| 70–85% | −1 |
| < 70% | −2 |

If no benchmark precision is published, fall back to:

### Agent type of edges
- Curated/manual (DrugBank, BindingDB, OMIM) → **0**
- Confidence-weighted predictions (STRING experimental, DISEASES with star scores) → **0 to −1**
- Purely text-mined/NLP-extracted with no confidence scoring (SemMedDB, raw triples) → **−2**

### Knowledge level of assertions
- Mechanistic/functional (inhibits, activates, binds, causes, prevents) → lower noise risk
- Generic/correlative ("associated with," "related to") → apply penalty (−1 if some mix, −2 if majority)

### Irrelevant scale inflation
Source contains many edges not useful for repurposing (procedures, billing codes, generic anatomy links, administrative terms): −1 if substantial but not dominant, −2 if majority.

### How to score
- **0 (No penalty):** Data are curated, confidence-scored, or benchmarked ≥85% precision. Example: DrugBank, BindingDB, STRING (experiment channel).
- **−1 (Moderate noise):** Substantial fraction of edges are generic/ambiguous, or baseline precision 70–85%. Filtering/thresholding recovers strong signal. Example: DISEASES (mix of curated + text-mined, 4–5 star subsets usable).
- **−2 (High noise):** Majority of edges are text-mined, generic, or <70% precision; requires heavy filtering. Example: SemMedDB, unscored text-mined triples dominated by ASSOCIATED_WITH/RELATED_TO.

**What to write:** `"Noise −1: text-mined edges ~80% precision; ASSOCIATED_WITH dominates. Need frequency/confidence filter."`

---

## Core Edge Family Flag (boolean)

Auto-detected from scorer comments. Set to `TRUE` if the source contains any of:
- Drug–Target (D–T) interactions
- Target–Drug (T–D) associations
- Indication (drug–disease)
- Adverse Event (AE) / contraindication
- Binding affinity
- GWAS associations
- Bioactivity data

---

## Automated Label Logic

```
HIGH   = (C = 3 AND D = 0)
         OR (C = 2 AND A ≥ 2 AND D = 0 AND CoreFlag = TRUE)

MEDIUM = (C = 2 AND D = −1)
         OR (C = 2 AND D = 0 AND CoreFlag = FALSE)
         OR (C = 1 AND A = 3 AND B ≥ 2 AND D = 0)
         OR (C = 1 AND B ≥ 2 AND D = 0 AND CoreFlag = TRUE)

LOW    = All remaining cases, including any source with D = −2
```

---

## Keeping A / B / C Distinct — Quick Reference

| Criterion | One-liner | Count | Don't Count |
|-----------|-----------|-------|-------------|
| **A** Domain Coverage | *What object types and link types are inside the source, and do records carry basic IDs/xrefs?* | Entity types present; relation families present; IDs/xrefs/synonyms on records | Scale/size (→B); evidence strength/direction (→C); engineering integration effort (→C) |
| **B** Source Scope | *How much of the biomedical world does it cover (breadth/scale), regardless of edge quality?* | Breadth across therapeutic areas, organisms, modalities; order-of-magnitude of edges/entities | What kinds of relations exist (→A); whether edges are directed/evidenced/mapped (→C) |
| **C** Utility | *How plug-and-play is it for D→D modeling right now?* | Direction/effect ("inhibits/activates"); PMIDs/scores; AE/contra indications; mapping effort | General breadth/size (→B); mere existence of relation families without directness/evidence (→A) |

**Bucket test:**
- Mentions entity types or relation families present → **A**
- Mentions how broad/large (disease classes, species, modalities, edge/entity magnitudes) → **B**
- Mentions direction/effect, PMIDs/confidence, AEs/contra, or mapping effort → **C**
