# Gahwa: A Computational Lexicon for Emirati Arabic

## About

Gahwa is a machine-readable lexicon for Emirati Arabic. It combines and enriches lexical records from three existing resources: **Gumar Ann**, the annotated Emirati subset of the Gumar corpus; the **AE Verb Lexicon**; and **Gulf Wiktionary**.

Each record links an entry form to its lemma and includes core POS and CAMeL POS, root information, English glosses, an MSA lemma equivalent, CAMeLBERT-Mix lemma frequency, and source information. The lexicon retains entry-level forms and distinguishes lexical entries by lemma, POS, and English gloss.

Gahwa is introduced in our [ArabicNLP 2026 paper](https://aclanthology.org/2026.arabicnlp-main.16/). **v1.1 is the latest version.**

## Download

### v1.1 — Latest version

- [TSV](data/v1.1/Gahwa-v1.1.tsv)
- [Excel](data/v1.1/Gahwa-v1.1.xlsx)

Both files contain the same 28,661 records and 14 columns. The TSV is UTF-8 encoded, with a header row and tab-separated fields.

## Data Format

| Column | Description |
| --- | --- |
| Source | Original source of the lexical entry. |
| Index | Sequential record number within this release. |
| Root | Arabic root letters separated by dots, or NTWS (non-templatic word stem). |
| Type | LEMMA, WORD, or PHRASE. |
| Entry | Base word form represented by the record. |
| Lemma | Lemma linked to the represented form. |
| CAMeL_POS | Fine-grained part-of-speech morphological category. |
| POS | Core part-of-speech category. |
| English_Gloss | English gloss of the lemma. |
| MSA_Lemma | Modern Standard Arabic lemma equivalent. |
| CAMeLBERT_Freq | Lemma frequency from CAMeLBERT-Mix counts. |
| Source_ID | Identifier in the source resource; N/A when unavailable. |
| Root Verified | TRUE when marked verified; blank otherwise. |
| MSA Lemma Verified | TRUE when marked verified; blank otherwise. |

Spaces in multiword entries and English gloss expressions are represented by underscores. Source attribution and root alternatives are retained when records are merged.

## Statistics

| Inventory | v1.1 |
| --- | ---: |
| Entries | 28,661 |
| Distinct lemmas (L) | 8,620 |
| Distinct lemma–POS pairs (LP) | 9,159 |
| Distinct lemma–POS–gloss triples (LPG) | 11,332 |
| Records attributed to Gumar Ann | 20,511 |
| Records attributed to AE Verb Lexicon | 7,788 |
| Records attributed to Gulf Wiktionary | 1,247 |

L, LP, and LPG count distinct values of Lemma, Lemma + POS, and Lemma + POS + English_Gloss, respectively, using the released fields as stored. LPG uses the complete English_Gloss field.

Source counts refer to retained records carrying each source label. Records attributed to more than one source are counted under each source, so these counts overlap.

## Evaluation

We evaluate Gahwa through automatic coverage analysis of three Emirati datasets, GumarAE, Mixat, and the Ramsa Corpus, against existing Arabic morphological resources, and through analysis of out-of-vocabulary (OOV) forms.

### Datasets

To our knowledge, no independent Emirati lexical gold standard is available. We therefore use three Emirati datasets as in-domain diagnostics of practical lexical coverage. For all of these sets, we remove punctuation- or symbol-only items, digit-containing items, foreign-language or web markers, and Arabic strings with attached symbols.

| Dataset | Tokens | Types |
| --- | ---: | ---: |
| GumarAE | 9,052,103 | 442,230 |
| Mixat | 91,865 | 15,636 |
| Ramsa Corpus | 26,489 | 7,873 |

### Coverage — v1.1

GLF uses `calima-glf-01`, EGY uses `calima-egy-r13` (only analyses marked `[CALIMA]`), and MSA uses `camel-morph-msa-v1.2.2`. ANY is their union.

A type is in-vocabulary (INV) if any analyzer returns an analysis, and out-of-vocabulary (OOV) otherwise. Because the CAMeL Tools GLF analyzer does not provide English glosses, we evaluate Gahwa at the Lemma-POS (LP) level. POS matching is largely a case-insensitive identity mapping between the two tagsets.

We use two LP matching conditions: Exact compares lemma strings as given, whereas dediacritized matching (Dediac) strips Arabic diacritics before comparison. LP-match columns use analyzer-covered output as the denominator, while effective coverage uses the full evaluation corpus. All values below are percentages.

#### GumarAE

| Analyzer | INV types | INV tokens | Gahwa types Exact | Gahwa types Dediac | Gahwa tokens Exact | Gahwa tokens Dediac | Effective Exact | Effective Dediac |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| GLF | 37.5 | 83.7 | 91.9 | 98.2 | 97.5 | 99.6 | 81.6 | 83.4 |
| EGY | 35.9 | 79.3 | 54.2 | 75.0 | 73.6 | 86.7 | 58.4 | 68.7 |
| MSA | 52.0 | 87.3 | 36.3 | 63.5 | 50.4 | 83.2 | 44.0 | 72.6 |
| ANY | 63.3 | 93.3 | 70.1 | 79.4 | 92.4 | 94.9 | 86.3 | 88.6 |
| ANY + RepNorm | 84.4 | 97.3 | 71.7 | 80.0 | 91.9 | 94.4 | 89.4 | 91.9 |

#### Mixat

| Analyzer | INV types | INV tokens | Gahwa types Exact | Gahwa types Dediac | Gahwa tokens Exact | Gahwa tokens Dediac | Effective Exact | Effective Dediac |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| GLF | 70.8 | 89.6 | 93.9 | 99.1 | 97.9 | 99.8 | 87.7 | 89.4 |
| EGY | 75.8 | 91.0 | 59.5 | 81.3 | 73.6 | 88.5 | 67.0 | 80.5 |
| MSA | 87.8 | 95.7 | 43.5 | 73.1 | 47.8 | 80.7 | 45.8 | 77.2 |
| ANY | 95.6 | 98.8 | 79.8 | 86.5 | 92.8 | 95.3 | 91.6 | 94.1 |
| ANY + RepNorm | 95.9 | 98.8 | 79.8 | 86.5 | 92.8 | 95.3 | 91.7 | 94.2 |

#### Ramsa Corpus

| Analyzer | INV types | INV tokens | Gahwa types Exact | Gahwa types Dediac | Gahwa tokens Exact | Gahwa tokens Dediac | Effective Exact | Effective Dediac |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| GLF | 66.6 | 81.7 | 95.1 | 99.4 | 97.9 | 99.8 | 80.0 | 81.6 |
| EGY | 76.8 | 88.1 | 56.7 | 76.4 | 70.0 | 84.3 | 61.7 | 74.3 |
| MSA | 90.7 | 96.3 | 40.3 | 70.0 | 44.3 | 79.6 | 42.7 | 76.6 |
| ANY | 95.4 | 98.3 | 75.7 | 82.3 | 85.8 | 90.4 | 84.3 | 88.9 |
| ANY + RepNorm | 95.8 | 98.4 | 75.6 | 82.3 | 85.8 | 90.4 | 84.4 | 89.0 |


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

See [LICENSE](LICENSE) for the full license text.

## ArabicNLP 2026 Release

The paper release, [Gahwa v1.0](versions/ArabicNLP2026), remains available with its downloads, statistics, and evaluation results.
## Changes in v1.1

- Preserved diacritization variants from the source records. Consolidation no longer selects a single, more diacritically informative lemma by prioritizing shadda and sukun, allowing greater diacritic variation to remain in v1.1.
- Consolidated entry-level records within each source and across sources. Exact duplicates are removed using Entry + Lemma + POS + CAMeL_POS + normalized English gloss. Overlapping gloss lists are merged only when Entry, Lemma, POS, and CAMeL_POS agree.
- Removed entries identified during inspection as containing obvious annotation errors inherited from the original sources.
- Applied the reviewed CAMeL_POS conversion map to added lemma entries before consolidation.
- Corrected Type labels for lemmas, word forms, and phrases. Standardized gloss separators and replaced spaces in multiword entries and gloss expressions with underscores.
- Updated inventory counts and coverage results using the same evaluation datasets. The paper’s repeated-letter normalization and OOV analyses are unchanged.

## Contact

- Saeed Alzaabi: [saeed.alzaabi@nyu.edu](mailto:saeed.alzaabi@nyu.edu)
- Salam Khalifa: [salam.khalifa@nyu.edu](mailto:salam.khalifa@nyu.edu)
- Mostafa Saeed: [mostafa.saeed@nyu.edu](mailto:mostafa.saeed@nyu.edu)
- Shahd Dibas: [shahd.dibas@mbzuai.ac.ae](mailto:shahd.dibas@mbzuai.ac.ae)
- Hanan Aldarmaki: [hanan.aldarmaki@mbzuai.ac.ae](mailto:hanan.aldarmaki@mbzuai.ac.ae)
- Nizar Habash: [nizar.habash@nyu.edu](mailto:nizar.habash@nyu.edu)
