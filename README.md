# SEO Content Quality & Duplicate Detector

## Project Overview
This project implements a content analysis system that evaluates the SEO quality of web pages and detects near-duplicate content. It processes pre-scraped HTML, extracts text and NLP features, computes similarity scores, and predicts content quality (Low/Medium/High) using a machine learning model. A real-time analysis function allows live URL assessment.

## Setup Instructions
1. Clone the repository:
```bash
git clone https://github.com/aishwaryashinde26/SEO-Content-Quality-Duplicate-Detector-.git
cd seo-content-detector
``` 

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Open the main notebook:
```bash
seo-content-detector/notebooks/seo_pipeline.ipynb
```

## Quick Start
1. Load the provided dataset: data/data.csv (URLs + HTML content).<br>
2. Run the notebook end-to-end to extract content, compute features, detect duplicates, and train the quality model.<br>
3. Use the analyze_url(url) function in the notebook to test live pages:
```bash
result = analyze_url("https://en.wikipedia.org/wiki/Main_Page")
print(result)
```

## Key Decisions
* **Libraries**: BeautifulSoup for HTML parsing, textstat for readability, scikit-learn for ML and similarity, nltk for sentence tokenization. Chosen for simplicity, speed, and reliability.
* **HTML Parsing**: Extracted <title> and main text, removed scripts/styles, used modular extract_from_html() for reuse.
* **Similarity Threshold**: 0.75 for cosine similarity to flag near-duplicates, balancing precision and recall.
* **Model Selection**: Random Forest trained on word count, sentence count, and readability; chosen for interpretability and robust performance.

## Results Summary

**Model Performance:**

```text
              Precision  Recall  F1-Score  Support
Low Quality     1.00      0.93      0.97        15
Medium Quality  0.78      0.88      0.82         8
High Quality    0.50      0.50      0.50         2

Overall Accuracy: 0.88
F1-Score (weighted): 0.88
```

* **Duplicate Detection**: Identified multiple near-duplicate pages with cosine similarity >= 0.80; thin content pages flagged if word count < 500. Total duplicate pairs found: 3
* **Sample Quality Scores**: Use analyze_url() to check live pages; returns word count, readability, quality label, and similar pages.
# Sample quality score output from analyze_url()
```text
{
  "url": "https://en.wikipedia.org/wiki/Main_Page",
  "word_count": 1971,
  "sentence_count": 53,
  "readability": 35.344451570758395,
  "is_thin": false,
  "quality_label": "Medium Quality",
  "similar_to": []
}
```

## Limitations
* Small dataset (60–70 pages) limits generalization; model may underperform on unseen content types.
* TF-IDF embeddings are sparse; using sentence transformers could improve duplicate detection.
* Readability scores may be negative or low for technical content; model relies on synthetic labeling rules.

