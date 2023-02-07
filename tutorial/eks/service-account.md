## IAM 서비스 어카운트 설정 하기 ##

https://levelup.gitconnected.com/using-iam-roles-to-allow-the-pods-in-aws-eks-to-read-the-aws-s3-bucket-be493fbdda84 문서에 따라 설정한다.


## 테스트 ##

서비스 어카운트 값이 spark 인 nginx yaml 로 컨테이너를 생성 한후, 아래와 같이 aws ls 명령어가 수행가능 한지 여부를 판단한다.
수행되는 경우 IAM 서비스 어카운트 설정이 완료 된 것이다.

[shell.yaml]
```
apiVersion: v1
kind: Pod
metadata:
  name: shell-demo
spec:
  volumes:
  - name: shared-data
    emptyDir: {}
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: shared-data
      mountPath: /usr/share/nginx/html
  hostNetwork: true
  dnsPolicy: Default
  serviceAccount: spark
```

```
$ kubectl apply -f shell.yaml

$ kubectl exec --stdin --tty shell-demo -- /bin/bash

root@ip-192-168-8-112:/# curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
                         apt update && apt upgrade && \
                         apt install unzip && \
                         unzip awscliv2.zip && \
                         ./aws/install

root@ip-192-168-80-19:/# aws s3 ls
...
2021-06-30 06:55:34 sagemaker-ap-northeast-2-509076023497
2021-07-30 00:44:45 sagemaker-studio-509076023497-58hjm8wua5j
2023-02-05 01:35:49 spark-on-eks-soonbeom
```

## 설정 ##

[spark.yaml]
```
apiVersion: v1
kind: ServiceAccount
metadata:
 name: spark
 namespace: default
 annotations:
   eks.amazonaws.com/role-arn:  arn:aws:iam::509076023497:role/SparkOnEKS-S3Role
```




## 참고자료 ##



* https://dev.to/aws-builders/working-with-eks-using-iam-and-native-k8s-service-accounts-to-access-aws-s3-3e20

* https://brunch.co.kr/@topasvga/1885
