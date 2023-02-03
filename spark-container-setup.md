
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









## 참고자료 ##
* https://docs.aws.amazon.com/AmazonECR/latest/public/getting-started-cli.html
*  https://spark.apache.org/docs/latest/running-on-kubernetes.html
*  https://spark.apache.org/docs/latest/configuration.html
