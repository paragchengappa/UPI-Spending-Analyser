# UPI Spending Analyser

An end-to-end Python data pipeline and analysis tool that ingests unstructured transaction exports from Google Pay PDF statements, parses multi-line text into structured DataFrames, normalizes vendor aliases using regular expressions, and provides key time-of-day and categorical financial analytics.

---

## Tech Stack & Key Libraries
* **Language:** Python 3.10+
* **Data Processing & Manipulation:** Pandas, NumPy
* **PDF Ingestion & Text Extraction:** `pdfplumber`
* **Pattern Matching & Parsing:** Regular Expressions (`re`)
* **Visualization:** Matplotlib, Seaborn

---

## Pipeline Architecture & Methodology

1. **PDF Ingestion & Multi-line Parsing:**
   - Extracts unstructured layout text across multi-page PDF transaction statements using `pdfplumber`.
   - Uses dynamic regex loop execution (`re.finditer`) to capture multi-line transaction records resilient to line-wrapping and split UPI handles.

2. **Data Hygiene & Structural Normalization:**
   - Cleans currency strings, strips non-numeric characters, and casts monetary values to floating-point representation.
   - Combines raw date and time strings into unified `pd.Timestamp` objects, enabling temporal indexing and time-of-day segmentation (Morning, Afternoon, Evening, Night).

3. **Merchant Categorization & Mapping Engine:**
   - Implements a priority-ordered categorization pipeline using regex patterns to map variant merchant handles (e.g., `BLINKIT`, `SWIGGY*INSTAMART`, `BMTC`) into canonical spend categories.
   - Categorizes spending into **Quick Commerce**, **Food & Dining**, **Transit**, **Personal Transfers**, **Utilities**, and **Online Shopping**.

---

## Key Findings & Financial Insights

* **Micro-Commerce Dominance:** High-frequency, low-friction quick commerce platforms (Blinkit, Zepto, Swiggy Instamart) represent a primary driver of non-essential debit velocity.
* **Transit Baseline:** Commute-related transactions across public transport networks (BMTC bus routes, BMRCL metro top-ups) establish a highly predictable recurring baseline expense.
* **Temporal Patterns:** Peak spending velocity concentrates heavily in late-afternoon and evening hours, corresponding with food ordering and evening quick-commerce routines.

---

## Limitations & Future Scope

* **Rule-Based Categorization:** Vendor classification relies on pre-defined regex lists. Unrecognized local vendors default to an `Other` category until rules are expanded.
* **PDF Format Sensitivity:** Parsing logic is tailored to Google Pay PDF statement structures; bank-native or alternate UPI application PDFs require custom extraction adapters.
* **Future Improvements:**
  - Introduce zero-shot LLM / NLP classification for dynamic merchant labeling.
  - Build an interactive web dashboard using Streamlit for dynamic budgeting visualizations.

---

## Getting Started

### Prerequisites
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt