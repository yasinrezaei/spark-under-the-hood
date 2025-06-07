# 📄 MapPartitionsRDD.scala

## 🔍 Purpose

`MapPartitionsRDD` is a subclass of `RDD` used to implement transformations like `map`, `flatMap`, and `filter`. It applies a user-defined function to **each partition** (not each element), which makes it more efficient and flexible for certain computations.

This is one of the most commonly created RDD types in Spark’s core transformations.

---

## 🧱 Key Classes & Traits

| Class / Trait | Description |
|---------------|-------------|
| `MapPartitionsRDD[U, T]` | A generic RDD that applies a function to each partition of the parent RDD and returns a new RDD of type `U`. |

---

## 🧪 Key Methods

| Method | Purpose |
|--------|---------|
| `compute(split: Partition, context: TaskContext): Iterator[U]` | Applies the function to each partition's iterator. |
| `getPartitions(): Array[Partition]` | Just returns the parent RDD’s partitions. |
| `getDependencies(): Seq[Dependency[_]]` | Returns a single narrow dependency on the parent RDD. |
| `getPreferredLocations(partition)` | For locality-aware scheduling. Delegates to parent. |

---

## 🔁 How It Works

- When a transformation like `.map(f)` is called on an RDD, it returns a new `MapPartitionsRDD(f)`.
- During execution, Spark calls `compute()` for each partition, passing the parent RDD’s iterator into `f`.
- This allows transforming a whole partition in one go, which is more efficient for complex functions or data-heavy operations.

---

## 🔗 Related Files / Classes

| File | Why it's related |
|------|------------------|
| `RDD.scala` | `MapPartitionsRDD` extends `RDD` and overrides its core methods. |
| `Task.scala` | Executes `MapPartitionsRDD.compute()` during stage execution. |
| `RDD.map()` | Transformation API in `RDD.scala` that returns a `MapPartitionsRDD`. |

---

## 🧠 My Notes / Insights

- The transformation is defined in a functional way (`f: Iterator[T] => Iterator[U]`), not per element.
- This makes it ideal for batching, database connections, or operations with high per-call cost.
- Simple transformations like `map(f)` ultimately delegate to this RDD subclass.

---

## 📌 TODO / Follow-Up

- [x] Trace `.map()` in a Spark program and confirm it returns a `MapPartitionsRDD`.
- [x] Understand how `compute()` is called by `Executor`.
- [ ] Check how Spark handles exceptions thrown inside the `f` function.
