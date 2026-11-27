# HomeLake

Adaptive SQL Lakehouse for heterogeneous data sources, featuring schema evolution, unified SQL queries, and intelligent small-file compaction.

> **Data Driven Systems** (MSc, Universidade de Aveiro, 2026) - Homework Assignment 2, **Option B: SQL on file system documents and databases**

## Overview

HomeLake is a platform for querying, integrating and analyzing heterogeneous data stored in **file systems, data lakes and databases** through **SQL**, following the principles of a **data lakehouse** architecture.

The focus is on **data management, integration, organization, updates and optimization** across the whole data pipeline. Raw SQL execution speed is not the main goal.

## Requirements

### 1. Heterogeneous sources and formats
Support for different data formats and sources:

| Category | Targets |
|---|---|
| Structured / semi-structured files | CSV, JSON, XML, Parquet, Avro, ORC |
| Tabular documents | Excel |
| Documents | PDF |
| Relational databases | e.g. PostgreSQL |
| Object stores | e.g. RustFS, MinIO, Amazon S3 |
| APIs | REST / HTTP sources |

### 2. Unified SQL
Integrated SQL querying across all sources, including joins between different formats and systems.

### 3. Schema evolution
Automatically detect schema changes and adapt the ingestion and query pipelines to them, while keeping data **consistent and compatible**.

### 4. Small files compaction
An automated **middleware** that monitors storage partitions and dynamically compacts small files into **optimally sized Parquet files**.

### 5. Consistency during compaction
Compaction must run **without disrupting active SQL queries**.

## Tech Stack (planned)

- **Python**: core implementation
- **DuckDB**: unified SQL query engine
- **Parquet**: lakehouse storage format
- **PostgreSQL**: relational source and metadata
- **RustFS**: S3-compatible object store (Apache 2.0; replaces MinIO, whose open-source edition is no longer maintained)

## Principles

- Modular architecture that can be extended later.
- Avoid unnecessary complexity.
- Every requirement backed by tests and measurable experiments.

## Deliverables

- Documented source code (datasets excluded from the repository).
- Technical report (up to 3,000 words).
- In-class presentation.

## Documentation

| Document | Purpose |
|---|---|
| [docs/HW02.pdf](docs/HW02.pdf) | Assignment brief (Option B) |
| [docs/state-of-art.md](docs/state-of-art.md) | Literature review and innovation opportunities |
| [docs/architecture.md](docs/architecture.md) | Baseline architecture, components and experiments |

## Future Vision

HomeLake could later be adapted to a **Smart Home** setting, with data from sensors, energy use, temperature, humidity and devices. This is **out of scope** for the current assignment.

## License

See [LICENSE](LICENSE).
