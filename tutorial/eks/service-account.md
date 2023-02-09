## IAM 서비스 어카운트 ##

1. https://levelup.gitconnected.com/using-iam-roles-to-allow-the-pods-in-aws-eks-to-read-the-aws-s3-bucket-be493fbdda84 문서를 참고 하여 관련 오브젝트를 생성하고, S3 버킷에 대한 접근 권한 설정시 아래 Policy 적용한다.

[SparkOnEKS-S3Policy]
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
                "s3:PutBucketPolicy",
                "s3:DeleteObject",
                "s3:GetBucketPolicy",
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

2. spark 서비스 어카운트에 위의 과정에서 생성된 Role을 바인딩 한다.

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

서비스 어카운트 값이 spark 인 nginx yaml 로 컨테이너를 생성 한후, 아래와 같이 s3의 read, write 가능 여부를 테스트 한다.
오류 없이 수행되는 경우, 정책값이 제대로 설정된 것이다. 컨테이너에서 bash 를 실행중인 상태에서 S3 정책을 수정하는 경우, 컨테이너에서 빠져나와서 bash 를 재 실행해야 한다.   

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
