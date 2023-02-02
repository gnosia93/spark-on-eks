
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
```
VPC 를 비롯한 서브넷, 시큐리티 그룹 등과 같은 AWS 리소스가 자동으로 생성된다.
