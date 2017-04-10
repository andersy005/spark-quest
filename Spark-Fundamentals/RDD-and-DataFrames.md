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


