# masterthesis-eeg

Code and context sentences for my master's thesis:

**"What drives neural alignment with word embeddings? A residual analysis of EEG-based semantic decoding"**

Valeria Bolgert, Goethe University Frankfurt

## Contents

| File | Description |
|---|---|
| `gpt_vector_generation.ipynb` | Collects the context sentences and extracts GPT-2 embeddings for all 13 layers. |
| `analysis_script.ipynb` | Feature decoding, embedding decoding, ablation analysis, statistics and figures. |
| `sentence_table_fixed.xlsx` | The 9,600 context sentences used for the GPT-2 embeddings. |
| `scores/` | Saved decoding scores (`.npy`) from which all reported results and figures can be reproduced. |

## Context sentences

`sentence_table_fixed.xlsx` contains 10 context sentences for each of the 960 Kiloword nouns, with the columns `word`, `sentence` and `source`.

Sentences were collected with a four-stage pipeline:

| Stage | Source | Label in `source` | Share |
|---|---|---|---|
| 1 | WikiText-2 | `wikitext` | 79% |
| 2 | Wikipedia API | `wikipedia` | 12% |
| 3 | Gemini 2.5 Flash | `gemini` | 1% |
| 4 | Llama-3.3-70B via the Groq API | `groq` | 8% |

Only sentences with at least four words of left context before the target word were retained. All sentences were checked manually.

## External data and models

These resources are not included in this repository and must be obtained from the original sources.

- **EEG data:** Kiloword dataset (Dufau et al., 2015), grand-averaged ERPs for 960 English nouns. Downloaded automatically through MNE-Python (`mne.datasets.kiloword`). https://mne.tools/stable/generated/mne.datasets.kiloword.data_path.html
- **FastText vectors:** pretrained English vectors `wiki-news-300d-1M.vec` (Bojanowski et al., 2017; Mikolov et al., 2018). https://fasttext.cc/docs/en/english-vectors.html
- **GPT-2 base** (Radford et al., 2019), accessed through the Hugging Face `transformers` library. https://huggingface.co/openai-community/gpt2
- **NRC Valence, Arousal, and Dominance Lexicon v2.1** (Mohammad, 2025), file `unigrams-NRC-VAD-Lexicon-v2.1.txt`. https://saifmohammad.com/WebPages/nrc-vad.html

Of the 960 Kiloword nouns, 944 are covered by the NRC-VAD Lexicon. All analyses use these 944 words.

## How to reproduce

1. Download `wiki-news-300d-1M.vec` and `unigrams-NRC-VAD-Lexicon-v2.1.txt` and place them in the same folder as the notebooks.
2. Run `gpt_vector_generation.ipynb`. It reads `sentence_table_fixed.xlsx` and writes `gpt2_embeddings.pkl`. Collecting new sentences requires your own Gemini and Groq API keys. This step can be skipped, because the finished sentence table is provided.
3. Run `analysis_script.ipynb`. It reads the embeddings, runs all decoding and ablation analyses, and writes the scores, the test statistics (`wilcoxon_fdr.xlsx`) and the figures.

To recreate the figures and statistics without rerunning the decoding, copy the files from `scores/` into the notebook folder.

## Analysis settings

- Ridge regression (`RidgeCV`) with alpha selected from {0.1, 1, 10, 100, 1000}
- 10-fold cross-validation, `random_state=0`
- Time windows: 0–300 ms, 300–500 ms, 500–920 ms
- One-sided Wilcoxon signed-rank tests on fold-level scores, Benjamini-Hochberg FDR correction

## Requirements

Python 3 with `numpy`, `pandas`, `scipy`, `scikit-learn`, `mne`, `matplotlib`, `openpyxl`.
Additionally for `gpt_vector_generation.ipynb`: `torch`, `transformers`, `datasets`, `wikipedia`, `google-genai`, `groq`.

## References

Full references are listed in the thesis.
