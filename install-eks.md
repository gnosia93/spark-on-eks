
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



### (Optional) 5. 노드 그룹 생성 ###
```
$ eksctl create nodegroup \
  --cluster spark-on-eks \
  --name spark-ng \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --ssh-access \
  --managed=false \
  --ssh-public-key aws-kp
  
$ eksctl get nodegroup --cluster=spark-on-eks
CLUSTER		NODEGROUP	STATUS		CREATED			MIN SIZE	MAX SIZE	DESIRED CAPACITY	INSTANCE TYPE	IMAGE ID		ASG NAME							TYPE
spark-on-eks	ng-2560a3f5	ACTIVE		2023-02-02T07:16:10Z	2		2		2			m5.large	AL2_x86_64		eks-ng-2560a3f5-d4c3087d-c634-f390-4ab1-f51f5dbb8ada		managed
spark-on-eks	spark-ng	CREATE_COMPLETE	2023-02-02T10:32:08Z	1		4		3			t3.medium	ami-0387dcacac4a946f3	eksctl-spark-on-eks-nodegroup-spark-ng-NodeGroup-DSN8A8DG1ZR	unmanaged

$ kubectl get nodes
NAME                                                STATUS   ROLES    AGE     VERSION
ip-192-168-21-146.ap-northeast-2.compute.internal   Ready    <none>   15m     v1.24.9-eks-49d8fe8
ip-192-168-31-233.ap-northeast-2.compute.internal   Ready    <none>   3h36m   v1.24.9-eks-49d8fe8
ip-192-168-56-158.ap-northeast-2.compute.internal   Ready    <none>   15m     v1.24.9-eks-49d8fe8
ip-192-168-60-56.ap-northeast-2.compute.internal    Ready    <none>   3h36m   v1.24.9-eks-49d8fe8
ip-192-168-94-56.ap-northeast-2.compute.internal    Ready    <none>   15m     v1.24.9-eks-49d8fe8
```

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/eks-nodegroup-1.png)

### (Optional) 6. 노드그룹 교체 ###
```
$ eksctl delete nodegroup --cluster spark-on-eks --name ng-2560a3f5
2023-02-02 19:55:50 [ℹ]  1 nodegroup (ng-2560a3f5) was included (based on the include/exclude rules)
2023-02-02 19:55:50 [ℹ]  will drain 1 nodegroup(s) in cluster "spark-on-eks"
2023-02-02 19:55:50 [ℹ]  starting parallel draining, max in-flight of 1
2023-02-02 19:55:50 [ℹ]  cordon node "ip-192-168-31-233.ap-northeast-2.compute.internal"
2023-02-02 19:55:50 [ℹ]  cordon node "ip-192-168-60-56.ap-northeast-2.compute.internal"
2023-02-02 19:56:01 [✔]  drained all nodes: [ip-192-168-31-233.ap-northeast-2.compute.internal ip-192-168-60-56.ap-northeast-2.compute.internal]
2023-02-02 19:56:01 [ℹ]  will delete 1 nodegroups from cluster "spark-on-eks"
2023-02-02 19:56:02 [ℹ]  1 task: { 1 task: { delete nodegroup "ng-2560a3f5" [async] } }
2023-02-02 19:56:03 [ℹ]  will delete stack "eksctl-spark-on-eks-nodegroup-ng-2560a3f5"
2023-02-02 19:56:03 [ℹ]  will delete 0 nodegroups from auth ConfigMap in cluster "spark-on-eks"
2023-02-02 19:56:03 [✔]  deleted 1 nodegroup(s) from cluster "spark-on-eks"

$ kubectl get nodes -o wide
NAME                                                STATUS   ROLES    AGE   VERSION               INTERNAL-IP      EXTERNAL-IP     OS-IMAGE         KERNEL-VERSION                 CONTAINER-RUNTIME
ip-192-168-21-146.ap-northeast-2.compute.internal   Ready    <none>   22m   v1.24.9-eks-49d8fe8   192.168.21.146   54.180.99.166   Amazon Linux 2   5.4.228-131.415.amzn2.x86_64   containerd://1.6.6
ip-192-168-56-158.ap-northeast-2.compute.internal   Ready    <none>   22m   v1.24.9-eks-49d8fe8   192.168.56.158   52.78.166.5     Amazon Linux 2   5.4.228-131.415.amzn2.x86_64   containerd://1.6.6
ip-192-168-94-56.ap-northeast-2.compute.internal    Ready    <none>   22m   v1.24.9-eks-49d8fe8   192.168.94.56    3.38.147.111    Amazon Linux 2   5.4.228-131.415.amzn2.x86_64   containerd://1.6.6

$ kubectl get pods -A -o wide
NAMESPACE     NAME                      READY   STATUS    RESTARTS   AGE     IP               NODE                                                NOMINATED NODE   READINESS GATES
kube-system   aws-node-8gv88            1/1     Running   0          23m     192.168.21.146   ip-192-168-21-146.ap-northeast-2.compute.internal   <none>           <none>
kube-system   aws-node-pbpwm            1/1     Running   0          23m     192.168.94.56    ip-192-168-94-56.ap-northeast-2.compute.internal    <none>           <none>
kube-system   aws-node-qvrxd            1/1     Running   0          23m     192.168.56.158   ip-192-168-56-158.ap-northeast-2.compute.internal   <none>           <none>
kube-system   coredns-dc4979556-6zllg   1/1     Running   0          4m57s   192.168.74.110   ip-192-168-94-56.ap-northeast-2.compute.internal    <none>           <none>
kube-system   coredns-dc4979556-nv7rq   1/1     Running   0          4m57s   192.168.52.85    ip-192-168-56-158.ap-northeast-2.compute.internal   <none>           <none>
kube-system   kube-proxy-48gdt          1/1     Running   0          23m     192.168.56.158   ip-192-168-56-158.ap-northeast-2.compute.internal   <none>           <none>
kube-system   kube-proxy-d7r62          1/1     Running   0          23m     192.168.94.56    ip-192-168-94-56.ap-northeast-2.compute.internal    <none>           <none>
kube-system   kube-proxy-s4zpz          1/1     Running   0          23m     192.168.21.146   ip-192-168-21-146.ap-northeast-2.compute.internal   <none>           <none>
```

self-managed 노드그룹 교체했기 때문에 노드 그룹 리스트에 나오지 않는다.

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/eks-nodegroup-2.png)

## 참고자료 ##

* https://stackoverflow.com/questions/64059038/the-results-of-aws-eks-list-nodegroups-and-eksctl-get-nodegroups-are-inconsi
