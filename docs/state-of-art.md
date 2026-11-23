# State of the Art

Literature review supporting HomeLake (Data Driven Systems, Assignment 2, Option B).
Main source: **IEEE Xplore**. Key non-IEEE references are added where they are foundational.

> **Status:** first pass (2026-11-21). Entries are based on **abstracts and search summaries**, not full-text reading.
> Before citing in the report, open each paper and confirm the claims marked here.

---

## 1. Lakehouse architecture & heterogeneous sources

| Paper | Venue | Key idea | Relevance |
|---|---|---|---|
| [A Lakehouse Architecture for the Management and Analysis of Heterogeneous Data for Biomedical Research and Mega-biobanks](https://ieeexplore.ieee.org/document/9671534/) | IEEE | Lakehouse to unify heterogeneous biomedical data; notes the lakehouse is still partly a conceptual construct. | Reference architecture for integrating mixed sources. |
| [Implementing Big Data Lake for Heterogeneous Data Sources](https://ieeexplore.ieee.org/iel7/8743499/8750905/08750951.pdf) | IEEE | Data lake built on big-data tools for collection, storage and integration of heterogeneous data (smart-city context). | Ingestion layer design. |
| [Precomputation-Optimized Lakehouse Architecture for OLAP Tasks](https://ieeexplore.ieee.org/document/11120579/) | IEEE | Lakehouses struggle to keep SQL performance at scale; precomputation improves queries by 6.1%–64.9%. | Shows performance is a known lakehouse weakness, which motivates compaction. |
| [A Use Case of Data Lake Metadata Management](https://ieeexplore.ieee.org/document/9822039/) | IEEE | Metadata across the lifecycle and zones of a data lake. | Catalog design. |

**Foundational (non-IEEE):**
- Armbrust et al., *Lakehouse: A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics*, CIDR 2021.
- Armbrust et al., *Delta Lake: High-Performance ACID Table Storage over Cloud Object Stores*, VLDB 2020.
- Hai et al., [Data Lakes: A Survey of Functions and Systems](https://arxiv.org/pdf/2106.09592), arXiv / IEEE TKDE.

## 2. Unified SQL over heterogeneous data (federation / polystores)

| Paper | Venue | Key idea | Relevance |
|---|---|---|---|
| [One Size Does Not Fit All: Querying Web Polystores (PolyWeb)](https://ieeexplore.ieee.org/document/8615997/) | IEEE | Federated queries over native data models, avoiding costly conversion (~3 h for 3.5 GB). | Argues for **query-in-place** rather than eager conversion. |
| [Federated Querying of Heterogeneous Spatiotemporal Data over Polyglot Stores (Hermes)](https://ieeexplore.ieee.org/document/11662514/) | IEEE | Unifying query layer over native stores; heuristic predicate ordering and result caching. | Ideas for query layer optimizations. |
| [Apache Calcite: Optimized Query Processing over Heterogeneous Data Sources](https://arxiv.org/pdf/1802.10233) | SIGMOD 2018 (arXiv) | Adapter-based federated SQL framework. | Alternative/related design to DuckDB extensions. |
| [DuckDB in Action](https://ieeexplore.ieee.org/document/10745360/) | Manning (IEEE Xplore) | DuckDB reading CSV/JSON/Parquet from Python. | Practical reference for the query engine. |

## 3. Schema evolution

| Paper | Venue | Key idea | Relevance |
|---|---|---|---|
| [Uncovering the Evolution History of Data Lakes](https://ieeexplore.ieee.org/document/8258204/) | IEEE | Reconstructs a **sequence of schema versions** from data; uses inclusion dependencies to map versions. | Schema version history in the catalog. |
| [NoSQL Schema Evolution and Big Data Migration at Scale](https://ieeexplore.ieee.org/document/7840924/) | IEEE | **Lazy vs. eager** migration; composite migrations across version chains. | Strategy choice: rewrite data or resolve at read time. |
| [Towards Self-Adapting Data Migration in the Context of Schema Evolution in NoSQL Databases](https://ieeexplore.ieee.org/document/9094136/) | IEEE | Choose migration strategy automatically based on workload and change type. | **Innovation hook:** adaptive policy per change type. |
| Scherzinger, Klettke, Störl, [Managing Schema Evolution in NoSQL Data Stores](https://arxiv.org/pdf/1308.0514) | DBPL 2013 (arXiv) | Declarative schema evolution language. | Change taxonomy (add / delete / rename / move / copy). |

**Related ideas:** GEMMS and Constance infer schemas from JSON/XML (cited in the Hai et al. survey). Patent literature (e.g. USPTO 10831726) **classifies changes** into ones that affect storage (column add) and ones that don't, and uses **checksums of schemas** for cheap change detection.

## 4. Small files problem & compaction

| Paper | Venue | Key idea | Relevance |
|---|---|---|---|
| [OCF-HP: An Offline Compaction Framework Based on Hotspot Prediction for Data Lakehouse](https://ieeexplore.ieee.org/document/10593781/) | IEEE | Schedules Hudi compaction based on **predicted hot partitions**. | **Closest match**: workload-aware compaction scheduling. |
| [An Effective Merge Strategy Based Hierarchy for Improving Small File Problem on HDFS](https://ieeexplore.ieee.org/document/7790278/) | IEEE | Hierarchical merge of small files. | Merge strategy design. |
| [An Optimized Strategy for Small Files Storing and Accessing in HDFS (OMSS)](https://ieeexplore.ieee.org/document/8005864/) | IEEE | **Worst-fit bin packing** to merge small files into blocks; −34.7% memory vs. MapFile. | Packing algorithm for target-size Parquet. |
| [An Optimized Approach for Storing Small Files on HDFS Based on Dynamic Queue (DQSF)](https://ieeexplore.ieee.org/document/8281195/) | IEEE | Size-based queues for merging. | Alternative packing strategy. |
| [LHF: Archive-Based Approach to Accelerate Massive Small Files Access in HDFS](https://ieeexplore.ieee.org/document/8848237/) | IEEE | Merge + index for fast lookup. | Index after compaction. |

## 5. Consistency during compaction (concurrent queries)

| Paper | Venue | Key idea | Relevance |
|---|---|---|---|
| Berenson et al., [A Critique of ANSI SQL Isolation Levels](https://arxiv.org/pdf/cs/0701157) | SIGMOD 1995 | Defines **snapshot isolation** (MVCC; first-committer-wins). | Formal guarantee to claim for compaction. |
| [TreeCat: Standalone Catalog Engine for Large Data Systems](https://arxiv.org/pdf/2503.02956) | arXiv | Delta uses snapshot-based OCC with atomic log append; single-table transactions. | Catalog commit protocol. |
| [Towards Unified File Management for Data Lakehouse Systems (Raven)](https://arxiv.org/pdf/2611.15975) | arXiv | Commit as atomic multi-row CAS over table snapshots in a metadata store. | **Metadata store in PostgreSQL** pattern. |
| [Compressed Lineage for Lakehouse DAGs: Row-Group Provenance](https://ieeexplore.ieee.org/abstract/document/11638715/) | IEEE | Row-group level provenance. | Optional: lineage across compaction. |

## 6. Data layout & file sizing

| Paper | Venue | Key idea | Relevance |
|---|---|---|---|
| [Column Cache: Buffer Cache for Columnar Storage on HDFS](https://ieeexplore.ieee.org/document/8622527/) | IEEE | Parquet-aware caching (up to 1.28× TPC-DS). | Read path optimization. |
| [A Segment-Level Adaptive Data Layout Scheme for Parallel File Systems](https://ieeexplore.ieee.org/document/5948632/) | IEEE | Adaptive stripe size per access pattern. | Inspiration for **adaptive target file size**. |
| [Storing Parquet Tile by Tile: Application-Aware Storage with Deduplication](https://ieeexplore.ieee.org/document/8892170/) | IEEE | Parquet-aware storage. | Parquet internals. |

---

## Gaps & Innovation Opportunities for HomeLake

Each opportunity maps to an Option B requirement and to a measurable experiment.

| # | Idea | Builds on | Requirement | How to measure |
|---|---|---|---|---|
| 1 | **Snapshot-based compaction with a PostgreSQL catalog:** readers pin a snapshot (list of files); compaction writes new Parquet, then swaps the snapshot pointer atomically in one transaction; old files are deleted only after no reader holds them. | Delta/Iceberg, Raven, SI theory | Consistency | Run queries in a loop during compaction: 0 failures, identical results before/after. |
| 2 | **Workload-aware compaction policy:** prioritize partitions by small-file count × query frequency instead of a fixed schedule. | OCF-HP | Small files | Query latency and file count over time vs. naive policy. |
| 3 | **Bin-packing merge to a target Parquet size** (worst-fit / first-fit decreasing). | OMSS, DQSF | Small files | Number of output files, size variance, latency vs. naive concatenation. |
| 4 | **Schema change classification** (compatible: add column, widen type; breaking: drop, rename, narrow type) with a policy per class: auto-evolve, read-time projection, or quarantine. | Patents, Scherzinger et al., self-adapting migration | Schema evolution | Inject changes; check that queries keep working and that breaking changes are blocked. |
| 5 | **Lazy vs. eager schema migration**: resolve old files at query time (union by name + defaults) vs. rewrite on compaction. Compaction becomes the natural moment for eager rewrite. | NoSQL migration papers | Schema evolution + small files | Query latency and storage cost under both modes. |
| 6 | **Schema version history** stored in the catalog, queryable via SQL. | Uncovering evolution history | Schema evolution | Demo: list versions and diffs per dataset. |
| 7 | **Query-in-place federation via DuckDB** (Parquet/CSV/JSON native, PostgreSQL scanner, S3 via httpfs) vs. ingest-to-Parquet; compare both. | PolyWeb, Hermes, Calcite | Unified SQL | Latency and freshness: federated vs. materialized. |

**Suggested core for the assignment:** ideas 1, 3, 4 and 7, with 2 and 5 as extensions. Ideas 1–4 are the clearest academic contributions and are directly testable.

## Next steps

- [ ] Read full text of: OCF-HP, OMSS, Uncovering the evolution history of data lakes, NoSQL schema evolution at scale, Raven.
- [ ] Add Iceberg spec, Delta Lake paper and the Lakehouse CIDR paper as formal references.
- [ ] Confirm each venue/year before using in the report.
