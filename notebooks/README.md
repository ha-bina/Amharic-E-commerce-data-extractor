# Amharic E-commerce Data Extractor

This project is designed to scrape, preprocess, and analyze Amharic-language e-commerce data from Telegram channels. It supports entity extraction (NER), data labeling, and vendor analytics, enabling downstream applications such as credit scoring, market analysis, and NLP research.

---

## Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Setup](#setup)
- [Usage](#usage)
  - [1. Data Collection](#1-data-collection)
  - [2. Data Preprocessing](#2-data-preprocessing)
  - [3. Data Labeling](#3-data-labeling)
  - [4. NER Model Training](#4-ner-model-training)
  - [5. Vendor Analytics](#5-vendor-analytics)
- [Customization](#customization)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Features

- **Telegram Scraping:** Collects messages (including media) from specified Amharic e-commerce Telegram channels.
- **Text Cleaning:** Normalizes and cleans Amharic text, removing noise and standardizing numerals.
- **Entity Extraction:** Extracts prices, products, locations, and contacts from messages.
- **Media Handling:** Downloads and processes images and documents attached to messages.
- **Data Preprocessing:** Aggregates and enriches data for analysis and machine learning.
- **NER Data Labeling:** Supports semi-automatic labeling and exports data in CoNLL format.
- **NER Model Training:** Fine-tunes and evaluates transformer-based NER models (e.g., XLM-RoBERTa, mBERT).
- **Vendor Analytics:** Computes vendor-level metrics (posting frequency, average views, price, etc.) for credit scoring or ranking.

---

## Project Structure

```
.github/
    workflows/
        unittests.yml
.vscode/
    settings.json
notebooks/
    __init__.py
    amharic_ecommerce_scraper.session
    data_ingestion_and_preprocessing.ipynb
    fully_processed_data.csv
    labeled_data.conll
    README.md
    train.txt
    processed_data/
        @ethio_brand_collection_messages.csv
        @Leyueqa@AwasMart_messages.csv
        ...
    raw_data/
        ...
README.md
requirements.txt
scripts/
    __init__.py
    README.md
src/
tests/
    __init__.py
```

---

## Setup

1. **Clone the repository**
    ```sh
    git clone <repo-url>
    cd Amharic-E-commerce-data-extractor
    ```

2. **Install dependencies**
    - Recommended: Use a virtual environment.
    - Install all requirements:
      ```sh
      pip install -r requirements.txt
      ```
    - Or, run the notebook cells to auto-install:
      - `pandas`
      - `telethon`
      - `Pillow`
      - `python-magic-bin`
      - `numpy`
      - `transformers`
      - `datasets`
      - `seqeval`
      - `shap`
      - `lime`

3. **Configure Telegram API**
    - Obtain your `API_ID` and `API_HASH` from [my.telegram.org](https://my.telegram.org).
    - Set your `PHONE_NUMBER` in `notebooks/data_ingestion_and_preprocessing.ipynb`.

---

## Usage

### 1. Data Collection

- Open and run `notebooks/data_ingestion_and_preprocessing.ipynb`.
- The `TelegramScraper` class will:
  - Connect to Telegram.
  - Scrape messages from channels listed in the `CHANNELS` variable.
  - Save raw and processed CSVs to `raw_data/` and `processed_data/`.

### 2. Data Preprocessing

- The `DataPreprocessor` class:
  - Loads all CSVs from `raw_data/`.
  - Cleans and normalizes text.
  - Extracts features (text length, word count, etc.).
  - Handles missing values.
  - Saves the processed data to `fully_processed_data.csv`.

### 3. Data Labeling

- 50 random messages are selected for labeling.
- The `label_message` function tokenizes and labels entities (prices, locations, products) using regex and keyword heuristics.
- Labeled data is saved in CoNLL format as `labeled_data.conll`.

### 4. NER Model Training

- The notebook supports fine-tuning transformer models (e.g., XLM-RoBERTa) for NER using Hugging Face Transformers and Datasets.
- Steps include:
  - Parsing CoNLL data.
  - Tokenization and label alignment.
  - Training and evaluation with `Trainer`.
  - Saving the best model to disk.
- Model explainability is supported via SHAP and LIME.

### 5. Vendor Analytics

- The `analyze_vendor` function computes:
  - Posting frequency (weekly average).
  - Average views per vendor.
  - Top product per vendor (via NER).
  - Average product price.
  - A simple lending score combining views and posting frequency.
- Example usage:
    ```python
    vendor_df = pd.read_csv("telegram_posts.csv")
    score_df = analyze_vendor(vendor_df, ner_pipeline)
    print(score_df.head())
    ```

---

## Customization

- **Channels:** Edit the `CHANNELS` list in the notebook to add/remove Telegram channels.
- **Entity Extraction:** Modify the `extract_entities` method in `TelegramScraper` for custom rules.
- **NER Labeling:** Adjust `label_message` and `product_keywords` for more/less aggressive product labeling.
- **Model Selection:** Change the `model_checkpoint` variable to try different transformer models.

---

## Troubleshooting

- **Empty Data Files:**  
  - Ensure your Telegram API credentials are correct.
  - Check that the channels in `CHANNELS` exist and are accessible.
  - If you see errors like "You must use 'async for'...", run the notebook as a script or adapt for async execution.
  - Make sure the correct data directory is used in `DataPreprocessor`.

- **No Columns to Parse from File:**  
  - This means the CSV is empty. Check the scraping step for errors.

- **Media Download Issues:**  
  - Ensure `python-magic-bin` and `Pillow` are installed.
  - Check file permissions for the `raw_data/images/` directory.

