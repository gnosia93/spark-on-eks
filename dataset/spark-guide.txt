3.5.0
Overview
Programming Guides
API Docs
Deploying
More
Search the docs
Apache Spark - A Unified engine for large-scale data analytics
Apache Spark is a unified analytics engine for large-scale data processing. It provides high-level APIs in Java, Scala, Python and R, and an optimized engine that supports general execution graphs. It also supports a rich set of higher-level tools including Spark SQL for SQL and structured data processing, pandas API on Spark for pandas workloads, MLlib for machine learning, GraphX for graph processing, and Structured Streaming for incremental computation and stream processing.
Downloading
Get Spark from the downloads page of the project website. This documentation is for Spark version 3.5.0. Spark uses Hadoop’s client libraries for HDFS and YARN. Downloads are pre-packaged for a handful of popular Hadoop versions. Users can also download a “Hadoop free” binary and run Spark with any Hadoop version by augmenting Spark’s classpath. Scala and Java users can include Spark in their projects using its Maven coordinates and Python users can install Spark from PyPI.

If you’d like to build Spark from source, visit Building Spark.

Spark runs on both Windows and UNIX-like systems (e.g. Linux, Mac OS), and it should run on any platform that runs a supported version of Java. This should include JVMs on x86_64 and ARM64. It’s easy to run locally on one machine — all you need is to have java installed on your system PATH, or the JAVA_HOME environment variable pointing to a Java installation.

Spark runs on Java 8/11/17, Scala 2.12/2.13, Python 3.8+, and R 3.5+. Java 8 prior to version 8u371 support is deprecated as of Spark 3.5.0. When using the Scala API, it is necessary for applications to use the same version of Scala that Spark was compiled for. For example, when using Scala 2.13, use Spark compiled for 2.13, and compile code/applications for Scala 2.13 as well.

For Java 11, setting -Dio.netty.tryReflectionSetAccessible=true is required for the Apache Arrow library. This prevents the java.lang.UnsupportedOperationException: sun.misc.Unsafe or java.nio.DirectByteBuffer.(long, int) not available error when Apache Arrow uses Netty internally.

Running the Examples and Shell
Spark comes with several sample programs. Python, Scala, Java, and R examples are in the examples/src/main directory.

To run Spark interactively in a Python interpreter, use bin/pyspark:

./bin/pyspark --master "local[2]"
Sample applications are provided in Python. For example:

./bin/spark-submit examples/src/main/python/pi.py 10
To run one of the Scala or Java sample programs, use bin/run-example <class> [params] in the top-level Spark directory. (Behind the scenes, this invokes the more general spark-submit script for launching applications). For example,

./bin/run-example SparkPi 10
You can also run Spark interactively through a modified version of the Scala shell. This is a great way to learn the framework.

./bin/spark-shell --master "local[2]"
The --master option specifies the master URL for a distributed cluster, or local to run locally with one thread, or local[N] to run locally with N threads. You should start by using local for testing. For a full list of options, run the Spark shell with the --help option.

Since version 1.4, Spark has provided an R API (only the DataFrame APIs are included). To run Spark interactively in an R interpreter, use bin/sparkR:

./bin/sparkR --master "local[2]"
Example applications are also provided in R. For example:

./bin/spark-submit examples/src/main/r/dataframe.R
Running Spark Client Applications Anywhere with Spark Connect
Spark Connect is a new client-server architecture introduced in Spark 3.4 that decouples Spark client applications and allows remote connectivity to Spark clusters. The separation between client and server allows Spark and its open ecosystem to be leveraged from anywhere, embedded in any application. In Spark 3.4, Spark Connect provides DataFrame API coverage for PySpark and DataFrame/Dataset API support in Scala.

To learn more about Spark Connect and how to use it, see Spark Connect Overview.

Launching on a Cluster
The Spark cluster mode overview explains the key concepts in running on a cluster. Spark can run both by itself, or over several existing cluster managers. It currently provides several options for deployment:

Standalone Deploy Mode: simplest way to deploy Spark on a private cluster
Apache Mesos (deprecated)
Hadoop YARN
Kubernetes
Where to Go from Here
Programming Guides:

Quick Start: a quick introduction to the Spark API; start here!
RDD Programming Guide: overview of Spark basics - RDDs (core but old API), accumulators, and broadcast variables
Spark SQL, Datasets, and DataFrames: processing structured data with relational queries (newer API than RDDs)
Structured Streaming: processing structured data streams with relation queries (using Datasets and DataFrames, newer API than DStreams)
Spark Streaming: processing data streams using DStreams (old API)
MLlib: applying machine learning algorithms
GraphX: processing graphs
SparkR: processing data with Spark in R
PySpark: processing data with Spark in Python
Spark SQL CLI: processing data with SQL on the command line
API Docs:

Spark Scala API (Scaladoc)
Spark Java API (Javadoc)
Spark Python API (Sphinx)
Spark R API (Roxygen2)
Spark SQL, Built-in Functions (MkDocs)
Deployment Guides:

Cluster Overview: overview of concepts and components when running on a cluster
Submitting Applications: packaging and deploying applications
Deployment modes:
Amazon EC2: scripts that let you launch a cluster on EC2 in about 5 minutes
Standalone Deploy Mode: launch a standalone cluster quickly without a third-party cluster manager
Mesos: deploy a private cluster using Apache Mesos
YARN: deploy Spark on top of Hadoop NextGen (YARN)
Kubernetes: deploy Spark on top of Kubernetes
Other Documents:

Configuration: customize Spark via its configuration system
Monitoring: track the behavior of your applications
Tuning Guide: best practices to optimize performance and memory use
Job Scheduling: scheduling resources across and within Spark applications
Security: Spark security support
Hardware Provisioning: recommendations for cluster hardware
Integration with other storage systems:
Cloud Infrastructures
OpenStack Swift
Migration Guide: Migration guides for Spark components
Building Spark: build Spark using the Maven system
Contributing to Spark
Third Party Projects: related third party Spark projects
External Resources:

Spark Homepage
Spark Community resources, including local meetups
StackOverflow tag apache-spark
Mailing Lists: ask questions about Spark here
AMP Camps: a series of training camps at UC Berkeley that featured talks and exercises about Spark, Spark Streaming, Mesos, and more. Videos, are available online for free.
Code Examples: more are also available in the examples subfolder of Spark (Scala, Java, Python, R)
