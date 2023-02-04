


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



```

