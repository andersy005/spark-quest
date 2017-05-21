# Resilient Distributed Dataset and DataFrames

**Learning Goals**:
- Describe Spark’s primary data abstraction
- Understand how to create parallelized collections and external datasets
- Work with Resilient Distributed Dataset (RDD) operations and DataFrames
- Utilize shared variables and key-value pairs


## RDD

- Fault-tolerant collection of elements that can be operated on in parallel.
- Immutable
- Three methods for creating RDD:
    - Parallelizing an existing collection
    - Referencing a dataset
    - Transformation from an existing RDD

- Two types of RDD operations:
    - Transformations
    - Actions

- Dataset from any storage supported by Hadoop:
    - HDFS
    - Cassandra
    - HBase
    - Amazon S3

- Types of files supported:
    - Text files
    - SequenceFiles
    - Hadoop InputFormat

## Initializing Spark

The first thing a Spark program must do is create a **sparkContext** object, which tells Spark how to access a cluster.

To Create a *SparkContext* you first need to build a **SparkConf** object that contains information about the application.

```python
conf = SparkConf().setAppName(appName).setMaster(master)
sc = SparkContext(conf=conf)
```

## Using the Shell

For example, to run bin/pyspark on exactly four cores, use:
```
$ ./bin/pyspark --master local[4]
```

Or, to also add code.py to the search path (in order to later be able to import code), use:
```
$ ./bin/pyspark --master local[4] --py-files code.py
```

To use the Jupyter notebook:
```
$ PYSPARK_DRIVER_PYTHON=jupyter ./bin/pyspark
```

## Parallelized Collections

```python
data = [1, 2, 3, 4, 5]
distData = sc.parallelize(data)
```

Once Created, the distributed dataset(distData) can be operated on in parallel. For example, we can call 
```python 
distData.reduce(lambda a, b: a + b)
```
 to add up the elements of the list.


 ## External Datasets

Alternatively we can create an RDD from an external Dataset

 ```python
 distFile  = sc.textFile("data.txt")
 ```


 ## RDD operations - Basics

 - Loading a file
 ```python
 lines = sc.textFile("hdfs://data.txt")
 ```
 - Applying transformation
 ```python
 line_lenghts = lines.map(s=> s.length)
 ```
 - Invoking action
 ```python
 total_lengths = line_lengths.reduce((a, b) => a + b)
 ```
 - MapReduce example:
 ```python
 word_counts = textFile.flatMap(line => line.split (" "))\
                        .map(word => (word,1))\
                        .reduceByKey((a, b) => a + b)

 word_counts.collect()
 ```


## RDD Operations - Transformations

- A subset of the transformations available.
- Transformations are lazy evaluations
- Returns a pointer to the transformed RDD

Some Examples:

| Transformation                | Meaning                                                                                                                                                                                                                                                                                                  |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| map(func)                     | Return a new distributed dataset formed by  passing each  element  of the  source through a function func.                                                                                                                                                                                               |
| filter(func)                  | Return a new dataset formed by selecting those  elements of the source on which func returns true.                                                                                                                                                                                                       |
| flatMap(func)                 | Similar to map, but each input item can be mapped to 0 or more output items (so func should return a  Seq rather than a single item).                                                                                                                                                                    |
| reduceByKey(func, [numTasks]) | When called on a dataset of (K, V) pairs, returns a dataset of (K, V)  pairs where the values for each key are aggregated using the given  reduce function func, which must be of type (V,V) => V. Like in  groupByKey, the number of reduce tasks is configurable through  an optional second argument. |



## RDD Operations - Actions

- Actions return values

Some Examples:

| Transformation | Meaning                                                                                                                                                                                                                                                                                 |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| collect()      | Return all the elements of the dataset as an array  at the driver program. This is usually useful after a filter  or other operation that returns a sufficiently small subset of the data.                                                                                              |
| count()        | Return the number of elements in the dataset.                                                                                                                                                                                                                                           |
| first()        | Return the first element of the dataset (similar to take(1)).                                                                                                                                                                                                                           |
| take(n)        | Return an array with the first n elements of the dataset.                                                                                                                                                                                                                               |
| foreach(func)  | Run a function func on each element of the dataset. This is usually done  for side effects such as updating an Accumulator or interacting with external  storage systems. Note: modifying variables other than Accumulators outside of the foreach() may result in undefined behavior.  |


## RDD Persistence

- One of the most important capabilities in Spark is **persisting(or caching)** a dataset in memory across operations.

- Each node stores any partitions of the cache that it computes in memory.
- Reuses them in other actions on that dataset( or datasets derived from it)
    - Future actions are much faster (often by more than 10x)

- Two methods for RDD persistence
    - ```persist()```
    - ```cache()``` -> essentially just persist with ```MEMORY_ONLY``` storage


## Shared Variables and key-value pairs

- When a function is passed from the driver to a worker, normally a separate copy of the variables are used.
- Two types of variables:
    - Broadcast variables
        - Read-Only copy on each machine
        - Distribute broadcast variables using efficient broadcast algorithms
    - Accumulators
        - variables added through an associative operation
        - Implement counters and sums
        - Only the driver can read the accumulators value
        - Numeric types accumulators. Extend for new types
```python
pair = ('a', 'b')
pair[0] # will return 'a'
pair[1] # will return 'b'
```




## Review Questions

1. What happens when an action is executed?
     - Executors prepare the data for operation in parallel  
     - The driver sends code to be executed on each block  
     - A cache is created for storing partial results in memory  
     - Data is partitioned into different blocks across the cluster