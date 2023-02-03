


### 1. spark 다운로드 ###




### 2. 이미지 빌드 ###
```
$ pwd
/Users/soonbeom/spark-container/spark-3.3.1-bin-hadoop3

$ cp ./kubernetes/dockerfiles/spark/Dockerfile .

$ docker build -t scala-container .
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
 => => naming to docker.io/library/scala-container  
 
$ docker images
REPOSITORY                                      TAG       IMAGE ID       CREATED             SIZE
scala-container                                 latest    7fc25f409862   20 minutes ago      601MB

```

### 3. 이미지 푸시 ###

```

$ docker push public.ecr.aws/o5l1c9o9/spark-scala-container:my-tag
```
