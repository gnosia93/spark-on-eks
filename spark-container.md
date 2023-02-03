### 1. 로컬 PC 에 스파크 설치 ###

https://spark.apache.org/downloads.html 으로 방문하여 로컬 PC 의 home 디렉토리에 스파크 바이너리를 설치한다. 

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/spark-download.png)
```
$ cd; wget https://dlcdn.apache.org/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz

$ tar xvfz spark-3.3.1-bin-hadoop3.tgz 

$ cd spark-3.3.1-bin-hadoop3
```


### 2. 이미지 빌드 ###
```
$ cp ./kubernetes/dockerfiles/spark/Dockerfile .

$ docker build -t spark-scala-container .
[+] Building 1.8s (18/18) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                       
 => => transferring dockerfile: 2.54kB                                                                                                                     
 => [internal] load .dockerignore                                                                                                                           
 => => transferring context: 2B                                                                                                                             
 => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                                     
 => [ 1/13] FROM docker.io/library/openjdk:11-jre-slim@sha256:93af7df2308c5141a751c4830e6b6c5717db102b3b31f012ea29d842dc4f2b02                             
 => [internal] load build context                                                                                                                           
 => => transferring context: 147.95kB                                                                                                                       
 => CACHED [ 2/13] RUN set -ex &&     sed -i 's/http:\/\/deb.\(.*\)/https:\/\/deb.\1/g' /etc/apt/sources.list &&     apt-get update &&     ln -s /lib 
 => CACHED [ 3/13] COPY jars /opt/spark/jars                                                                                                               
 => CACHED [ 4/13] COPY bin /opt/spark/bin                                                                                                                 
 => CACHED [ 5/13] COPY sbin /opt/spark/sbin                                                                                                               
 => CACHED [ 6/13] COPY kubernetes/dockerfiles/spark/entrypoint.sh /opt/                                                                                   
 => CACHED [ 7/13] COPY kubernetes/dockerfiles/spark/decom.sh /opt/                                                                                         
 => CACHED [ 8/13] COPY examples /opt/spark/examples                                                                                                       
 => CACHED [ 9/13] COPY kubernetes/tests /opt/spark/tests                                                                                                   
 => CACHED [10/13] COPY data /opt/spark/data                                                                                                               
 => CACHED [11/13] WORKDIR /opt/spark/work-dir                                                                                                             
 => CACHED [12/13] RUN chmod g+w /opt/spark/work-dir                                                                                                       
 => CACHED [13/13] RUN chmod a+x /opt/decom.sh                                                                                                             
 => exporting to image                                                                                                                                     
 => => exporting layers                                                                                                                                     
 => => writing image sha256:7fc25f409862e783414a4d1554e0e313a63a71f199fec003499570ca9ab56927                                                               
 => => naming to docker.io/library/spark-scala-container  
 
$ docker images
REPOSITORY                                      TAG       IMAGE ID       CREATED             SIZE
spark-scala-container                           latest    7fc25f409862   20 minutes ago      601MB
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


### 4. ecr 로그인 ###

docker login 명령어를 사용하여 AWS public erc 레포지토리에 로그인 한다. 패스워드는 아래와 같이 aws CLI 명령어로 가져온다.
```
$ aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws

Login Succeeded
```

### 5. ecr 에 이미지 등록 ###

docker tag 를 사용하여 이미지 태깅시 TARGET_IMAGE 파리미터는 위에서 생성된 ecr의 epositoryUri 를 사용한다.   
```
$ docker tag spark-scala-container:latest public.ecr.aws/o5l1c9o9/spark-scala-container

$ docker images
REPOSITORY                                      TAG       IMAGE ID       CREATED             SIZE
spark-scala-container                           latest    7fc25f409862   About an hour ago   601MB
public.ecr.aws/o5l1c9o9/spark-scala-container   latest    7fc25f409862   About an hour ago   601MB

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
