#  AI-Augmented E-Commerce Insights Dashboard

An AI-powered business intelligence dashboard that transforms raw e-commerce transaction data into automated KPIs, statistical anomaly detection, and LLM-generated executive summaries — built to mirror real analytics workflows at data-driven e-commerce companies.

This project demonstrates how AI can **support** analytics rather than replace it: all numbers are computed deterministically with Pandas, and the AI layer (Gemini) is strictly constrained to narrate only the pre-computed metrics — never to invent or estimate figures.

---

##  What It Does

- **Automated Data Cleaning** — merges and cleans a multi-table relational e-commerce dataset (~110K order line items), with explicit, defensible business-logic decisions (e.g., filtering to delivered orders only)
- **KPI Engine** — computes revenue, freight economics, delivery performance, and category/state breakdowns directly from the data
- **Interactive Visualizations** — Plotly charts for monthly trends, category/state breakdowns, and freight cost burden analysis
- **Statistical Anomaly Detection** — z-score based detection of genuinely unusual months, with iterative refinement to exclude early-stage/incomplete data periods
- **AI Executive Summary** — Gemini generates a narrative business summary, but is only ever given the *computed KPI values* (never raw data), preventing hallucinated figures
- **PDF Report Generation** — one-click export of a polished, shareable business report
- **Interactive Dashboard** — built with Gradio, upload a CSV and get the full pipeline output in one click

---

## 🏗️ Architecture
Raw CSVs (Orders, Items, Products, Customers)
↓
Merge + Clean (Pandas)
↓
KPI Engine (Pandas/NumPy)
↓
┌─────────────┬──────────────────┬─────────────────┐
↓ ↓ ↓
Plotly Charts Anomaly Detection AI Summary (Gemini)
↓ ↓ ↓
└─────────────┴──────────────────┴─────────────────┘
↓
PDF Report + Gradio Dashboard

**Key design decision:** The AI (Gemini) is only ever given a curated JSON payload of pre-computed KPI values — never the raw dataframe. This guardrail ensures the AI-generated narrative is always grounded in verified numbers rather than free-form data interpretation, which is a critical distinction for using AI safely in business reporting.

---

##  Dataset

This project uses the **[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)** — a real, anonymized dataset of ~100,000 orders from a Brazilian e-commerce marketplace (2016–2018).

### To run this project yourself:

1. Download the dataset from the Kaggle link above
2. You'll need these specific files:
   - `olist_orders_dataset.csv`
   - `olist_order_items_dataset.csv`
   - `olist_products_dataset.csv`
   - `olist_customers_dataset.csv`
   - `product_category_name_translation.csv`
3. Upload them to your Colab environment (or local `/data` folder)
4. Run the notebook cells in order — the notebook handles merging, cleaning, and analysis end-to-end

*(Raw dataset files are not included in this repo due to size constraints — see Kaggle link above.)*

---

##  Key Findings (Example Run)

- **$13.2M+ total sales** across ~96,500 delivered orders
- **Freight costs average 32% of item price** — for the worst-affected category, freight exceeded 93% of the item's price, revealing a significant logistics/margin problem
- **Deliveries consistently beat estimated delivery dates by ~12 days on average** — while positive on the surface, this points to overly conservative delivery estimates that may be under-communicating speed to customers
- **Only 6.6% of orders were delivered late** — strong overall logistics reliability

---

##  Tech Stack

| Category | Tools |
|---|---|
| Data Processing | Python, Pandas, NumPy |
| Visualization | Plotly |
| AI / LLM | Google Gemini API |
| Dashboard UI | Gradio |
| PDF Generation | ReportLab |
| Environment | Google Colab |

---

##  Sample Output

A sample generated report is included in this repo: [`business_report_2026-07-23.pdf`](./business_report_2026-07-23.pdf)

---

##  Design Decisions Worth Noting

- **Delivered-only filtering**: Orders with cancelled/processing status were excluded from KPI calculations, since they don't represent completed, realized revenue — a deliberate business judgment, not just data cleaning.
- **Freight-to-price ratio as the core margin risk metric**: Rather than a generic "discount" field, this project uses freight cost as a percentage of item price — a more realistic and actionable metric for e-commerce/logistics-driven businesses.
- **Z-score anomaly detection with iterative threshold refinement**: An initial anomaly detection pass revealed that Olist's early operating months (2016) had incomplete data, which was distorting the analysis. The detection window was refined to focus on the platform's stable operating period, and this refinement process is documented directly in the notebook.
- **AI guardrails**: The Gemini prompt explicitly instructs the model to use *only* the provided computed metrics — a deliberate architectural choice to prevent hallucinated or invented figures in AI-generated business content.

---

##  Running the Dashboard

The notebook includes a Gradio-based interactive dashboard. After running all cells, a public shareable link is generated (valid for up to 1 week) where you can upload a cleaned CSV and generate the full report in one click.

---

##  Future Improvements

- Deploy permanently via Hugging Face Spaces instead of temporary Gradio links
- Add natural-language-to-SQL query support for ad-hoc business questions
- Extend anomaly detection to account for seasonality more precisely
- Support live multi-file upload/merge directly in the Gradio UI
