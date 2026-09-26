# masterthesis-eeg
All data and code used for my master's thesis:
"What drives neural alignment with word embeddings? A residual analysis of EEG-based semantic decoding"

# Data and models used

EEG data - Kiloword dataset (Dufau et al., 2015): grand-averaged ERPs for 960 English nouns, available through MNE-Python via mne.datasets.kiloword. 
https://mne.tools/stable/generated/mne.datasets.kiloword.data_path.html

FastText word vectors (Bojanowski et al., 2017; Mikolov et al., 2018): pretrained English vectors wiki-news-300d-1M.vec. https://fasttext.cc/docs/en/english-vectors.html

GPT-2 base (Radford et al., 2019): accessed via the Hugging Face transformers library.
https://huggingface.co/openai-community/gpt2

NRC Valence, Arousal, and Dominance Lexicon v2.1 (Mohammad, 2025): valence, arousal, and dominance ratings. https://saifmohammad.com/WebPages/nrc-vad.html



# Context sentences
Context sentences for the GPT-2 embeddings were collected with a four-stage pipeline: (1) WikiText-2, (2) the Wikipedia API, (3) Gemini 2.5 Flash, and (4) Llama-3.3-70B via the Groq API. Only sentences containing at least four words of left context before the target word were retained.


# Author
Valeria Bolgert, Goethe University Frankfurt


