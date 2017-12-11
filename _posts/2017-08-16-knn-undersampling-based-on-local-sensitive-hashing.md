---
title: KNN Undersampling Based on Local Sensitive Hashing
date: 2017-08-16 10:49:31
---

This post is mainly talking about big data sampling process by using Approximate Nearest Neighbor(ANN) in Spark.

### Data preprocessing
The following picture is the whole process of data sampling, speciallly, the biggest rectangle shows data preprocessing part.
1. "delete null row with null rate greater than 10%": since raw data is dirty with lots of null or missing value. For those columns that have null rates greater than 10%, we remove them. After that, we need to remove the rows that still contain null values.
2. "delete row with negative delayed time": It is illogical to have a delayed time less than zero, therefore we need to delete row with negative delayed time((UNIX) signin_tm - consigned_tm).
3. "cargo & limit type simplified mapping": Just for simplification.
4. "fill all null features with OTHER or -1.0": In case there are null values remaining, fill in those exceptions with OTHER(categorical feature) or -1.0(numerical feature).
5. "one-hot encoding":
6. "combine all transformed features" & "index label": these two steps are required in Spark MLlib for later usage.
![](/images/knn_undersampling_based_on_local_sensitive_hashing/data_flow.png)

### KNN undersampling
[KNN sampling](https://github.com/karlhigley/spark-neighbors) can sample data based on the relationship among them, since each data point has relative distance to others, the distributionn of data might obey specific model. If we use random sampling, it will sample data without considering data relationship.(LSH: Local Sensitive Hashing)

### Appendix: Spark API usage in Scala

```scala
//1.create a sparksession
val spark = SparkSession.builder().master("local[4]").getOrCreate()

//2. schema construction
val schema = StructType("collection type: Seq(StructField(),...) / Array(StructField(),...)")

//3. read csv
val data: DataFrame = spark.read.option("header","true").option("seq","\0001").schema(schema).csv(args(0))
data.printSchema()

//4. get column
val col = data.columns

//5. repartition & cache
data.repartition()
data.cache()

//6. add a column
data.withColumn("column name","column expression")

//7. groupby & agg (only "id" column & "column name" column will be selected.)
var id = data.groupBy("id").agg(sum("column name")).alias("ID")

//8. join
def join(right : DataFrame, usingColumns : Seq[String], joinType : String) : DataFrame 
def join(right : DataFrame, joinExprs : Column, joinType : String) : DataFrame 

//9. drop & union 
var sampledNegs = negativesAndT.filter(negativesAndT("t") < t).drop("t")
var data = sampledNegs.union(positives)
```











































