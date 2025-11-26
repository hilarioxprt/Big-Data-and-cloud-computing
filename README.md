# my_data_analysis — Spark-based Big Data ML & Analysis work

This repository contains concise, runnable Apache Spark examples (PySpark) and Jupyter notebooks that demonstrate practical big-data analysis, production-minded preprocessing, and end-to-end model workflows.

Highlights
- Worked PySpark notebook that ingests a CSV (DigitalBreathTestData2013.csv), demonstrates RDD → DataFrame conversion, explicit schema enforcement, and end-to-end analytics.
- Typical Spark ML preprocessing patterns (StringIndexer, VectorAssembler, OneHotEncoder, scaling) and pipeline-ready data shaping.
- Examples show grouping, filtering, SQL queries, aggregation, correlation analysis and exporting small results to Pandas for reporting.

Requirements
- Java 8 or 11
- Apache Spark 3.x
- Python 3.8+ and pyspark
- Optional: virtualenv or conda for dependency isolation

Quick setup
```bash
python -m venv venv
source venv/bin/activate
pip install pyspark pandas matplotlib seaborn
```

Repository layout (typical)
- README.md — this file
- examples/ — PySpark example scripts (train, preprocess, evaluate)
- notebooks/ — Jupyter notebooks (interactive walkthroughs)
- data/ — small sample datasets (e.g., DigitalBreathTestData2013.csv)
- models/ — serialized models / outputs (not committed)
- tests/ — unit / integration tests (if present)

Quickstart — run locally
1. Start a SparkSession inside a script or notebook:
```python
from pyspark.sql import SparkSession
spark = SparkSession.builder \
    .appName("my_data_analysis") \
    .master("local[*]") \
    .getOrCreate()
```
2. Run an example script (adjust paths and args):
```bash
python examples/train_model.py --input data/DigitalBreathTestData2013.csv --output models/
```
3. Use spark-submit to mimic cluster behavior:
```bash
$SPARK_HOME/bin/spark-submit \
  --master local[*] \
  examples/train_model.py \
  --input data/DigitalBreathTestData2013.csv \
  --output models/
```

Notebook: combined self and 4a.ipynb — key points
- Ingest: CSV via RDD (sc.textFile) and DataFrame (spark.read.csv) with explicit schema; stable column renaming for awkward headers.
- Parsing & typing: robust RDD → tuple parsing, StructType schemas for typed columns (IntegerType/DoubleType).
- Cleaning: categorical normalization, null handling, type casting, and focused filters (e.g., AlcoholLevel > 35).
- Analysis: groupBy/SQL aggregations, fail-rate calculations, top-k queries, exporting summarized results to Pandas for reporting.
- ML workflow: feature engineering (indexing, one-hot, assembler), pipeline composition, model training, evaluation and persistence.
- Performance: use of .cache(), partition and memory tuning, and packaging dependencies for cluster runs.

## Skills demonstrated (Jupyter notebooks)

- Core big-data engineering (PySpark)
  - RDD and DataFrame APIs, SparkSession usage, explicit schema design and enforcement.
  - Scalable ingestion patterns and safe parsing for messy CSV inputs.

- Data wrangling & production-ready cleaning
  - Robust parsing, type casting, header normalization, null handling, and categorical mapping suitable for pipelines.

- Feature engineering & ML pipelines
  - StringIndexer, OneHotEncoder, VectorAssembler, scaling and composing Spark ML pipelines ready for productionizing.

- Model training & evaluation
  - Train/test splits, model fitting (example: Naive Bayes), MulticlassClassificationEvaluator, confusion-matrix calculations and standard metrics reporting.

- Exploratory data analysis & visualization
  - Aggregations, correlation matrices (Pearson & Spearman), scatter/heatmap visuals, and export-to-Pandas for concise stakeholder summaries.

- Performance & deployment considerations
  - Caching, partition awareness, spark-submit packaging (--py-files / wheel), and driver/executor memory tuning documented and demonstrated.

- Reproducibility, tooling & communication
  - Environment isolation (venv/conda), clear quickstart, notebook narratives, and concise Pandas exports that make analyses reviewable and audit-friendly.

Why this matters to a hiring reviewer
- End-to-end evidence: the notebooks show the full pipeline from messy data ingestion to production-minded feature pipelines, model training, evaluation, and shareable reporting.
- Practical judgement: demonstrates when to use Spark vs. Pandas, how to tune performance for scale, and how to prepare work for cluster execution and hand-off.
- Readiness for production: code and notebook patterns focus on schema stability, reproducibility, and packaging—exact qualities valuable for data engineering or ML infrastructure roles.

Where to look
- Open /notebooks to inspect the concrete code cells and outputs. Each notebook is intentionally concise, documented, and focused on demonstrable outcomes you can validate quickly.

Contributing
- Fork, create a branch, add tests, update README for any changes, and open a pull request describing your change.

Author / Contact
- hilarioxprt — repository owner

License & Data
- Data in the notebook is Crown Copyright (Open Government Licence v3.0).
- Do not commit large datasets to this repository; use external storage for large files.