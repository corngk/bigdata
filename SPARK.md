# Apache Spark
Apache Spark is a general-purpose data processing tool called a data processing engine. Used by data engineers and data scientists to perform extremely fast data queries on large amounts of data in the terabyte range. It is a framework for cluster-based calculations that competes with the classic Hadoop Map / Reduce by using the RAM available in the cluster for faster execution of jobs.

In addition, Spark also offers the option of controlling the data via SQL, processing it by streaming in (near) real-time, and provides its own graph database and a machine learning library.  The framework offers in-memory technologies for this purpose, i.e. it can store queries and data directly in the main memory of the cluster nodes.

Apache Spark is ideal for processing large amounts of data quickly. Spark’s programming model is based on Resilient Distributed Datasets (RDD), a collection class that operates distributed in a cluster. This open-source platform supports a variety of programming languages such as Java, Scala, Python, and R.

## Install Apache Spark on Windows 10 WDSL 2 Ubuntu 20.4

## Download Spark
    wget https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz

## Extract Spark
I use different drive for all Big Data resources at e drive on Windows 10. From Ubuntu it is */mnt/e*. You can use your own drive or use */opt* folder.

    sudo mkdir /opt/spark

    sudo tar -xf spark*.tgz -C /opt/spark --strip-component 1

Change permission of the folder, so Spark can write inside it.

    sudo chmod -R 777 /opt/spark


Add Spark to the system path

We need to configure environment variable for Spark.

    nano ~/.bashrc

Add the following code in the *~/.bashrc* file. Note that there is settings for Java and Hadoop as well in the file.

    # java setting
    export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"

    # Spark
    export SPARK_HOME="/mnt/e/Spark/spark-3.1.2-bin-hadoop3.2"

    # Hadoop
    export HADOOP_HOME="/mnt/e/Hadoop/hadoop-3.3.1"

    # Set PATH
    export PATH=$SPARK_HOME/bin:$SPARK_HOME/sbin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH

## Start Apache Spark master server on Ubuntu

Now let's start Spark as standalone master server.

    start-master.sh

If you want to use a custom port then that is possible to use, options or arguments given below.
 
**–port**  – Port for service to listen on (default: 7077 for master, random for worker)
**–webui-port**  – Port for web UI (default: 8080 for master, 8081 for worker)

If you want to run Spark web UI on 8082, and make it listen to port 7072 then the command to start it will be like this:

    start-master.sh --port 7072 --webui-port 8082

Access Spark Master Web Interface

From your browser open `http://127.0.0.1:8080`

## Run Slave Worker Script

Run the slave worker:

    start-worker.sh spark://127.0.0.1:7707

Change the IP address and port number with yours. You will see from the Web that the workder is run.

You can change amount of memory you would like to allocate to the worker.

    stop-worker.sh

    start-worker.sh -m 256M spark://127.0.0.1:7707

Use Spark-Shell

To start working with Spark Shell:

    spark-shell

    Spark context Web UI available at http://172.30.134.125:4040
    Spark context available as 'sc' (master = local[*], app id = local-1629886689843).
    Spark session available as 'spark'.
    Welcome to
        ____              __
        / __/__  ___ _____/ /__
        _\ \/ _ \/ _ `/ __/  '_/
    /___/ .__/\_,_/_/ /_/\_\   version 3.1.2
        /_/

    Using Scala version 2.12.10 (OpenJDK 64-Bit Server VM, Java 1.8.0_292)
    Type in expressions to have them evaluated.
    Type :help for more information.

Get some basic shell commands

    scala> :help
    All commands can be abbreviated, e.g., :he instead of :help.
    :completions <string>    output completions for the given string
    :edit <id>|<line>        edit history
    :help [command]          print this summary or command-specific help
    :history [num]           show the history (optional num is commands to show)
    :h? <string>             search the history
    :imports [name name ...] show import history, identifying sources of names
    :implicits [-v]          show the implicits in scope
    :javap <path|class>      disassemble a file or class name
    :line <id>|<line>        place line(s) at the end of history
    :load <path>             interpret lines in a file
    :paste [-raw] [path]     enter paste mode or paste a file
    :power                   enable power user mode
    :quit                    exit the interpreter
    :replay [options]        reset the repl and replay all previous commands
    :require <path>          add a jar to the classpath
    :reset [options]         reset the repl to its initial state, forgetting all session entries
    :save <path>             save replayable session to a file
    :sh <command line>       run a shell command (result is implicitly => List[String])
    :settings <options>      update compiler options, if possible; see reset
    :silent                  disable/enable automatic printing of results
    :type [-v] <expr>        display the type of an expression without evaluating it
    :kind [-v] <type>        display the kind of a type. see also :help kind
    :warnings                show the suppressed warnings from the most recent line which had any

## Start Stop Commands

To stop Spark, type the following commands:

    stop-master.sh

    stop-worker.sh

To stop all at once

    stop-all.sh

To start all at once

    start-all.sh

## Some Example

Let's now try to code something

    scala> sc.parallelize(List(1,2,3)).flatMap(x=>List(x,x,x)).collect
    res1: Array[Int] = Array(1, 1, 1, 2, 2, 2, 3, 3, 3)

    scala> sc.parallelize(List(1,2,3)).map(x=>List(x,x,x)).collect
    res2: Array[List[Int]] = Array(List(1, 1, 1), List(2, 2, 2), List(3, 3, 3))

Let's Do Some Other Examples

    scala> val parallel = sc.parallelize(1 to 9, 3)
    parallel: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[4] at parallelize at <console>:24

    scala> parallel.mapPartitions( x => List(x.next).iterator).collect
    res3: Array[Int] = Array(1, 4, 7)

    scala> val parallel = sc.parallelize(1 to 9)
    parallel: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[6] at parallelize at <console>:24

    scala> parallel.mapPartitionsWithIndex((index: Int, it: Iterator[Int]) => it.toList.map(x => index + ", "+x).iterator).collect
    res4: Array[String] = Array(1, 1, 2, 2, 3, 3, 5, 4, 6, 5, 7, 6, 9, 7, 10, 8, 11, 9)

    scala> val parallel = sc.parallelize(1 to 9)
    parallel: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[8] at parallelize at <console>:24

    scala> parallel.sample(true, .2).count
    res5: Long = 2

    scala> parallel.sample(true, .2).count
    res6: Long = 1

    scala> parallel.sample(true, .1)
    res9: org.apache.spark.rdd.RDD[Int] = PartitionwiseSampledRDD[13] at sample at <console>:26

    scala> val par2 = sc.parallelize(5 to 15)
    par2: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[14] at parallelize at <console>:24

    scala> parallel.union(par2).collect
    res11: Array[Int] = Array(1, 2, 3, 4, 5, 6, 7, 8, 9, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15)

    scala> parallel.intersection(par2).collect
    res12: Array[Int] = Array(5, 6, 7, 8, 9)

## Reduce, Count, First, Take, and TakeSample Functions

    scala> val names1 = sc.parallelize(List("abe", "abby", "apple"))
    names1: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[0] at parallelize at <console>:24

    scala> names1.reduce((t1, t2) => t1 + t2)
    res0: String = abbyappleabe

    scala> names1.flatMap(k => List(k.size)).reduce((t1, t2) => t1 + t2)
    res2: Int = 12

    scala> val names2 = sc.parallelize(List("apple", "beatty", "beatrice")).map(a => (a, a.size))
    names2: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[3] at map at <console>:24

    scala> names2.flatMap(t => Array(t. _2)).reduce(_ + _)
    res3: Int = 19

    scala> sc.parallelize(List(1, 2, 3)).flatMap(x => List(x, x, x)).collect
    res4: Array[Int] = Array(1, 1, 1, 2, 2, 2, 3, 3, 3)

    scala> sc.parallelize(List(1, 2, 3)).map(x => List(x, x, x)).collect
    res5: Array[List[Int]] = Array(List(1, 1, 1), List(2, 2, 2), List(3, 3, 3))

    scala> val names2 = sc.parallelize(List("apple", "beatty", "beatrice"))
    names2: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[9] at parallelize at <console>:24

    scala> names2.count
    res6: Long = 3

    scala> names2.first
    res7: String = apple

    scala> names2.take(2)
    res8: Array[String] = Array(apple, beatty)

    scala> val teams = sc.parallelize(List("twins", "brewers", "cubs", "white sox", "indians", "bad news bears"))
    teams: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[10] at parallelize at <console>:24

    scala> teams.takeSample(true, 3)
    res9: Array[String] = Array(indians, bad news bears, indians)

    scala> teams.takeSample(true, 6)
    res10: Array[String] = Array(brewers, brewers, cubs, bad news bears, indians, indians)

    scala> teams.takeSample(true, 6)
    res11: Array[String] = Array(bad news bears, cubs, brewers, white sox, bad news bears, bad news bears)

    scala> val hockeyTeams = sc.parallelize(List("wild", "blackhowks", "red wings", "wild", "oilers", "whalers", "jets", "wild"))
    hockeyTeams: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[15] at parallelize at <console>:24

    scala> hockeyTeams.map(k => (k, 1)).countByKey
    res13: scala.collection.Map[String,Long] = Map(wild -> 3, oilers -> 1, jets -> 1, red wings -> 1, blackhowks -> 1, whalers -> 1)

That's all folks. Happy coding!