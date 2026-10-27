# Gahwa: A Computational Lexicon for Emirati Arabic

## About

This repository contains Gahwa v1.0, described in our [ArabicNLP 2026 paper](https://aclanthology.org/2026.arabicnlp-main.16/).

Gahwa is a machine-readable lexicon constructed by extracting, enriching, and standardizing evidence from an annotated Emirati corpus, a Gulf Arabic verb database, and a Gulf Arabic Wiktionary, unifying their input into one lemma-POS keyed scheme.

## Download

- [TSV](../../../data/v1.0/gahwa-v1.0.tsv)
- [Excel](../../../data/v1.0/gahwa-v1.0.xlsx)

Both files contain the same entries and columns, sorted by Index. The TSV is UTF-8 encoded, with a header row and tab-separated fields.

## Data Format

| Column | Description |
| --- | --- |
| Source | Original source of the lexical entry. |
| Index | Unique record identifier. |
| Root | Templatic consonantal root or NTWS (non-templatic word stem). |
| Type | Lemma if Entry and Lemma are identical; Word otherwise. |
| Entry | Base word form represented by the record. |
| Lemma | Lemma linked to the represented form. |
| CAMeL_POS | Fine-grained part-of-speech morphological category. |
| POS | Core part-of-speech category. |
| English_Gloss | English gloss of the lemma. |
| MSA_Lemma | Modern Standard Arabic lemma equivalent. |
| CAMeLBERT_Freq | Lemma frequency from CAMeLBERT-Mix counts. |
| Source_ID | Identifier in the source resource. N/A indicates a missing AEWiki identifier. |
| Root Verified | TRUE for AEWiki/AE Verb Lexicon roots or completed root review; FALSE for estimated roots. Blank indicates unverified. |
| MSA Lemma Verified | TRUE when the retained MSA equivalent matches the completed annotation; FALSE otherwise. |

## Statistics

| Inventory | Count |
| --- | ---: |
| Gumar Ann records | 24,607 |
| AE Verb Lexicon records | 7,924 |
| Gulf Wiktionary records | 1,161 |
| Consolidated records | 33,687 |
| Distinct lemmas (L) | 8,890 |
| Distinct lemma–POS pairs (LP) | 9,339 |
| Distinct lemma–POS–gloss triples (LPG) | 10,456 |

The distinct lexical-entry unit in Gahwa is lemma-POS-gloss (LPG). The lemma gives the lexical head, POS separates category-specific uses, and the normalized English gloss distinguishes meanings that share the same lemma and POS.

## Evaluation

We evaluate Gahwa through automatic coverage analysis of three Emirati datasets, GumarAE, Mixat, and the Ramsa Corpus, against existing Arabic morphological resources, and through analysis of out-of-vocabulary (OOV) forms.

### Datasets

To our knowledge, no independent Emirati lexical gold standard is available. We therefore use three Emirati datasets as in-domain diagnostics of practical lexical coverage. For all of these sets, we remove punctuation- or symbol-only items, digit-containing items, foreign-language or web markers, and Arabic strings with attached symbols.

| Dataset | Tokens | Types |
| --- | ---: | ---: |
| GumarAE | 9,052,103 | 442,230 |
| Mixat | 91,865 | 15,636 |
| Ramsa Corpus | 26,489 | 7,873 |

### Coverage

GLF uses `calima-glf-01`, EGY uses `calima-egy-r13` (only analyses marked `[CALIMA]`), and MSA uses `camel-morph-msa-v1.2.2`. ANY is their union.

A type is in-vocabulary (INV) if any analyzer returns an analysis, and out-of-vocabulary (OOV) otherwise. Because the CAMeL Tools GLF analyzer does not provide English glosses, we evaluate Gahwa at the Lemma-POS (LP) level. POS matching is largely a case-insensitive identity mapping between the two tagsets.

We use two LP matching conditions: Exact compares lemma strings as given, whereas dediacritized matching (Dediac) strips Arabic diacritics before comparison. LP-match columns use analyzer-covered output as the denominator, while effective coverage uses the full evaluation corpus. All values below are percentages.


#### GumarAE

| Analyzer | INV types | INV tokens | Gahwa types Exact | Gahwa types Dediac | Gahwa tokens Exact | Gahwa tokens Dediac | Effective Exact | Effective Dediac |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| GLF | 37.5 | 83.7 | 77.7 | 93.5 | 84.4 | 95.4 | 70.6 | 79.8 |
| EGY | 35.9 | 79.3 | 60.8 | 75.7 | 76.7 | 89.5 | 60.8 | 71.0 |
| MSA | 52.0 | 87.3 | 48.8 | 69.1 | 63.8 | 86.8 | 55.7 | 75.7 |
| ANY | 63.3 | 93.3 | 71.3 | 80.2 | 92.6 | 95.2 | 86.4 | 88.8 |
| ANY + RepNorm | 84.4 | 97.3 | 71.3 | 80.9 | 92.0 | 94.8 | 89.5 | 92.2 |

#### Mixat

| Analyzer | INV types | INV tokens | Gahwa types Exact | Gahwa types Dediac | Gahwa tokens Exact | Gahwa tokens Dediac | Effective Exact | Effective Dediac |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| GLF | 70.8 | 89.6 | 78.9 | 94.8 | 83.5 | 94.0 | 74.9 | 84.3 |
| EGY | 75.8 | 91.0 | 67.1 | 81.5 | 75.5 | 90.1 | 68.7 | 82.0 |
| MSA | 87.8 | 95.7 | 58.6 | 78.6 | 65.0 | 86.7 | 62.2 | 82.9 |
| ANY | 95.6 | 98.8 | 80.7 | 86.6 | 92.8 | 95.3 | 91.6 | 94.1 |
| ANY + RepNorm | 95.9 | 98.8 | 80.6 | 86.5 | 92.7 | 95.3 | 91.7 | 94.2 |

#### Ramsa Corpus

| Analyzer | INV types | INV tokens | Gahwa types Exact | Gahwa types Dediac | Gahwa tokens Exact | Gahwa tokens Dediac | Effective Exact | Effective Dediac |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| GLF | 66.6 | 81.7 | 78.1 | 94.2 | 82.9 | 94.3 | 67.7 | 77.1 |
| EGY | 76.8 | 88.1 | 64.0 | 77.0 | 72.6 | 86.2 | 64.0 | 76.0 |
| MSA | 90.7 | 96.3 | 54.7 | 75.3 | 60.8 | 83.6 | 58.6 | 80.6 |
| ANY | 95.4 | 98.3 | 76.9 | 82.6 | 86.5 | 90.5 | 85.0 | 89.0 |
| ANY + RepNorm | 95.8 | 98.4 | 76.8 | 82.6 | 86.5 | 90.5 | 85.1 | 89.1 |

### Repeated-letter Normalization

We apply repeated-letter normalization (RepNorm) to all OOV forms by progressively collapsing repeated character sequences. We first reduce each repeated sequence to at most two characters; if the resulting form remains OOV, we reduce it further to a single character. The table compares ANY before and after RepNorm.

| Dataset | INV types before | INV types after | INV tokens before | INV tokens after | Effective Dediac before | Effective Dediac after |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| GumarAE | 63.3 | 84.4 | 93.3 | 97.3 | 88.8 | 92.2 |
| Mixat | 95.6 | 95.9 | 98.8 | 98.8 | 94.1 | 94.2 |
| Ramsa Corpus | 95.4 | 95.8 | 98.3 | 98.4 | 89.0 | 89.1 |

We acknowledge that these matches may not necessarily be the correct analysis in context, but represent a possible analysis nonetheless.

One resource used to construct Gahwa, Gumar Ann, is derived from material related to GumarAE, although it represents only about 1.5% of its size. Coverage estimates also depend on the available morphological analyzers, whose own lexical limitations may underestimate the true coverage of Gahwa.

### OOV Analysis

GumarAE is based on a systematic sample of 1,000 residual types (8,623 tokens), while the smaller Ramsa and Mixat datasets include all remaining OOV types: 331 (416 tokens) and 638 (1,070 tokens), respectively.

All types included in the three error-analysis sets are manually checked and assigned a single dominant error category by the authors. For Ramsa and Mixat, preliminary LLM-assisted labels were used as a starting point, but every label was subsequently manually verified and corrected using the same categories and criteria applied to GumarAE. When multiple phenomena overlap, the category judged to be the primary cause of analyzer failure is selected.

Percentages refer to the reviewed OOV sets.

| Category | GumarAE tokens % | GumarAE types % | Ramsa tokens % | Ramsa types % | Mixat tokens % | Mixat types % |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Dialect / lexical gap | 78.6 | 33.3 | 45.9 | 43.8 | 51.5 | 43.1 |
| Orthographic / spelling variation | 6.3 | 21.4 | 12.2 | 14.2 | 8.4 | 11.6 |
| Fused expression / tokenization | 6.8 | 21.0 | 4.5 | 5.1 | 16.2 | 13.1 |
| Dialect morphology / clitic gap | 4.3 | 11.6 | 17.7 | 19.6 | 15.3 | 21.9 |
| Proper name / named entity | 1.6 | 3.3 | 3.8 | 2.7 | 1.7 | 2.1 |
| Loanword / foreign form | 1.3 | 4.3 | 11.0 | 10.2 | 5.3 | 6.1 |
| Unclear / ambiguous | 0.7 | 5.1 | 4.5 | 4.2 | 1.3 | 1.8 |

## Citation

If you use Gahwa, please cite:

```bibtex
@inproceedings{alzaabi-etal-2026-gahwa,
  title = {Gahwa: A Computational Lexicon for Emirati Arabic},
  author = {Alzaabi, Saeed and Khalifa, Salam and Saeed, Mostafa and
            Dibas, Shahd and Aldarmaki, Hanan and Habash, Nizar},
  booktitle = {Proceedings of The Fourth Arabic Natural Language Processing Conference},
  year = {2026},
  pages = {219--227},
  publisher = {Association for Computational Linguistics},
  url = {https://aclanthology.org/2026.arabicnlp-main.16/}
}
```

## License

See [LICENSE](../../../LICENSE) for the full license text.

## Contact

- Saeed Alzaabi: [saeed.alzaabi@nyu.edu](mailto:saeed.alzaabi@nyu.edu)
- Salam Khalifa: [salam.khalifa@nyu.edu](mailto:salam.khalifa@nyu.edu)
- Mostafa Saeed: [mostafa.saeed@nyu.edu](mailto:mostafa.saeed@nyu.edu)
- Shahd Dibas: [shahd.dibas@mbzuai.ac.ae](mailto:shahd.dibas@mbzuai.ac.ae)
- Hanan Aldarmaki: [hanan.aldarmaki@mbzuai.ac.ae](mailto:hanan.aldarmaki@mbzuai.ac.ae)
- Nizar Habash: [nizar.habash@nyu.edu](mailto:nizar.habash@nyu.edu)
