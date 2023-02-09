* https://blog.banksalad.com/tech/spark-on-kubernetes/

***
spark conf 에 인증정보를 넣고 해야 겠다. ㅜㅜ

https://jhleeeme.github.io/read-aws-s3-data-on-spark/

****







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

$ scp -i ~/aws-kp.pem SparkySpark-assembly-0.1.0-SNAPSHOT.jar \
  ec2-user@ec2-3-38-202-76.ap-northeast-2.compute.amazonaws.com:
  
[ec2-user@ip-172-31-17-0 ~]$ $SPARK_HOME/bin/spark-submit --master "local[4]" SparkySpark-assembly-0.1.0-SNAPSHOT.jar  
```

---> 인스턴스 프로파일로 제대로 동작한다.


---> 그러면 K8S 관련 설정이 문제인 것으로 예상된다. *****



** ec2 eks role 검사


IAM Role
 eksctl-spark-on-eks-nodegroup-spa-NodeInstanceRole-FIYEKMFETV5I 
 
  eksctl-spark-on-eks-nodegroup-spa-NodeInstanceRole-FIYEKMFETV5I 
  
   eksctl-spark-on-eks-nodegroup-spa-NodeInstanceRole-1H8KFQM987K7X 
