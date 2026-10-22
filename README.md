# Persian News Recommender System

A content-based recommender for Persian-language news. Given an article, it returns the five most similar articles from a corpus of 5,000 news items, using Hazm for Persian text preprocessing, TF-IDF for vectorization and cosine similarity for ranking.

## How it works

```
raw article → normalize → tokenize → lemmatize → remove stop words
            → TF-IDF vector → cosine similarity → top-5 similar articles
```

| Stage | Tooling |
|---|---|
| Persian preprocessing | [Hazm](https://github.com/roshan-research/hazm) |
| Vectorization | scikit-learn `TfidfVectorizer` (5,000 features) |
| Similarity | scikit-learn `cosine_similarity` |
| Ranking | NumPy `argsort` |

Design notes:
- The half-space (ZWNJ) is preserved during cleaning, so words like «می‌روم» are not broken.
- Each article is represented by its category label plus its preprocessed text, which nudges recommendations towards the same section.

## Example

Recommendations for article #46:

| Rank | Category | Article index |
|---|---|---|
| 1 | Science & technology | 1688 |
| 2 | Science & technology | 4844 |
| 3 | Science & technology | 2578 |
| 4 | Science & technology | 2048 |
| 5 | Science & technology | 3142 |

The notebook also prints recommendations with category and cosine similarity score, and discusses where the approach works and where it does not.

## Project structure

```
.
├── notebooks/
│   └── persian_news_recommender.ipynb   # full walkthrough
├── data/                                # put news_data.csv here (not included)
├── requirements.txt
├── LICENSE
└── README.md
```

## Getting started

```bash
git clone https://github.com/<your-username>/persian-news-recommender.git
cd persian-news-recommender

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

jupyter notebook notebooks/persian_news_recommender.ipynb
```

## Dataset

The dataset is **not included** in this repository, because the article texts belong to their publisher. The notebook expects a file `data/news_data.csv` with these columns:

| Column | Description |
|---|---|
| `content` | Full article text (Persian) |
| `label` | Category name (8 categories) |
| `label_id` | Numeric category id |

The data used here is 5,000 news articles from the Young Journalists Club (YJC), Iran (Provided by Quera Recommender Systems Course).

## Limitations and future work

- TF-IDF is lexical: it matches words, not meaning. Next step: compare with Persian or multilingual transformer embeddings.
- Shared agency boilerplate inflates similarity. Corpus-specific stop words or `max_df` would help.
- The category label is part of the representation, so category-level metrics would be optimistic. A text-only baseline would show its real effect.
- No quantitative evaluation yet beyond manual inspection.
- The dense n × n similarity matrix does not scale; use sparse nearest-neighbour search or FAISS for large corpora.

## Tech stack

Python · Hazm · scikit-learn · NumPy · pandas · Jupyter

## Author

**Alireza Bagheri** · [LinkedIn](https://www.linkedin.com/in/alireza-bagheri-a5624123b/) · [GitHub](https://github.com/Aleezzawho)

## License

MIT, see [LICENSE](LICENSE). The license covers the code only, not the dataset.
