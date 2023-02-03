


### 1. spark 다운로드 ###




### 2. 이미지 빌드 ###
```
$ pwd
/Users/soonbeom/spark-container/spark-3.3.1-bin-hadoop3

$ ./bin/docker-image-tool.sh -r public.ecr.aws/o5l1c9o9/spark-scala-container -t my-tag build
[+] Building 16.0s (18/18) FINISHED
 => [internal] load build definition from Dockerfile                                                                                       0.0s
 => => transferring dockerfile: 2.54kB                                                                                                     0.0s
 => [internal] load .dockerignore                                                                                                          0.0s
 => => transferring context: 2B                                                                                                            0.0s
 => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                     2.7s
 => [ 1/13] FROM docker.io/library/openjdk:11-jre-slim@sha256:93af7df2308c5141a751c4830e6b6c5717db102b3b31f012ea29d842dc4f2b02             2.6s
 => => resolve docker.io/library/openjdk:11-jre-slim@sha256:93af7df2308c5141a751c4830e6b6c5717db102b3b31f012ea29d842dc4f2b02               0.0s
 => => sha256:fd343789f78f94e3d0fa4df3267fdadbc18138ad5785aa9a8afec60ec0407136 212B / 212B                                                 0.5s
 => => sha256:93af7df2308c5141a751c4830e6b6c5717db102b3b31f012ea29d842dc4f2b02 549B / 549B                                                 0.0s
 => => sha256:4e762c7129be5d5a3d04c51e6e15f9dc8168506775055c8ce5ed023f7688fe2b 1.16kB / 1.16kB                                             0.0s
 => => sha256:a1aef51062387fc731472d3257e9086f72497dcb7a3f94c2e6487708c51584ad 7.57kB / 7.57kB                                             0.0s
 => => sha256:a9fe95647e78b5516c7e2327355b6996e2ea295cd76ae242cbfe87f016b4e760 30.05MB / 30.05MB                                           0.7s
 => => sha256:4015b6e8cc8db11af172df5c0369552bc2a74cd69094b0756d21fc6b0b2a5393 1.57MB / 1.57MB                                             0.7s
 => => sha256:5e8df94248cfc56b40d896855ef271976224c34fbfbe8a456006be727a3635c0 45.13MB / 45.13MB                                           1.5s
 => => extracting sha256:a9fe95647e78b5516c7e2327355b6996e2ea295cd76ae242cbfe87f016b4e760                                                  0.8s
 => => extracting sha256:4015b6e8cc8db11af172df5c0369552bc2a74cd69094b0756d21fc6b0b2a5393                                                  0.1s
 => => extracting sha256:fd343789f78f94e3d0fa4df3267fdadbc18138ad5785aa9a8afec60ec0407136                                                  0.0s
 => => extracting sha256:5e8df94248cfc56b40d896855ef271976224c34fbfbe8a456006be727a3635c0                                                  0.7s
 => [internal] load build context                                                                                                          2.3s
 => => transferring context: 305.86MB                                                                                                      2.3s
 => [ 2/13] RUN set -ex &&     sed -i 's/http:\/\/deb.\(.*\)/https:\/\/deb.\1/g' /etc/apt/sources.list &&     apt-get update &&     ln -s  9.5s
 => [ 3/13] COPY jars /opt/spark/jars                                                                                                      0.3s
 => [ 4/13] COPY bin /opt/spark/bin                                                                                                        0.0s
 => [ 5/13] COPY sbin /opt/spark/sbin                                                                                                      0.0s
 => [ 6/13] COPY kubernetes/dockerfiles/spark/entrypoint.sh /opt/                                                                          0.0s
 => [ 7/13] COPY kubernetes/dockerfiles/spark/decom.sh /opt/                                                                               0.0s
 => [ 8/13] COPY examples /opt/spark/examples                                                                                              0.0s
 => [ 9/13] COPY kubernetes/tests /opt/spark/tests                                                                                         0.0s
 => [10/13] COPY data /opt/spark/data                                                                                                      0.0s
 => [11/13] WORKDIR /opt/spark/work-dir                                                                                                    0.0s
 => [12/13] RUN chmod g+w /opt/spark/work-dir                                                                                              0.1s
 => [13/13] RUN chmod a+x /opt/decom.sh                                                                                                    0.1s
 => exporting to image                                                                                                                     0.5s
 => => exporting layers                                                                                                                    0.5s
 => => writing image sha256:7fc25f409862e783414a4d1554e0e313a63a71f199fec003499570ca9ab56927                                               0.0s
 => => naming to public.ecr.aws/o5l1c9o9/spark-scala-container/spark:my-tag                                                                0.0s

$ docker images
REPOSITORY                                            TAG       IMAGE ID       CREATED             SIZE
public.ecr.aws/o5l1c9o9/spark-scala-container/spark   my-tag    7fc25f409862   19 seconds ago      601MB
spark-container                                       latest    2673042eb6e8   About an hour ago   524MB
public.ecr.aws/o5l1c9o9/spark-scala-container         latest    2673042eb6e8   About an hour ago   524MB
```

### 3. 이미지 푸시 ###

```
$ ./bin/docker-image-tool.sh -r public.ecr.aws/o5l1c9o9/spark-scala-container -t my-tag push
```
