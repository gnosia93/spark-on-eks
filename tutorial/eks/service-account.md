* https://levelup.gitconnected.com/using-iam-roles-to-allow-the-pods-in-aws-eks-to-read-the-aws-s3-bucket-be493fbdda84

* https://dev.to/aws-builders/working-with-eks-using-iam-and-native-k8s-service-accounts-to-access-aws-s3-3e20

* https://brunch.co.kr/@topasvga/1885



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

```
$ kubectl config get-contexts
CURRENT   NAME                                             CLUSTER                                 AUTHINFO                                         NAMESPACE
          docker-desktop                                   docker-desktop                          docker-desktop
*         hopigaga@spark-on-eks.ap-northeast-2.eksctl.io   spark-on-eks.ap-northeast-2.eksctl.io   hopigaga@spark-on-eks.ap-northeast-2.eksctl.io
(base) soonbeom@bcd07468d10a spark-on-eks %

$ kubectl create -f spark.yaml
$ kubectl apply -f spark.yaml

$ kubectl get sa spark
NAME    SECRETS   AGE
spark   0         2d12h

$ kubectl get sa spark -o yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::509076023497:role/SparkOnEKS-S3Role
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"ServiceAccount","metadata":{"annotations":{"eks.amazonaws.com/role-arn":"arn:aws:iam::509076023497:role/SparkOnEKS-S3Role"},"name":"spark","namespace":"default"}}
  creationTimestamp: "2023-02-04T11:51:07Z"
  name: spark
  namespace: default
  resourceVersion: "678950"
  uid: 10f033ff-73ef-4a32-93ba-ca2e797f46af
```
