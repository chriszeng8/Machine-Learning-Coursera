Notes taken from edX: Introduction to Spark from BerkeleyX
Chris Zeng
Jun, 2015

###Objective
* Programming Spark
* Resilient Distributed Datasets (RDDs)
* Creating an RDD
* Spark Transformation and Actions
* Spark Programming Model

###PySpark
* Python programming interface for Spark.
* To make PySpark work, RDD is the key concept.

####Spark Driver and Workers
A Spark programmer consists of two programmer: driver and worker

* driver programme: runs on driver machine
* worker programme: runs on cluster or in local threads
* RDD are distributed across workers

#####Essential 1: Spark Context

The first thing a program does is to create a **SparkContext** object. This tells Spark how and where to access a cluster. pySpark shell and Databricks Cloud automatically create the **sc** variable for you.
(In iPython and other programs, users have to use a constructor to create a new **SparkContext**)

#####Essential 2: Master Parameter

Master parameter decides which type of clusters to use. Two types of master parameter: 
1. local clusters
2. remote clusters

For local clusters (threads), users can run Spark with (1) one thread, or (2) K threads (K, ideally, set to the number of cores)

For remote clusters, users can either connect to Spark cluster(``?``) or Apache Mesos(``?``) cluster.

#####Essential 3: Spark Transformation

* Create new datasets from an existing one
* Use Lazy evaluation, which means the resulted are not computed right away
* Think it as a recipe for creating result

#####Transformation functions
``map`` ``filter`` ``distinct`` ``flatmap (return a sequence)``

####What is RDD?

####Cache()
Cache function can avoid recompute/reread the file again.


####Spark Program Lifecycle
1. **Create RDDs** from external data or parallelize a collection in your driver program
2. Lazily **transform** them into new RDD (using transformation functions; transformations are recipe for results)
3. **Cache** some RDDs for reuse (using ```cache()``` function)
4. Perform **actions** to execute parallel computation and produce results (actions are the executions unlike transformation)

####Key-Value Transformation
1. ``returnByKey(func)`` return a new distributed dataset of (K,V), which Vs belong to the same K aggregated (sum, product, or whatever operation specified.)
2. ``sortByKey`` return a new dataset (K,V) pairs sorted by key K in ascending order
3. ``

e.g., 

**reduceByKey**
``>>> rdd = sc.parallelize([(1,2),(3,4),(3,6),(4,1),(4,4),(4,2)])``

``>>> rdd.reduceByKey(lambda a, b: a + b)``

RDD: [(1,2),(3,4),(3,6),(4,1),(4,4),(4,2)] -> [(1,2),(3,10),(4,7)]

**sortByKey**
``>>> rdd2 = sc.parallelize([(1,'a'),(2,'c'),(1,'b')])``

``>>> rdd2 = sortByKey()``

RDD: [(1,'a'),(2,'c'),(1,'b')] -> [(1,'a'),(1,'b'),(2,'c')]


**groupByKey**
``>>> rdd2 = sc.parallelize([(1,'a'),(2,'c'),(1,'b')])``

``>>> rdd2 = groupByKey()``

RDD: [(1,'a'),(2,'c'),(1,' b')] -> [(1,['a','b']),(2,['c'])]

The first tuple consists of the key 1 and the **iterables** a and b.
Be careful of groupByKey as it can create a lot of data movement and create LARGE iterables at workers

####pySpark Closure
#####What is a closure anyway?
In computer science, a  [closure](http://simple.wikipedia.org/wiki/Closure_(computer_science)) is a function that has an environment of its own. Inside this environment, there is at least one bound variable. Closures were first used in programming languages such as ML and Lisp.

Spark automatically creates closures for 

* functions run on RDDs at the workers, and
* any global variables used by those workers

#####One closure per worker
* no communication between workers

####What if?
* iterative or single jobs with large variables
* count events that occur during job execution (get updates from global variables)

**Problem**
* inefficient to send large data to each worker
* closure is one way: driver -> worker

####Solutions to what ifs: Broadcast and Accumulator Variables
* Broadcast Variables
Efficiently send large data to workers (all of the nodes)

* Accumulators 
Aggregate values from workers to driver. Only the driver can access the values from the accummulator. 
**Accumulators can only be written by workers and read by the driver program.**


####Accumulators
can be used in each actions or transformations.

* Actions: each task's update to accumulator is applied __only once__
* Transformations: __no guarantees__ (if fails or delay happen, can run multiple times). Should only be used for debugging purposes.

####References
[Spark Documentation](https://spark.apache.org/documentation.html)