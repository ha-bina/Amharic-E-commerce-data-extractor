# Amharic E-commerce Data Extractor

This project scrapes Amharic e-commerce Telegram channels, preprocesses the collected data, and prepares it for downstream NLP tasks such as Named Entity Recognition (NER).

## Features

- Scrapes messages (including media) from specified Telegram channels
- Cleans and normalizes Amharic text
- Extracts entities: prices, products, locations, contacts
- Handles media downloads (images, documents)
- Preprocesses and aggregates data for analysis or ML tasks
- Exports labeled data in CoNLL format for NER

## Project Structure

```
.github/
  workflows/
    unittests.yml
.vscode/
  settings.json
notebooks/
  data_ingestion_and_preprocessing.ipynb
  fully_processed_data.csv
  processed_data/
  raw_data/
  README.md
  train.txt
  amharic_ecommerce_scraper.session
README.md
requirements.txt
scripts/
src/
tests/
```

## Setup

1. **Clone the repository**  
   ```sh
   git clone <repo-url>
   cd Amharic-E-commerce-data-extractor
   ```

2. **Install dependencies**  
   It is recommended to use a virtual environment.
   ```sh
   pip install -r requirements.txt
   ```

   Or, if running in Jupyter/VSCode, the notebook will install:
   - pandas
   - telethon
   - Pillow
   - python-magic-bin
   - numpy

3. **Configure Telegram API**  
   - Set your `API_ID`, `API_HASH`, and `PHONE_NUMBER` in `data_ingestion_and_preprocessing.ipynb`.

## Usage

### 1. Data Collection

Run the notebook `notebooks/data_ingestion_and_preprocessing.ipynb`:

- Scrapes messages from the channels listed in the `CHANNELS` variable.
- Saves raw and processed data to `raw_data/` and `processed_data/`.

### 2. Data Preprocessing

- Loads all CSVs from `raw_data/`
- Cleans and normalizes text
- Extracts features and entities
- Saves the fully processed data to `fully_processed_data.csv`

### 3. Data Labeling

- Selects 50 random messages for manual or automatic labeling
- Tokenizes and labels entities in CoNLL format
- Saves labeled data to `labeled_data.conll`

## Customization

- **Channels:** Edit the `CHANNELS` list in the notebook to add/remove Telegram channels.
- **Entity Extraction:** Modify the `extract_entities` method in the `TelegramScraper` class for custom entity rules.

