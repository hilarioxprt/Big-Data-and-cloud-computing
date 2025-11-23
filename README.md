# my_data_analysis — Spark-based Big Data ML & Analysis work

This repository contains concise, runnable Apache Spark examples (PySpark) and a focused notebook demonstrating practical big-data analysis and ML-ready preprocessing. The examples are implementation-agnostic so you can run them locally, on a cluster, or in cloud notebooks (Databricks, EMR, etc.). Update paths and commands to match your environment.

Highlights
- Worked PySpark notebook that ingests a CSV (DigitalBreathTestData2013.csv), demonstrates RDD → DataFrame conversion, schema enforcement, and end-to-end analytics.
- Typical Spark ML preprocessing patterns (StringIndexer, VectorAssembler, OneHotEncoder, scaling) and pipeline-ready data shaping.
- Examples show grouping, filtering, SQL queries, aggregation, and exporting small results to Pandas for reporting.

Requirements
- Java 8 or 11
- Apache Spark 3.x
- Python 3.8+ and pyspark
- Optional: virtualenv or conda for dependency isolation

Quick setup
```bash
python -m venv venv
source venv/bin/activate
pip install pyspark pandas
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

Notebook: "comnined self and 4a.ipynb" — key points
- Data ingestion:
  - Loaded CSV both via RDD (sc.textFile) and DataFrame (spark.read.csv) with an explicit schema.
  - Handled header collisions by renaming the column BreathAlcoholLevel(microg 100ml) → AlcoholLevel.
- Parsing and schema:
  - Demonstrates a robust parsing function for RDD → tuples and creation of a DataFrame with named fields (Reason, Month, Year, WeekType, TimeBand, AlcoholLevel, AgeBand, Gender).
  - Shows defining a StructType schema when reading CSV to get typed columns (IntegerType for Year, AlcoholLevel).
- Data cleaning / projection:
  - Converted Gender values between string ("Male"/"Female") and numeric representations used in code.
  - Filtered to create subsets (e.g., failed tests with AlcoholLevel > 35).
- Analysis examples:
  - Counted total records and per-key counts (countByKey / groupBy).
  - Computed per-month fail proportions (positive tests / total tests), ordered by fail rate and exported to Pandas for summary/statistics.
  - Identified top Reasons, most common TimeBand and AgeBand for failed tests.
  - Showed sample of highest AlcoholLevel records (orderBy desc).
- Performance and UX notes:
  - Use .cache() on intermediate DataFrames/RDDs used repeatedly.
  - Converting to Pandas is appropriate for small aggregations; avoid for full datasets.
  - When running on clusters, ensure dependencies are available to executors (--py-files or a packaged wheel).

Typical ML workflow illustrated
1. Ingest raw data (CSV, parquet).
2. Cleaning and casting types (schema enforcement).
3. Feature engineering (categorical encoding, VectorAssembler).
4. Build Spark ML Pipelines.
5. Train, evaluate, and persist models.
6. Hyperparameter tuning with CrossValidator / TrainValidationSplit.

Best practices
- Define and reuse an explicit schema for CSV reads to avoid header / auto-inference surprises.
- Rename awkward column names early to stable, code-friendly identifiers.
- Use Spark SQL or DataFrame API consistently; register temp views for repeatable analyses.
- Keep heavy exports to Pandas limited to aggregated or sampled results.
- For production / cluster runs, package dependent modules and control driver/executor memory via spark-submit flags.

Troubleshooting
- CSVHeaderChecker warnings: ensure schema matches header names or rename after read.
- ClassNotFound / dependency issues on cluster: package Python deps (--py-files) or use a shared environment.
- Memory issues: set --driver-memory and --executor-memory appropriately; tune partitions.

Contributing
- Fork, create a branch, add tests, update README for any changes, and open a pull request describing your change.

Author / Contact
- hilarioxprt — repository owner

License & Data
- Data in the notebook is noted as Crown Copyright (Open Government Licence v3.0).
- Do not commit large datasets to this repository; use external storage for large files.
