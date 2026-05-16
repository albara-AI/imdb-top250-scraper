# 🎬 IMDb Top 250 — Web Scraping & Data Pipeline

[![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)](https://python.org)
[![Selenium](https://img.shields.io/badge/Selenium-4.x-green?logo=selenium)](https://selenium.dev)
[![Pandas](https://img.shields.io/badge/Pandas-2.x-purple?logo=pandas)](https://pandas.pydata.org)
[![SQLite](https://img.shields.io/badge/SQLite-Database-blue?logo=sqlite)](https://sqlite.org)
[![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup4-HTML%20Parser-orange)]()

> **End-to-end data pipeline: scrape IMDb Top 250 → clean & transform →
> store in SQLite → query with SQL.**
> Academic Assignment — Data Science & AI, The Hashemite University, 2025.
>
> **Team:** Omar  · Albara · Mo'men 

---

## 📌 Project Overview

A complete 3-stage data engineering pipeline built entirely in Python:

| Stage | Description | Output |
|-------|-------------|--------|
| **Q1 — Scraping** | Selenium + BeautifulSoup scrape movie details & cast from IMDb | `movies1.csv`, `full_cast_and_crew.csv` |
| **Q2 — Preprocessing** | Clean votes, budget, revenue, genres; merge datasets | `cleaned_movies.csv`, `merged_movies_cast.csv` |
| **Q3 — SQL Queries** | Load into SQLite via SQLAlchemy; run analytical queries | `movies_cast.db` |

---

## 🔍 Data Collected

### Movies (`movies1.csv`)
| Field | Example |
|-------|---------|
| Title | The Shawshank Redemption |
| Release Year | 1994 |
| Rating | 9.3 |
| Votes | 2,900,000 |
| Budget | $25,000,000 |
| Gross Worldwide Revenue | $16,000,000 |
| Director(s) | Frank Darabont |
| Genre(s) | Drama |
| Languages | English |
| Country of Origin | United States |
| Production Companies | Castle Rock Entertainment |
| Filming Locations | Mansfield, Ohio, USA |

### Cast & Crew (`full_cast_and_crew.csv`)
| Field | Example |
|-------|---------|
| Movie_ID | 1 |
| Name | Tim Robbins |
| Role_Type | Actor |
| Character_Position | Andy Dufresne |

---

## ⚙️ Pipeline Details

### Stage 1 — Web Scraping
- **Selenium** (headless Chrome) navigates dynamic JS-rendered IMDb pages
- **BeautifulSoup** parses HTML and extracts structured data
- Polite scraping: `sleep(1–7s)` between requests to avoid rate limiting
- Automatic fallback selectors when primary CSS selectors fail
- Scrapes each movie's detail page individually for full metadata

### Stage 2 — Data Cleaning
- **Votes**: `"3M"` → `3,000,000` · `"919K"` → `919,000`
- **Budget**: removes `$`, commas, `(estimated)` · converts ¥ → USD (÷125) · R$ → USD (÷3.3)
- **Gross Revenue**: removes `$` and commas → integer
- **Genres / Languages**: split strings into lists · strip `"Back to top"` artifact
- **Cast names**: strip whitespace · remove `(as ...)` credit notes
- Deduplication applied to both DataFrames

### Stage 3 — SQL Analytics
Three SQL queries on the merged SQLite database:
1. **Top-rated movies** — `ORDER BY Rating DESC`
2. **Movies per release year** — `GROUP BY Release Year`
3. **Most-appearing cast members** — `GROUP BY Name ORDER BY movies_count DESC`

---

## 🚀 Quick Start

### 1. Install dependencies
```bash
pip install -r requirements.txt
```

### 2. Make sure ChromeDriver is available
```bash
# webdriver_manager handles this automatically — no manual install needed
```

### 3. Run the notebook
```bash
jupyter notebook last_version.ipynb
```

> ⚠️ **Note**: IMDb may block automated requests. If scraping fails,
> see the [Known Limitations](#-known-limitations) section below.

---

## 📁 Project Structure

```
imdb-scraper-pipeline/
├── last_version.ipynb          ← Full pipeline notebook (Q1 + Q2 + Q3)
├── requirements.txt            ← Python dependencies
└── README.md
```

> **Output files** (`movies1.csv`, `full_cast_and_crew.csv`,
> `cleaned_movies.csv`, `merged_movies_cast.csv`, `movies_cast.db`)
> are excluded from version control — run the notebook to regenerate them.

---

## ⚠️ Known Limitations

### 1. IMDb Anti-Scraping
IMDb actively detects bots and may return CAPTCHA or block requests.
**Workaround**: increase `sleep()` delays or use a proxy/VPN rotation.

### 2. CSS Selector Fragility
IMDb changes its HTML structure frequently. If selectors like
`data-testid="title-cast"` break, they need to be updated manually.
**Workaround**: inspect the live page and update the selector constants.

### 3. Hardcoded Local Paths
```python
# Original — machine-specific path
data_path = r'C:\Users\DELL\Desktop\movies.csv'
```
**Fix applied in notebook**: use `os.path` for portable paths.

### 4. `LIMIT` Not Working in SQLite Query
One query comment notes `# LIMIT 10 not working`.
**Root cause**: `LIMIT` requires placement *after* `ORDER BY` — confirmed working
with correct syntax.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Selenium 4 | Dynamic page rendering / JS execution |
| BeautifulSoup 4 | HTML parsing |
| Pandas | Data cleaning & transformation |
| SQLAlchemy + SQLite | Relational storage & SQL queries |
| webdriver-manager | Automatic ChromeDriver management |

---

## 📬 Team

| Name | ID |
|------|----|
| Omar | 2330200 |
| **Albaraa Aljaber** | **2239515** |
| Mo'men | 2130462 |

**Albaraa Aljaber** — Data Science & AI, The Hashemite University
📧 [albaraaljaberwork@gmail.com]
💼 [https://www.linkedin.com/in/albara-aljaber/]
