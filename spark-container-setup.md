
### 1. ecr 생성 ###

us-east-1 리전에 퍼블릭 레포지토리를 만든다. 프라이비트 레포지토리와 달리 퍼블릭의 경우 us-east-1 리전에만 생성이 가능하다.
```
$ aws ecr-public create-repository \
     --repository-name spark-scala-container \
     --region us-east-1     
{
    "repository": {
        "repositoryArn": "arn:aws:ecr-public::xxxxxxxxxxxx:repository/spark-scala-container",
        "registryId": "xxxxxxxxxxxx",
        "repositoryName": "spark-scala-container",
        "repositoryUri": "public.ecr.aws/o5l1c9o9/spark-scala-container",
        "createdAt": "2023-02-03T13:44:39.344000+09:00"
    },
    "catalogData": {}
}     
```

### 2. Dockerfile 생성 ###

아래의 명령어를 사용하여 spark-container 도커 이미지를 생성한다. 
```
$ mkdir spark-container
$ cd spark-container
$ vi Dockerfile
```
[Dockerfile]
```
FROM public.ecr.aws/amazonlinux/amazonlinux:latest

# Install dependencies
RUN yum update -y && \
 yum install -y httpd

# Install apache and write hello world message
RUN echo 'Hello World!' > /var/www/html/index.html

# Configure apache
RUN echo 'mkdir -p /var/run/httpd' >> /root/run_apache.sh && \
 echo 'mkdir -p /var/lock/httpd' >> /root/run_apache.sh && \
 echo '/usr/sbin/httpd -D FOREGROUND' >> /root/run_apache.sh && \
 chmod 755 /root/run_apache.sh

EXPOSE 80

CMD /root/run_apache.sh
```

```
$ docker build -t spark-container .
$ docker images --filter reference=spark-container
REATED         SIZE
spark-container   latest    2673042eb6e8   2 minutes ago   524MB
```






## 참고자료 ##
* https://docs.aws.amazon.com/AmazonECR/latest/public/getting-started-cli.html
*  https://spark.apache.org/docs/latest/running-on-kubernetes.html
*  https://spark.apache.org/docs/latest/configuration.html
