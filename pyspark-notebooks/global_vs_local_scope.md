#Global versus local scope




One of the things that you, as a prospective PySpark user, need to get used to is the inherent parallelism of Spark. Even if you are proficient in Python, executing scripts in PySpark requires shifting your thinking a bit.


Spark can be run in two modes: Local and cluster. When you run Spark locally your code might not differ to what you are currently used to with running Python: Changes would most likely be more syntactic than anything else but with an added twist that data and code can be copied between separate worker processes.However, taking the same code and deploying it to a cluster might cause a lot of head-scratching if you are not careful. 

This requires understanding how Spark executes a job on the cluster.

In the cluster mode, when a job is submitted for execution, the job is sent to the driver (or a master) node. The driver node creates a DAG for a job and decides which executor (or worker) nodes will run specific tasks.

The driver then instructs the workers to execute their tasks and return the results to the driver when done. Before that happens, however, the driver prepares each task's closure: A set of variables and methods present on the driver for the worker to execute its task on the RDD.

This set of variables and methods is inherently static within the executors' context, that is, each executor gets a copy of the variables and methods from the driver. If, when running the task, the executor alters these variables or overwrites the methods, it does so without affecting either other executors' copies or the variables and methods of the driver. 

This might lead to some unexpected behavior and runtime bugs that can sometimes be really hard to track down.


Note: Check out this discussion in PySpark's documentation for a more hands-on example: http://spark.apache.org/docs/latest/programming-guide.html#local-vs-cluster-modes.