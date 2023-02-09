* https://sungminoh.github.io/posts/development/use-s3-for-spark/

* https://github.com/skyer9/spark-with-ec2-s3/blob/master/get-file-from-s3-with-scala.md

ec2 에 spark 을 인스톨해서 403 에러가 발생하는지 테스트 해 본다..

```
% ssh -i aws-kp.pem ec2-user@ec2-3-38-202-76.ap-northeast-2.compute.amazonaws.com


$ cd; wget https://dlcdn.apache.org/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz

$ tar xvfz spark-3.3.1-bin-hadoop3.tgz 

$ cd spark-3.3.1-bin-hadoop3

$ sudo yum install java-1.8.0-openjdk

$ java -version
```


```
[ec2-user@ip-172-31-17-0 ~]$ aws s3 ls
Unable to locate credentials. You can configure credentials by running "aws configure".
[ec2-user@ip-172-31-17-0 ~]$
```

[SparkOnEKS-EC2Role]

// 그림1 (role)
// 그림2 (policy)


```
$ export SPARK_HOME=~/spark-3.3.1-bin-hadoop3

$ cd /Users/soonbeom/spark-on-eks/app

$ scp SparkySpark-assembly-0.1.0-SNAPSHOT.jar ec2-user@ec2-3-38-202-76.ap-northeast-2.compute.amazonaws.com:~

```
