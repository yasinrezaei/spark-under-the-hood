# 📄 RDD.scala

## 🔍 Purpose

This file defines the abstract class `RDD[T]`, which represents a **Resilient Distributed Dataset** — the core data structure in Spark. It provides a **lazy, distributed, immutable collection** of objects that can be transformed and acted upon using functional-style operations.

`RDD` is the **foundation of Spark’s execution engine**, allowing lineage-based fault tolerance, partitioning, and distributed computation.

---

## 🧱 Key Classes & Traits

| Class / Trait | Description |
|---------------|-------------|
| `RDD[T]` | Abstract base class for all RDDs. Defines the API and behavior for distributed datasets. |
| `Partition` | Represents a split of data within an RDD. Each RDD must define its own `getPartitions()` logic. |
| `Dependency[T]` | Describes how an RDD depends on its parent(s), e.g., narrow vs shuffle. |

---

## 🧪 Key Methods

| Method | Purpose |
|--------|---------|
| `compute(partition: Partition, context: TaskContext): Iterator[T]` | Abstract method. Subclasses implement this to compute data for a partition. |
| `getPartitions(): Array[Partition]` | Abstract method. Returns the set of partitions for the RDD. |
| `dependencies: Seq[Dependency[_]]` | Defines the parent RDDs and how they're related (lineage). |
| `map`, `flatMap`, `filter` | Transformation methods — return new RDDs with new computation logic. |
| `persist`, `cache` | Mark the RDD for storage in memory or disk for reuse. |
| `collect`, `count`, `take` | Action methods that trigger execution and return results to the driver. |
| `iterator(partition, context)` | Internal method used by `computeOrReadCheckpoint`. |
| `partitions` | Lazily initialized via `getPartitions`. |
| `runJob` (via SparkContext) | Triggers a job that applies a function to RDD elements. |

---

## 🔁 How It Works

- Users never instantiate `RDD` directly — they use APIs like `parallelize()`, `textFile()`, or transformations like `map()`.
- Each transformation creates a **new RDD object** that holds a reference to its parent.
- When an action is called, Spark traces back the RDD lineage graph, splits it into stages (in `DAGScheduler`), and schedules tasks to compute each partition by calling `compute()`.

---

## 🔗 Related Files / Classes

| File | Why it's related |
|------|------------------|
| `SparkContext.scala` | Calls `makeRDD()` and other factory methods to create RDDs |
| `Dependency.scala` | Used by `RDD.dependencies` to track lineage |
| `MapPartitionsRDD.scala` | Concrete subclass implementing `map()` and similar |
| `Task.scala` | Executes `RDD.compute()` for a given partition |
| `BlockManager.scala` | Stores RDD partitions when persisted |
| `DAGScheduler.scala` | Schedules stages based on RDD dependencies |

---

## 🧠 My Notes / Insights

- RDDs are **immutable and lazily evaluated**. Each transformation adds to the DAG, but computation happens only when an action is triggered.
- `compute()` is abstract, and each concrete RDD subclass implements its own logic.
- Spark's **fault tolerance** comes from the `dependencies` field and the ability to recompute partitions from lineage.
- `partitioner` is crucial for shuffles — e.g., groupByKey vs reduceByKey.
- `persist()` and `cache()` affect where data is stored, but the logic is still in `BlockManager`.

---

## 📌 TODO / Follow-Up

- [x] Trace how `map()` creates a `MapPartitionsRDD`
- [x] Understand how `compute()` is executed by the `Executor`
- [ ] Add a sequence diagram from `map()` to `Task.compute()`
- [ ] Look into how checkpointing alters the RDD DAG
# 📄 RDD.scala

## 🔍 Purpose

This file defines the abstract class `RDD[T]`, which represents a **Resilient Distributed Dataset** — the core data structure in Spark. It provides a **lazy, distributed, immutable collection** of objects that can be transformed and acted upon using functional-style operations.

`RDD` is the **foundation of Spark’s execution engine**, allowing lineage-based fault tolerance, partitioning, and distributed computation.

---

## 🧱 Key Classes & Traits

| Class / Trait | Description |
|---------------|-------------|
| `RDD[T]` | Abstract base class for all RDDs. Defines the API and behavior for distributed datasets. |
| `Partition` | Represents a split of data within an RDD. Each RDD must define its own `getPartitions()` logic. |
| `Dependency[T]` | Describes how an RDD depends on its parent(s), e.g., narrow vs shuffle. |

---

## 🧪 Key Methods

| Method | Purpose |
|--------|---------|
| `compute(partition: Partition, context: TaskContext): Iterator[T]` | Abstract method. Subclasses implement this to compute data for a partition. |
| `getPartitions(): Array[Partition]` | Abstract method. Returns the set of partitions for the RDD. |
| `dependencies: Seq[Dependency[_]]` | Defines the parent RDDs and how they're related (lineage). |
| `map`, `flatMap`, `filter` | Transformation methods — return new RDDs with new computation logic. |
| `persist`, `cache` | Mark the RDD for storage in memory or disk for reuse. |
| `collect`, `count`, `take` | Action methods that trigger execution and return results to the driver. |
| `iterator(partition, context)` | Internal method used by `computeOrReadCheckpoint`. |
| `partitions` | Lazily initialized via `getPartitions`. |
| `runJob` (via SparkContext) | Triggers a job that applies a function to RDD elements. |

---

## 🔁 How It Works

- Users never instantiate `RDD` directly — they use APIs like `parallelize()`, `textFile()`, or transformations like `map()`.
- Each transformation creates a **new RDD object** that holds a reference to its parent.
- When an action is called, Spark traces back the RDD lineage graph, splits it into stages (in `DAGScheduler`), and schedules tasks to compute each partition by calling `compute()`.

---

## 🔗 Related Files / Classes

| File | Why it's related |
|------|------------------|
| `SparkContext.scala` | Calls `makeRDD()` and other factory methods to create RDDs |
| `Dependency.scala` | Used by `RDD.dependencies` to track lineage |
| `MapPartitionsRDD.scala` | Concrete subclass implementing `map()` and similar |
| `Task.scala` | Executes `RDD.compute()` for a given partition |
| `BlockManager.scala` | Stores RDD partitions when persisted |
| `DAGScheduler.scala` | Schedules stages based on RDD dependencies |

---

## 🧠 My Notes / Insights

- RDDs are **immutable and lazily evaluated**. Each transformation adds to the DAG, but computation happens only when an action is triggered.
- `compute()` is abstract, and each concrete RDD subclass implements its own logic.
- Spark's **fault tolerance** comes from the `dependencies` field and the ability to recompute partitions from lineage.
- `partitioner` is crucial for shuffles — e.g., groupByKey vs reduceByKey.
- `persist()` and `cache()` affect where data is stored, but the logic is still in `BlockManager`.

---

## 📌 TODO / Follow-Up

- [x] Trace how `map()` creates a `MapPartitionsRDD`
- [x] Understand how `compute()` is executed by the `Executor`
- [ ] Add a sequence diagram from `map()` to `Task.compute()`
- [ ] Look into how checkpointing alters the RDD DAG
