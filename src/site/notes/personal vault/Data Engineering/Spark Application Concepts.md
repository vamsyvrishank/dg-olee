---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/spark-application-concepts/"}
---


Application -A user program built on Spark using its APIs. It consists of a driver program and executors on the cluster.

SparkSession -An object that provides a point of entry to interact with underlying Spark functionality and allows programming Spark with its APIs. In an interactive Spark shell, the Spark driver instantiates a SparkSession for you, while in a Spark application, you create a SparkSession object yourself.

Job - A parallel computation consisting of multiple tasks that gets spawned in response to a Spark action (e.g., save(), collect()).

Stage - Each job gets divided into smaller sets of tasks called stages that depend on each other.

Task -A single unit of work or execution that will be sent to a Spark executor.


## Spark Jobs

During interactive sessions with Spark shells, the driver converts your Spark application into one or more Spark jobs (Figure 2-3). It then transforms each job into a DAG. This, in essence, is Spark’s execution plan, where each node within a job
could be a single or multiple Spark stages.

![[Pasted image 20240610143330.png \| 800]]

## Spark Stages

As part of the DAG nodes, stages are created based on what operations can be performed serially or in parallel (Figure 2-4). Not all Spark operations can happen in a single stage, so they may be divided into multiple stages. Often stages are delineated on the operator’s computation boundaries, where they dictate data transfer among Spark executors.

![[Pasted image 20240610143511.png \| 800]]


## Spark Tasks

Each stage is comprised of Spark tasks (a unit of execution), which are then federated across each Spark executor; each task maps to a single core and works on a single partition of data (Figure 2-5). As such, an executor with 16 cores can have 16 or more tasks working on 16 or more partitions in parallel, making the execution of Spark’s tasks exceedingly parallel!

![[Pasted image 20240610143710.png \| 800]]]



## Transformations, Actions, and Lazy Evaluation

Spark operations on distributed data can be classified into two types: transformations and actions.

Transformations, as the name suggests, transform a Spark DataFrame into a new DataFrame without altering the original data, giving it the property of immutability. Put another way, an operation such as select() or filter() will not change the original DataFrame; instead, it will return the transformed results of the operation as a new DataFrame.

<mark class="hltr-green">All transformations are evaluated lazily.</mark>

That is, their results are not computed immediately, but they are recorded or remembered as a lineage.

A recorded lineage allows Spark, at a later time in its execution plan, to rearrange certain transformations, coalesce them, or optimize transformations into stages for more efficient execution. Lazy evaluation is Spark’s strategy for delaying execution until an action is invoked or data is “touched” (read from or written to disk).

![[Pasted image 20240610144052.png \| 900]]

The actions invoke all the stages of transformations stored in a lineage

![[Pasted image 20240610144122.png \| 600]]
Read is also transformation.


The query plan is created until an action is invoked and Nothing is executed until then.


## Narrow and Wide Transformation


Any transformation where a single output partition can be computed from a single input partition is a narrow transformation.

For example: 
filter() and contains() represent narrow transformations because they can operate on a single partition and produce the resulting output partition without any exchange of data.

groupBy() or orderBy() instruct Spark to perform <mark class="hltr-green">wide transformations</mark>, where data from other partitions is read in, combined, and written to disk. Since each partition will have its own count of the word that contains the “Spark” word in its row of data, a count (groupBy()) will force a shuffle of data from each of the executor’s partitions across the cluster.

![[Pasted image 20240610144622.png \| 800]]
