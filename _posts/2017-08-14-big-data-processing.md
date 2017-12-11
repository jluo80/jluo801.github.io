---
title: Big Data Processing
date: 2017-08-14 20:49:31
---

In the past month, I've been learning a lot new technics and implementing them in the real working environment, as an AI developing engineer, although I am currently working on order claim prediction in order to help me get involved with the team and working environment. 

#### Hadoop(Linear scalability): MapReduce & HDFS
1. Use master-worker architecture
2. Computation model: MapReduce
	- Map: master divide dta; worker works parallel
	- Shuffle: sorts and moves automatically
	- Reduce: worker combines results parallel
3. Hadoop File System

#### Pig: write program easier

#### Hive:SQL query

#### Spark
Apache Spark a "great" big data processing tool which I uses a lot right now. Although Apache Spark provides lots of API to enrich the function of Spark, the learning curve is pretty steep. There is not too much documents or textbooks to systematically follow.

1. RDD: the basic abstract data structure to store data in Spark.
2. DataFrame: a much easy-to-use data structure. RDD & DataFrame can be converted to each other.
3. Scala: Spark has API for different languages like Java, Python etc, but Scala is the one that I preferred. Scala is working on JVM. (This reminds me of the whole installation of Spark environment, I might write a second post about it later.)

#### Working process
1. Configure related dependencies (build.sbt)
   ```
   name := "antifraud_sf_2_test_easy_ensemble"

   version := "1.0"

   scalaVersion := "2.11.8"

   libraryDependencies ++= Seq(
	......
   )

   // name of jar file
   assemblyJarName in assembly := "knn_sampling.jar"
   
   // main function that will be running
   mainClass in assembly := Some("KnnUndUnderSample")
   ```
2. Code
3. Use sbt to build a jar file and run on the clusters.
    ```
    sbt compile
    sbt assembly
    ```
4. FTP & Hadoop shell script
   ```
   hadoop fs -ls 
   hadoop fs -cp
   hadoop fs -cat
   hadoop fs -rm -r
   ```
5. spark-submit
   ```
   spark-submit --queue root.mlalg --master yarn --deploy-mode cluster --executor-memory 50G  --driver-memory 14G --executor-cores 8 --num-executors 40 --conf spark.default.parallelism=500 --conf spark.storage.memoryFraction=0.5 --conf spark.shuffle.memoryFraction=0.3 knn_sampling.jar "data_src" "data_dest"
   ```

#### Elastic Search: Restful API & json & distibuted system
shard is the minimum worker unit, it is a complete engine.
[Elastic Search Guide](https://es.xiaoleilu.com/)
