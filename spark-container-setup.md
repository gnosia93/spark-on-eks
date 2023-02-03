
### 1. ecr 생성 ###

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



 * https://docs.aws.amazon.com/AmazonECR/latest/public/getting-started-cli.html






## 참고자료 ##

*  https://spark.apache.org/docs/latest/running-on-kubernetes.html
*  https://spark.apache.org/docs/latest/configuration.html
