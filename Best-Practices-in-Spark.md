# What Are The Best Practices in Spark?

There are tons of possibilities when you’re working with PySpark, but that doesn’t mean that there are some simple and general best practices that you can follow:

## 1.Use Spark DataFrames

Spark DataFrames are optimized and therefore also faster than RDDs. Especially when you’re working with structured data, you should really consider switching your RDD to a DataFrame.

## 2.RDD Best Practices

Don’t call ```collect()``` on large RDDs

By calling collect() on any RDD, you drag data back into your applications from the nodes. Each RDD element will be copy onto the single driver program, which will run out of memory and crash. Given the fact that you want to make use of Spark in the most efficient way possible, it’s not a good idea to call ```collect()``` on large RDDs.

Other functions that you can use to inspect your data are ```take()``` or ```takeSample()```, but also ```countByKey()```, ```countByValue()``` or ```collectAsMap()``` can help you out. If you really need to take a look at the complete data, you can always write out the RDD to files or export it to a database that is large enough to keep your data.

## 3.Reduce Your RDD Before Joining

The fact that you can chain operations comes in handy when you’re working with Spark RDDs, but what you might not realize is that you have a responsibility to build efficient transformation chains, too. Taking care of the efficiency is also a way of tuning your Spark jobs’ efficiency and performance.

One of the most basic rules that you can apply when you’re revising the chain of operations that you have written down is to make sure that you filter or reduce your data before joining it. This way, you avoid sending too much data over the network that you’ll throw away after the join, which is already a good reason, right?

But there is more. The join operation is one of the most expensive operations that you can use in Spark, so that’s why it makes sense to be wary of this. When you reduce the data before the join, you avoid shuffling your data around too much.

## 4.Avoid ```groupByKey()``` on large RDDs

On big data sets, you’re better off making use of other functions, such as ```reduceByKey()```, ```combineByKey()``` or ```foldByKey()```. When you use ```groupByKey()```, all key-value pairs are shuffled around in the cluster. A lot of unnecessary data is being transferred over the network. Additionally, this also means that if more data is shuffled onto a single machine than can fit in memory, the data will be spilled to disk. This heavily impacts the performance of your Spark job.

When you make use of ```reduceByKey()```, for example, the pairs with the same key are already combined before the data is shuffled. As a result, you’ll have to send less data over the network. Next, the reduce function is called again so that all the values from each partition are reduced.

## 5.Broadcast Variables

Since you already know what broadcast variables are and in which situations they can come in handy, you’ll also have gathered that this is one of the best practices for when you’re working with Spark because you can reduce the cost of launching a job over the cluster.

Avoid ```flatmap()```, ```join()``` and ```groupBy()``` Pattern
When you have two datasets that are grouped by key and you want to join them, but still keep them grouped, use ```cogroup()``` instead of the above pattern.

## 6.Spark UI
Making use of the Spark UI is really something that you can not miss. This web interface allows you to monitor and inspect the execution of your jobs in a web browser, which is extremely important if you want to exercise control over your jobs.

The Spark UI allows you to maintain an overview off your active, completed and failed jobs. You can see when you submitted the job, and how long it took for the job to run. Besides the schematic overview, you can also see the event timeline section in the “Jobs” tab. Make sure to also find out more about your jobs by clicking the jobs themselves. You’ll get more information on the stages of tasks inside it.

The “Stages” tab in the UI shows you the current stage of all stages of all jobs in a Spark application, while the “Storage” tab will give you more insights on the RDD size and the memory use.