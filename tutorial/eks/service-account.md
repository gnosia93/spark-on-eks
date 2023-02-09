## IAM 서비스 어카운트 ##

* https://levelup.gitconnected.com/using-iam-roles-to-allow-the-pods-in-aws-eks-to-read-the-aws-s3-bucket-be493fbdda84 문서를 참고 하여 관련 설정을 한다. S3 버킷에 대한 접근 권한 설정시 List, Get, Put 모두 설정한다.

[SparkOnEKS-S3Polocy]
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:ListBucketMultipartUploads",
                "s3:AbortMultipartUpload",
                "s3:PutObjectVersionAcl",
                "s3:DeleteObject",
                "s3:PutObjectAcl",
                "s3:ListMultipartUploadParts"
            ],
            "Resource": [
                "arn:aws:s3:::spark-on-eks-*",
                "arn:aws:s3:::spark-on-eks-*/*"
            ]
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Sid": "VisualEditor2",
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "arn:aws:s3:::*"
        }
    ]
}
```

* spark 서비스 어카운트에 위의 과정에서 생성된 Role을 바인딩 한다.

[spark.yaml]
```
apiVersion: v1
kind: ServiceAccount
metadata:
 name: spark
 namespace: default
 annotations:
   eks.amazonaws.com/role-arn:  arn:aws:iam::xxxxxx:role/SparkOnEKS-S3Role
```
$ kubectl apply -f spark.yaml 



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

root@ip-192-168-80-19:/# aws s3 cp awscliv2.zip s3://spark-on-eks-soonbeom
upload: ./awscliv2.zip to s3://spark-on-eks-soonbeom/awscliv2.zip
root@ip-192-168-80-19:/# aws s3 ls spark-on-eks-soonbeom
                           PRE raw/
2023-02-09 11:00:58   49129277 awscliv2.zip
root@ip-192-168-80-19:/#
```





## 참고자료 ##



* https://dev.to/aws-builders/working-with-eks-using-iam-and-native-k8s-service-accounts-to-access-aws-s3-3e20

* https://brunch.co.kr/@topasvga/1885
