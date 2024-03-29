---
title: Spark Launching
date: 2017-10-31 10:49:31
---

除了在Mesos和YARN集群管理器上运行，Spark也提供了简单的Standalone部署模式。我们可以手动创建master和workers节点，或者利用Spark提供的脚本去运行。

### Installing Spark Standalone to a Cluster
（1）手动形式启动cluster
在master server上执行以下语句。一旦执行后，master会打印出一个URL spark://HOST:PORT，然后我们就可以用这个URL去链接别的workers，或者在源码中把这个URL当做“master”传入给SparkContext。
```sh
$ ./sbin/start-master.sh
```
同理，创建workers可以通过以下语句。
```sh
./sbin/start-slave.sh <master-spark-URL>
```
通过master的web UI(http://localhost:8080 by default)，你就可以看到刚创建的worker nodes。

（2）利用Spark提供scripts启动cluster
要启动Spark集群，我们需要在Spark目录下创建一个名为conf/slaves的文件，里面需要包含了所有Spark workers的hostname。注意，master链接workers是通过ssh进行的。为此，我们需要password-less设置，也就是生成一个private key。

通过以下scripts，我们可以启动或停止master或workers。
- sbin/start-master.sh - Starts a master instance on the machine the script is executed on.
- sbin/start-slaves.sh - Starts a slave instance on each machine specified in the conf/slaves file.
- sbin/start-slave.sh - Starts a slave instance on the machine the script is executed on.
- sbin/start-all.sh - Starts both a master and a number of slaves as described above.
- sbin/stop-master.sh - Stops the master that was started via the bin/start-master.sh script.
- sbin/stop-slaves.sh - Stops all slave instances on the machines specified in the conf/slaves file.
- sbin/stop-all.sh - Stops both the master and the slaves as described above.

### Launching Spark Applications
Spark-submit script提供了最直接的方式去运行一个编译好的Spark应用。
```sh
./bin/spark-submit \
  --class <main-class> \
  --master <master-url> \
  --deploy-mode <deploy-mode> \
  --conf <key>=<value> \
  ... # other options
  <application-jar> \
  [application-arguments]
```
一些常用选项：
  -  --class: The entry point for your application (e.g. org.apache.spark.examples.SparkPi)
  -  --master: The master URL for the cluster (e.g. spark://23.195.26.187:7077)
  -  --deploy-mode: Whether to deploy your driver on the worker nodes (cluster) or locally as an external client (client) (default: client) †
  -  --conf: Arbitrary Spark configuration property in key=value format. For values that contain spaces wrap “key=value” in quotes (as shown).
  -  application-jar: Path to a bundled jar including your application and all dependencies. The URL must be globally visible inside of your cluster, for instance, an hdfs:// path or a file:// path that is present on all nodes.
   - application-arguments: Arguments passed to the main method of your main class, if any

```sh
# Run application locally on 8 cores
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master local[8] \
  /path/to/examples.jar \
  100

# Run on a Spark standalone cluster in client deploy mode
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master spark://207.184.161.138:7077 \
  --executor-memory 20G \
  --total-executor-cores 100 \
  /path/to/examples.jar \
  1000

# Run on a Spark standalone cluster in cluster deploy mode with supervise
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master spark://207.184.161.138:7077 \
  --deploy-mode cluster \
  --supervise \
  --executor-memory 20G \
  --total-executor-cores 100 \
  /path/to/examples.jar \
  1000

# Run on a YARN cluster
export HADOOP_CONF_DIR=XXX
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master yarn \
  --deploy-mode cluster \  # can be client for client mode
  --executor-memory 20G \
  --num-executors 50 \
  /path/to/examples.jar \
  1000

# Run a Python application on a Spark standalone cluster
./bin/spark-submit \
  --master spark://207.184.161.138:7077 \
  examples/src/main/python/pi.py \
  1000

# Run on a Mesos cluster in cluster deploy mode with supervise
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master mesos://207.184.161.138:7077 \
  --deploy-mode cluster \
  --supervise \
  --executor-memory 20G \
  --total-executor-cores 100 \
  http://path/to/examples.jar \
  1000
```

在client模式下，driver在master节点上启动，master和其他workers在同一个网段。在cluser模式下，driver是在其中一个workers中启动，当client进程完成了提交任务的责任后便会退出，并不需要等应用完成，master和其他workers可以在不同网段。通过spark-submit启动应用，应用的jar包会被自动分发到所有workers上。对于其他需要用到的额外jar包，可以通过--jars jar1, jar2来指定。

![](/images/spark_launching/client_mode.png)
![](/images/spark_launching/cluster_mode.png)

### Master URL
![](/images/spark_launching/master_url.png)
