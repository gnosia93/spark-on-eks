


## 실습순서 ##

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
