### 1. 로컬 PC 에 스파크 설치 ###

https://spark.apache.org/downloads.html 으로 방문하여 PC 의 home 디렉토리에 스파크 바이너리를 다운로드 하여 설치한다. 


```
$ 
```





### 2. Dockerfile 생성 ###

spark 전용 컨테이너를 만들기 위해서 Dockerfile 를 만든다. 
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
아래의 명령어를 사용하여 spark-container 도커 이미지를 생성하고, 동작여부를 테스트 한다.
```
$ docker build -t spark-container .
$ docker images --filter reference=spark-container
REATED         SIZE
spark-container   latest    2673042eb6e8   2 minutes ago   524MB

$ docker run -t -i -p 8080:80 spark-container
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
```


### 3. ecr 생성 ###

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


### 4. ecr 에 이미지 등록 ###

docker tag 명령어 수행시, ecr 생성시 출력된 repositoryUri 값을 taget_image 파리미터로 설정한다.
```
$ aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws

Login Succeeded
$ docker images
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
spark-container   latest    2673042eb6e8   23 minutes ago   524MB

$ docker tag spark-container:latest public.ecr.aws/o5l1c9o9/spark-scala-container

$ docker push public.ecr.aws/o5l1c9o9/spark-scala-container
Using default tag: latest
The push refers to repository [public.ecr.aws/o5l1c9o9/spark-scala-container]
c517369d99ff: Pushed
d67656653a16: Pushed
186b247cb7db: Pushed
08d6245c5886: Pushed
latest: digest: sha256:ace0211d11a6b9e428336cd5ce5fe8cd4c3464e8b1ba84a6d0393f7928fed8bd size: 1156

$ docker images
REPOSITORY                                      TAG       IMAGE ID       CREATED          SIZE
spark-container                                 latest    2673042eb6e8   27 minutes ago   524MB
public.ecr.aws/o5l1c9o9/spark-scala-container   latest    2673042eb6e8   27 minutes ago   524MB
```

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/ecr-docker-image.png)


### [참고] 이미지 및 레포지토리 삭제 ###

* ecr 이미지 삭제 
```
aws ecr-public batch-delete-image \
      --repository-name spark-scala-container \
      --image-ids imageTag=latest \
      --region us-east-1
```
* ecr 레포지토리 삭제 
```
aws ecr-public delete-repository \
      --repository-name spark-scala-container \
      --force \
      --region us-east-1
```





## 참고자료 ##
* https://docs.aws.amazon.com/AmazonECR/latest/public/getting-started-cli.html
*  https://spark.apache.org/docs/latest/running-on-kubernetes.html
*  https://spark.apache.org/docs/latest/configuration.html
