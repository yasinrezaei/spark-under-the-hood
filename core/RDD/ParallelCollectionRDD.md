# 📄 ParallelCollectionRDD.scala

## 🔍 Purpose

`ParallelCollectionRDD` is a concrete subclass of `RDD` that represents a dataset constructed from an **in-memory Scala collection** (like a `Seq`, `Range`, or `Array`) and **parallelized** across a Spark cluster.

It's used internally when a user calls `SparkContext.parallelize()`.

---

## 🧱 Key Classes & Traits

| Class / Trait | Description |
|---------------|-------------|
| `ParallelCollectionRDD[T]` | An RDD that reads data from a local Scala collection and splits it into partitions for parallel execution. |
| `ParallelCollectionPartition` | Represents a partition of the collection, containing metadata about index and range. |

---

## 🧪 Key Methods

| Method | Purpose |
|--------|---------|
| `compute(split: Partition, context: TaskContext): Iterator[T]` | Slices the original collection into the data range for the current partition. |
| `getPartitions(): Array[Partition]` | Divides the collection into specified number of partitions. Returns an array of `ParallelCollectionPartition`. |
| `getPreferredLocations(partition)` | Returns `Nil` (no locality preference). |
| `countApproxDistinctByKey()` | Additional optimizations available for counting in some versions. |
| Constructor | Accepts collection, number of slices, and optional location preferences. |

---

## 🔁 How It Works

1. A user calls `sc.parallelize(Seq(1, 2, 3, 4), numSlices = 2)`.
2. This calls the `ParallelCollectionRDD` constructor with:
   - The original collection
   - The desired number of slices (partitions)
3. `getPartitions()` splits the collection using ranges and returns an array of `ParallelCollectionPartition`.
4. At runtime, `compute()` is called for each partition, which simply returns an iterator over that partition’s range from the original data.

---

## 🔗 Related Files / Classes

| File | Why it's related |
|------|------------------|
| `RDD.scala` | `ParallelCollectionRDD` extends the abstract RDD class. |
| `SparkContext.scala` | `parallelize()` creates instances of this RDD. |
| `Task.scala`, `Executor.scala` | Run the actual `compute()` for each partition. |

---

## 🧠 My Notes / Insights

- This is a useful RDD for **testing**, **prototyping**, or **small-scale parallel computation**.
- Since data originates from the **driver memory**, it’s not suitable for large datasets.
- No shuffling or external data access — it's 100% local to driver memory, then broadcast to workers via task serialization.
- Can be used to simulate distributed operations without needing external input sources like HDFS or S3.

---

## 📌 TODO / Follow-Up

- [x] Try using `parallelize(1 to 1000)` in a Spark shell and observe how partitions are created.
- [ ] Confirm behavior when collection size < numSlices.
- [ ] Explore performance implications of large collections vs partitions.

