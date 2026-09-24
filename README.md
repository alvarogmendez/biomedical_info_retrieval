# Biomedical Information Retrieval — generalist vs. domain-specific Transformers

[![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97-Transformers-yellow.svg)](https://huggingface.co/docs/transformers)

A binary text classifier that reads the abstract of a scientific paper and decides whether it is
**relevant to polyphenol research** — the filtering step of an information-retrieval system.
It compares a generalist model with two biomedical ones:

| Model | Type | Checkpoint | Pre-trained on |
|---|---|---|---|
| **BERT** | Generalist | `google-bert/bert-base-cased` | Books and English Wikipedia |
| **BioBERT** | Domain-adapted | `dmis-lab/biobert-v1.1` | BERT further trained on PubMed abstracts and PMC articles |
| **BiomedBERT** | Domain-specific | `microsoft/BiomedNLP-BiomedBERT-base-uncased-abstract-fulltext` | From scratch on PubMed abstracts and PMC full texts |

Course project for *Information Retrieval* (MSc in Artificial Intelligence, Universidad
Politécnica de Madrid, November 2025). Full report: [`report.pdf`](report.pdf) ·
slides: [`slides.pdf`](slides.pdf).

## Results

Test set of 356 abstracts (178 relevant, 178 not relevant), after 10 epochs of fine-tuning:

| Model | Accuracy | F1 (weighted) | ROC-AUC | Average precision | P@10 |
|---|:---:|:---:|:---:|:---:|:---:|
| BERT | 0.975 | 0.975 | 0.9981 | 0.9980 | 1.00 |
| BioBERT | 0.989 | 0.989 | 0.9979 | 0.9976 | 1.00 |
| **BiomedBERT** | **0.989** | **0.989** | **0.9996** | **0.9996** | 1.00 |

All three models exceed 0.97 F1; **BiomedBERT** was selected for its near-perfect ranking
(ROC-AUC 0.9996). The most useful lesson came from comparing with another group: with a slightly
more complete dataset they reached 0.9917 F1 using BioBERT, a theoretically weaker model than
BiomedBERT — **the quality of the dataset mattered more than the choice of model**.

## Dataset

The teaching staff provided metadata for 1,308 polyphenol papers, but not their abstracts:

1. **Relevant abstracts** — retrieved in cascade: PubMed (NCBI E-utilities) returned only 663,
   Europe PMC raised it to 962 and Scopus recovered most of the rest: **1,186** of 1,308.
   Entries that did not correspond to a real article were removed.
2. **Non-relevant abstracts** — **1,185** papers from PubMed with the query `NOT polyphenol`.
3. **Split** — stratified 70 / 15 / 15: 1,659 training, 356 validation and 356 test abstracts,
   balanced in every subset.

## Method

- Fine-tuning with the Hugging Face `Trainer`, identical arguments for the three models:
  10 epochs (2,080 steps); the checkpoint with the best validation accuracy is kept.
- **Set-based metrics:** accuracy, precision, recall, F1 and confusion matrices.
- **Rank-based metrics:** ROC-AUC, P@5, P@10, R-precision, average precision and reciprocal rank.
- Trained on a free Google Colab T4 GPU, about 30 minutes per model.

## Running it

> **Not included in this repository:** neither the fine-tuned models nor the list of papers
> (`publications.xlsx`, provided by the teaching staff) can be published. The notebook contains
> every step needed to rebuild the dataset and fine-tune the three models yourself.

Everything is in the notebook [`bio_IRS_BERT.ipynb`](bio_IRS_BERT.ipynb), designed for Google Colab:

1. Open the notebook in Colab and select a **T4 GPU** (*Runtime → Change runtime type*).
2. Upload your own list of papers in the format of `publications.xlsx` (title, authors, journal,
   publication date…) to the Colab session.
3. Run all cells: they download the abstracts, build `polyphenol_dataset.csv`, split the data,
   fine-tune the three models and produce every table and plot.

To run it locally instead: `pip install -r requirements.txt` (a GPU is strongly recommended).

## Author

Álvaro González Méndez — [alvarogmendez.es](https://alvarogmendez.es) · [LinkedIn](https://www.linkedin.com/in/alvarogmendez/)
