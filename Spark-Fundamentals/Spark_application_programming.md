# Spark Application Programming

**Learning Objectives**:

- Understand the purpose and usage of the SparkContext
- Initialize Spark with the various programming languages
- Describe and run some Spark examples
- Pass functions to Spark
- Create and run a Spark standalone application
- Submit applications to the cluster


## SparkContext
- The main entry point for Spark functionality
- Represents the connection to a Spark cluster
- Create RDDs, accumulators, and broadcast variables on that cluster
- In the Spark shell, the SparkContext, sc, is automatically initialized for you to use.
- In a Spark program, import some classes and implicit conversions into your program:

```python
from pyspark import SparkContext
from pyspark import SparkConf
```


## Linking with Pyspark
- Uses the standard CPython interpreter, so libraries like Numpy can be used
- To run Spark applications in Python, use the *bin/spark-submit* script loaded in the Spark directory.
    - Load Spark's Java/Scala libraries
    - Allow you to submit applications to cluster

- If you wish to access HDFS, you need to build a PySpark linking your version of HDFS.

## Initializing Spark - Python
- Build a SparkConf object that contains information about your application

```conf = SparkConf().setAppName(appName).setMaster(master)```
- The *appName* parameter -> Name of your application to show on the cluster UI
- The *master* parameter -> is a Spark, Mesos, or YARN cluster URL(or a special "local" string to run in local mode)
    - In production mode, do not hardcode *master* in the program. Launch with *spark-submit* and provide it there.
    - In testing, you can pass "local" to run Spark
- Then, you will need to create the SparkContext object.

```sc = SparkContext(conf=conf)```

## Passing functions to Spark
- Spark's API relies on heavily passing functions in the driver program to run on the cluster.
- Three methods:
    - **Anonymous function syntax**
    - **Static methods in a global singleton object**
    - **Passing by reference**