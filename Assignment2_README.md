# STQD6324 Data Management — Assignment 2

**MovieLens 100k — a PySpark + Cassandra data pipeline with HDFS (and a MongoDB extension)**

Author: Suraya Balqis binti Ab. Ghafar (P166248)

Course: STQD6324 Data Management, MSc Data Science & Analytics, Universiti Kebangsaan Malaysia (UKM)

Notebook: [`Assignment2_STQD6324_P166248.ipynb`](Assignment2_STQD6324_P166248.ipynb)

---

## Project Details

This project builds an end-to-end big-data pipeline over the **MovieLens 100k** dataset
(100,000 ratings from 943 users on 1,682 movies) using **Apache Spark**, with results
persisted to **Apache Cassandra** and read back for validation. The raw files are loaded
into a **HDFS** instance before processing, and an optional extension reproduces the
analysis on **MongoDB**.

The notebook answers five analytical questions:

| # | Task |
|---|------|
| i | Average rating for each movie |
| ii | Top ten movies by average rating (with a ≥100-ratings threshold so the ranking is reliable) |
| iii | Users who rated ≥50 movies, and each user's most-rated ("favourite") genre |
| iv | Users younger than 20 |
| v | Users whose occupation is "scientist" and whose age is between 30 and 40 |

It also adds a native **MongoDB aggregation pipeline** that reproduces the top-ten ranking on a second
NoSQL store for an optional extension.

## Pipeline

```
ml-100k files ──► HDFS ──► Spark RDDs ──► Spark DataFrames (cleaned)
                                              │
                                     Spark SQL analysis (Tasks i–v)
                                              │
                         ┌────────────────────┴───────────────────┐
                         ▼                                         ▼
              write to Cassandra  ──► read back (validate)   visualise + interpret
                         │
                  (optional) MongoDB: persist results + aggregation pipeline
```

This maps to the ten technical requirements: import libraries → load into HDFS → create RDDs →
transform to DataFrames → clean/preprocess → query with Spark SQL → write to Cassandra → read
back for validation → visualise → interpret.

## Repository contents

- `Assignment2_STQD6324_P166248.ipynb.ipynb` — the full pipeline notebook (Spark + Cassandra + HDFS, MongoDB extension, visualisations and interpretation).
- `Assignment2_README.md` — this file.

The MovieLens data files are **not** stored in this repository (the dataset licence forbids
redistribution). The notebook downloads them automatically at runtime from GroupLens.

---

## Reproducibility

The notebook is **fully self-contained** and designed to run end-to-end in **Google Colab**
with no manual setup. It installs Hadoop, Spark, Cassandra and MongoDB inside the session and
downloads the dataset itself.

### Run it in Colab (recommended)

1. Open the notebook in Colab (use the "Open in Colab" badge at the top of the notebook, or
   upload the `.ipynb` at <https://colab.research.google.com>).
2. **Runtime → Run all.**
3. The setup cells take a few minutes on first run (they download Hadoop, Cassandra, MongoDB
   and the dataset, and the Spark–Cassandra connector from Maven). After setup, the analysis
   runs in seconds.

Because everything runs co-located inside one Colab VM, HDFS, Cassandra and MongoDB are all
reachable on `localhost`, so no external accounts, cloud networking or credentials are needed.

- **HDFS** — a single-node Hadoop cluster is started inside the VM; the data is loaded with
  `hdfs dfs -put` and Spark reads it back over `hdfs://localhost:9000`.
- **Spark** — local mode (`local[*]`); the Spark–Cassandra connector is supplied via
  `spark.jars.packages`.
- **Cassandra** — started on `127.0.0.1:9042`; results are written and read back.
- **MongoDB** — started on `127.0.0.1:27017` for the optional extension.

### Environment / versions

| Component | Version |
|-----------|---------|
| Python | 3.x (Colab default) |
| Apache Hadoop | 3.3.6 (single-node HDFS) |
| Apache Spark | 3.5.1 (via `pip install pyspark`) |
| Apache Cassandra | 4.1.4 |
| spark-cassandra-connector | 3.5.1 (`_2.12`) |
| MongoDB | 7.0.14 |
| Python libraries | `cassandra-driver`, `pymongo`, `pandas`, `matplotlib` |

### Optional speed-ups

- **Faster downloads:** large files are fetched from Apache's CDN (`dlcdn`) with a fallback to
  the slower archive server.
- **Drive caching (optional):** a cell can cache the Hadoop/Cassandra/MongoDB tarballs to
  Google Drive so repeated runs skip the downloads. It is **off by default** — leave it unrun
  for a clean one-click execution.

### Notes / troubleshooting

- **First HDFS write:** HDFS may briefly sit in safe mode while the datanode registers; the
  setup includes a short wait and a `safemode leave`, and prints `dfsadmin -report` so you can
  confirm the datanode is live before continuing.
- **MongoDB download:** the URL targets the Ubuntu 22.04 build (`ubuntu2204`). If Colab is on a
  different Ubuntu and the download 404s, change `ubuntu2204` to `ubuntu2004` (or bump the
  version) in the MongoDB setup cell.
- **Running outside Colab:** the notebook assumes a Linux environment running as root (as Colab
  does). On a normal cluster, drop the `-R` flag from the Cassandra start command and adjust the
  HDFS user paths accordingly.

---

## References

**Dataset**

- F. M. Harper and J. A. Konstan (2015). *The MovieLens Datasets: History and Context.* ACM
  Transactions on Interactive Intelligent Systems (TiiS) 5(4): Article 19.
  <https://doi.org/10.1145/2827872>
- GroupLens Research, University of Minnesota. *MovieLens 100K Dataset.*
  <https://grouplens.org/datasets/movielens/100k/>

**Software**

- Apache Spark — <https://spark.apache.org/docs/latest/>
- Apache Hadoop (HDFS) — <https://hadoop.apache.org/docs/stable/>
- Apache Cassandra — <https://cassandra.apache.org/doc/latest/>
- DataStax Spark–Cassandra Connector — <https://github.com/datastax/spark-cassandra-connector>
- MongoDB — <https://www.mongodb.com/docs/>
- PySpark, `cassandra-driver`, `pymongo`, `pandas`, `matplotlib` (official documentation)

---

## Acknowledgement

This dataset is provided by GroupLens Research. Per the dataset's terms, it is used here for
academic coursework only and is not redistributed in this repository.
