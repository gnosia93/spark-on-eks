## 테스트 ##

생성된 서비스 어카운트가 제대로 동작하는지 테스트 한다.

```

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

컨피그맵
```
$ kubectl describe configmap spark-drv-3ba9488626b0e079-conf-map
% kubectl describe configmap spark-drv-3ba9488626b0e079-conf-map
Name:         spark-drv-3ba9488626b0e079-conf-map
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
spark.kubernetes.namespace:
----
default
spark.properties:
----
#Java properties built from Kubernetes config map with name: spark-drv-3ba9488626b0e079-conf-map
#Mon Feb 06 21:26:11 KST 2023
spark.driver.port=7078
spark.master=k8s\://https\://FC91D79A06A89466662783481F8328C7.gr7.ap-northeast-2.eks.amazonaws.com
spark.submit.pyFiles=
spark.app.name=sparky-spark-app
spark.kubernetes.resource.type=java
spark.submit.deployMode=cluster
spark.driver.host=sparky-spark-app-3b69a48626b0db6c-driver-svc.default.svc
spark.driver.blockManager.port=7079
spark.app.id=spark-32e062565f0a426598dd7f7c508994d5
spark.app.submitTime=1675686370063
spark.kubernetes.container.image=public.ecr.aws/o5l1c9o9/sparky-spark-app\:latest
spark.kubernetes.memoryOverheadFactor=0.1
spark.kubernetes.submitInDriver=true
spark.kubernetes.context=hopigaga@spark-on-eks.ap-northeast-2.eksctl.io
spark.kubernetes.authenticate.driver.serviceAccountName=spark
spark.kubernetes.driver.pod.name=sparky-spark-app-3b69a48626b0db6c-driver
spark.executor.instances=5
spark.jars=local\:////opt/spark/app/SparkySpark-assembly-0.1.0-SNAPSHOT.jar


BinaryData
====

Events:  <none>
(base) soonbeom@bcd07468d10a ~ % cd bin
```


## 참고자료 ##

* https://levelup.gitconnected.com/using-iam-roles-to-allow-the-pods-in-aws-eks-to-read-the-aws-s3-bucket-be493fbdda84

* https://dev.to/aws-builders/working-with-eks-using-iam-and-native-k8s-service-accounts-to-access-aws-s3-3e20

* https://brunch.co.kr/@topasvga/1885
