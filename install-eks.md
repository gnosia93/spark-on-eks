
### 1.kubectl 설치하기 ###

```
$ curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.7/2022-10-31/bin/darwin/amd64/kubectl
$ chmod +x ./kubectl
$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
$ kubectl version --short --client
```

### 2. eksctl 설치하기 ###

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
$ brew upgrade eksctl && { brew link --overwrite eksctl; } || { brew tap weaveworks/tap; brew install weaveworks/tap/eksctl; }
$ eksctl version
0.127.0
```


### 3. eks 클러스터 생성 ###

```
$ eksctl create cluster --name spark-on-eks --region ap-northeast-2
2023-02-02 16:01:02 [ℹ]  eksctl version 0.127.0
2023-02-02 16:01:02 [ℹ]  using region ap-northeast-2
2023-02-02 16:01:03 [ℹ]  setting availability zones to [ap-northeast-2c ap-northeast-2a ap-northeast-2d]
2023-02-02 16:01:03 [ℹ]  subnets for ap-northeast-2c - public:192.168.0.0/19 private:192.168.96.0/19
2023-02-02 16:01:03 [ℹ]  subnets for ap-northeast-2a - public:192.168.32.0/19 private:192.168.128.0/19
2023-02-02 16:01:03 [ℹ]  subnets for ap-northeast-2d - public:192.168.64.0/19 private:192.168.160.0/19
2023-02-02 16:01:03 [ℹ]  nodegroup "ng-2560a3f5" will use "" [AmazonLinux2/1.24]
2023-02-02 16:01:03 [ℹ]  using Kubernetes version 1.24
2023-02-02 16:01:03 [ℹ]  creating EKS cluster "spark-on-eks" in "ap-northeast-2" region with managed nodes
2023-02-02 16:01:03 [ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial managed nodegroup
2023-02-02 16:01:03 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=ap-northeast-2 --cluster=spark-on-eks'
2023-02-02 16:01:03 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "spark-on-eks" in "ap-northeast-2"
2023-02-02 16:01:03 [ℹ]  CloudWatch logging will not be enabled for cluster "spark-on-eks" in "ap-northeast-2"
2023-02-02 16:01:03 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=ap-northeast-2 --cluster=spark-on-eks'
2023-02-02 16:01:03 [ℹ]
2 sequential tasks: { create cluster control plane "spark-on-eks",
    2 sequential sub-tasks: {
        wait for control plane to become ready,
        create managed nodegroup "ng-2560a3f5",
    }
}
2023-02-02 16:01:03 [ℹ]  building cluster stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:01:03 [ℹ]  deploying stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:01:33 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:02:03 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:03:04 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:04:05 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:05:05 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:06:06 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:07:06 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:08:08 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:09:08 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:10:09 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:11:09 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:12:10 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:13:14 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-cluster"
2023-02-02 16:15:15 [ℹ]  building managed nodegroup stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 16:15:16 [ℹ]  deploying stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 16:15:16 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 16:15:46 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 16:16:45 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 16:17:31 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 16:18:48 [ℹ]  waiting for CloudFormation stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 16:18:48 [ℹ]  waiting for the control plane to become ready
2023-02-02 16:18:48 [✔]  saved kubeconfig as "/Users/soonbeom/.kube/config"
2023-02-02 16:18:48 [ℹ]  no tasks
2023-02-02 16:18:48 [✔]  all EKS cluster resources for "spark-on-eks" have been created
2023-02-02 16:18:48 [ℹ]  nodegroup "ng-2560a3f5" has 2 node(s)
2023-02-02 16:18:48 [ℹ]  node "ip-192-168-31-233.ap-northeast-2.compute.internal" is ready
2023-02-02 16:18:48 [ℹ]  node "ip-192-168-60-56.ap-northeast-2.compute.internal" is ready
2023-02-02 16:18:48 [ℹ]  waiting for at least 2 node(s) to become ready in "ng-2560a3f5"
2023-02-02 16:18:48 [ℹ]  nodegroup "ng-2560a3f5" has 2 node(s)
2023-02-02 16:18:48 [ℹ]  node "ip-192-168-31-233.ap-northeast-2.compute.internal" is ready
2023-02-02 16:18:48 [ℹ]  node "ip-192-168-60-56.ap-northeast-2.compute.internal" is ready
2023-02-02 16:18:49 [ℹ]  kubectl command should work with "/Users/soonbeom/.kube/config", try 'kubectl get nodes'
2023-02-02 16:18:49 [✔]  EKS cluster "spark-on-eks" in "ap-northeast-2" region is ready
$
```
VPC 를 비롯한 서브넷, 시큐리티 그룹 등과 같은 AWS 리소스가 자동으로 생성된다.


### 4. 클러스터 생성 확인 ###
```
$ kubectl get nodes
NAME                                                STATUS   ROLES    AGE     VERSION
ip-192-168-31-233.ap-northeast-2.compute.internal   Ready    <none>   4m10s   v1.24.9-eks-49d8fe8
ip-192-168-60-56.ap-northeast-2.compute.internal    Ready    <none>   4m8s    v1.24.9-eks-49d8fe8
$
$ kubectl get pods -A
NAMESPACE     NAME                      READY   STATUS    RESTARTS   AGE
kube-system   aws-node-l7b4d            1/1     Running   0          4m28s
kube-system   aws-node-z8l8s            1/1     Running   0          4m26s
kube-system   coredns-dc4979556-mp8vm   1/1     Running   0          12m
kube-system   coredns-dc4979556-p547d   1/1     Running   0          12m
kube-system   kube-proxy-4lz65          1/1     Running   0          4m28s
kube-system   kube-proxy-j4fpj          1/1     Running   0          4m26s
```
