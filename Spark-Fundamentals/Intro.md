# Introduction To Spark

- Apache Spark is a computing platform designed to be fast and general-purpose, and easy to use:
    - **Speed**:
        - In-memory computations
        - Faster than MapReduce for complex applications on disk
    - **Generality**:
        - Covers a wide range of workloads on one system
        - Batch applications(e.g: MapReduce)
        - Iterative algorithms
        - Interactive queries and streaming

    - **Ease of Use**:
        - APIs for scala, Python, Java
        - Libraries for SQL, machine learning, streaming, and graph processing
        - Runs on Hadoop clusters or as a standalone


## Why use Spark?
- Parallel distributed processing, fault tolerance on commodity hardware, scalability, in-memory computing, high level APIs, etc.

- Analyze and model the data to obtain insight using ad-hoc analysis
- Transforming the data into a useable format
- Statistics, machine learning, SQL
- Ease of Use
- A wide variey of functionality


## Resilient Distributed Datasets(RDD)
- Spark's primary abstraction
- Distributed collection of elements
- Parallelized across the cluster
- Two types of RDD operations
    - Transformations
        - Creates a DAG
        - Lazy Evaluations
        - No return value
    - Actions
        - Performs the transformations and the action that follows
        - Returns a value

- Fault tolerance
- Caching
- Example of RDD flow:

Hadoop RDD -> Filtered RDD -> Mapped RDD -> Reduced RDD