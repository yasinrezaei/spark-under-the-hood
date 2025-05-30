# 🔍 Deep Dive into Apache Spark Source Code

Welcome to my personal deep-dive into the [Apache Spark](https://github.com/apache/spark) source code. This project is a curated, structured study of Spark’s internals — designed to help others understand its architecture, key components, and internal execution flow.

Each module is explored with detailed Markdown notes, code tracing, diagrams, and summaries.

---

## 📁 Project Structure

---

## 📌 Goals

- Understand how Apache Spark works under the hood
- Summarize key components like RDD, DAG scheduler, Catalyst optimizer
- Provide annotated diagrams and call flows
- Make it easier for new contributors to onboard into Spark

---

## 🧠 Topics Covered

### 🔧 Core Components (`core/`)
| File | Summary |
|------|---------|
| `RDD.md` | Explains Spark's RDD abstraction, lazy evaluation, lineage, compute model |
| `SparkContext.md` | The entry point of any Spark application – driver program interactions |
| `DAGScheduler.md` | Translates jobs into stage DAGs |
| `TaskScheduler.md` | Handles actual task scheduling and execution on executors |

---

### 🧮 SQL Engine (`sql/`)
| File | Summary |
|------|---------|
| `Catalyst.md` | Deep dive into the Catalyst query optimization framework |
| `LogicalPlans.md` | How logical query plans are constructed |
| `Optimizer.md` | How Catalyst applies optimization rules |
| `Tungsten.md` | Physical execution layer focused on memory + CPU efficiency |

---

### 🔁 Streaming (`streaming/`)
| File | Summary |
|------|---------|
| `Overview.md` | Architecture of Spark Streaming, micro-batches, DStreams |

---

### 📌 Examples (`examples/`)
| File | Summary |
|------|---------|
| `sparkpi-explained.md` | Breaks down the `SparkPi` example to understand execution flow |

---

## 🧭 How to Use This Repository

If you're a:
- **Beginner** → Start with `examples/sparkpi-explained.md`
- **Intermediate** → Move to `core/RDD.md` and `core/SparkContext.md`
- **Advanced** → Explore `sql/`, Catalyst, and internal optimizations

---

## 📊 Diagrams

Visual aids are in the `diagrams/` folder. For example:

![Spark Architecture](diagrams/spark-architecture.png)

---

## 🔗 References

- [Apache Spark GitHub](https://github.com/apache/spark)
- [Spark Architecture Overview](https://spark.apache.org/docs/latest/cluster-overview.html)
- [Spark SQL Guide](https://spark.apache.org/docs/latest/sql-getting-started.html)

---

## 🙌 Contributions

This is a personal learning project. Feel free to fork, star, or suggest improvements via issues or PRs.

---

## 📅 Progress Tracker

| Module | Status |
|--------|--------|
| RDD.md | ✅ In Progress |
| SparkContext.md | ⏳ Planned |
| Catalyst.md | ⏳ Planned |
| ... | ... |

---

## 🧑‍💻 Author

Maintained by [Yasin Rezaei](https://github.com/yasinrezaei)

---


