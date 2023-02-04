


## 실습순서 ##

* 로컥 PC 도커 데스크 탑 설치

* [1. 주피터 노트북 설정하기](https://github.com/gnosia93/spark-on-eks/blob/main/tutorial/1.jupyter-setup.md)

* [2. EKS 클러스터 생성하기](https://github.com/gnosia93/spark-on-eks/blob/main/tutorial/2.eks-install.md)

* [3. 스파크 컨테이너 이미지 생성하기](https://github.com/gnosia93/spark-on-eks/blob/main/tutorial/3.spark-container.md)

* [4. 스파크 어플리케이션 실행](https://github.com/gnosia93/spark-on-eks/blob/main/tutorial/4.spark-app.md)


```
모든 컨테이너 삭제하기

docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

모든 이미지 삭제하기
docker rmi $(docker images -q)

Exit 상태의 모든 컨테이너 삭제하기
docker rm $(docker ps --filter 'status=exited' -a -q)
```

```
% kubectl config get-contexts
CURRENT   NAME                                             CLUSTER                                 AUTHINFO                                         NAMESPACE
          docker-desktop                                   docker-desktop                          docker-desktop
*         hopigaga@spark-on-eks.ap-northeast-2.eksctl.io   spark-on-eks.ap-northeast-2.eksctl.io   hopigaga@spark-on-eks.ap-northeast-2.eksctl.io
(base) soonbeom@bcd07468d10a .kube % kubectl config use-context docker-desktop
Switched to context "docker-desktop".
(base) soonbeom@bcd07468d10a .kube % kubectl config get-contexts
CURRENT   NAME                                             CLUSTER                                 AUTHINFO                                         NAMESPACE
*         docker-desktop                                   docker-desktop                          docker-desktop
          hopigaga@spark-on-eks.ap-northeast-2.eksctl.io   spark-on-eks.ap-northeast-2.eksctl.io   hopigaga@spark-on-eks.ap-northeast-2.eksctl.io

% kubectl get nodes
NAME             STATUS   ROLES           AGE   VERSION
docker-desktop   Ready    control-plane   24m   v1.25.2
(base) soonbeom@bcd07468d10a .kube % kubectl get pods -A
NAMESPACE     NAME                                     READY   STATUS    RESTARTS        AGE
kube-system   coredns-95db45d46-8s6hx                  1/1     Running   1 (22m ago)     24m
kube-system   coredns-95db45d46-sr2js                  1/1     Running   1 (22m ago)     24m
kube-system   etcd-docker-desktop                      1/1     Running   1 (22m ago)     24m
kube-system   kube-apiserver-docker-desktop            1/1     Running   1 (22m ago)     24m
kube-system   kube-controller-manager-docker-desktop   1/1     Running   1 (22m ago)     24m
kube-system   kube-proxy-k95nn                         1/1     Running   1 (22m ago)     24m
kube-system   kube-scheduler-docker-desktop            1/1     Running   1 (22m ago)     24m
kube-system   storage-provisioner                      1/1     Running   1 (22m ago)     24m
kube-system   vpnkit-controller                        1/1     Running   3 (3m37s ago)   24m

```

