# Information Retrieval System: Domain-Specific Transformer Models for Binary Classification
### Author: Alvaro González Méndez

This project is a binary text classifier, developed for an Information Retrieval course assignment. The system is designed to read the abstract of a scientific paper and classify whether it is relevant to the topic of **polyphenols**.

The project implements and evaluates three different Transformer models:
1.  **BERT** (Generalist Model)
2.  **BioBERT** (Domain-Adapted Model)
3.  **BiomedBERT** (Domain-Specific Model)

## Features

This repository contains the complete code for:
* **Data Retrieval:** Scripts to fetch scientific abstracts from PubMed, Europe PMC, and Scopus.
* **Model Fine-Tuning:** A complete pipeline to fine-tune all three Transformer models for sequence classification.
* **Evaluation:** Generation of a comprehensive metrics report, including:
    * **Set-Based:** Accuracy, Precision, Recall, F1-Score, and Confusion Matrices.
    * **Rank-Based:** AUC-ROC, P@5, P@10, Average Precision, and Reciprocal Rank.
* **Analysis:** A comparative analysis to select the best model and a discussion comparing results with a peer group's project.

## How to Run

This project was developed entirely in **Google Colab** using a free T4 GPU environment.

### 1. Dependencies

All required Python libraries are listed in the `requirements.txt` file. You can install them using:
```bash
pip install -r requirements.txt
```

### 2. Execution

The main code is contained in the Jupyter Notebook: `bio_IRS_BERT.ipynb`.

It is highly recommended to run this notebook in Google Colab:

1.    Upload the notebook to your Google Drive and open it with Google Colab.

2.    Ensure the hardware accelerator is set to GPU (Runtime > Change runtime type > T4 GPU).

3.    Upload the publications.xlsx file provided for the assignment to your Colab session.

4.    Run the cells in order. The notebook will:

      *  Perform data retrieval and create the polyphenol_dataset.csv.

      *  Split the data into training, validation, and test sets.

      *  Fine-tune all three models (approx. 30 minutes each).

      *  Generate all tables and plots for the final evaluation.

## Results

All three models performed exceptionally well (>97% F1-Score). The fine-tuned BiomedBERT was selected as the best-performing model, achieving a near-perfect 0.9996 AUC.

A key finding of the project was that a peer group with a slightly more complete dataset achieved marginally better results (0.9917 F1-Score) with a theoretically inferior model (BioBERT). This highlights that the dataset is the most critical component of the system.
