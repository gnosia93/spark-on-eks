### 1. HomeBrew 설치 ###

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. spylon-kernel ###
```
$ conda install -y -c conda-forge spylon-kernel
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /opt/anaconda3

  added / updated specs:
    - spylon-kernel


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    ipyparallel-8.4.1          |     pyhd8ed1ab_0         199 KB  conda-forge
    metakernel-0.29.4          |     pyhd8ed1ab_0         182 KB  conda-forge
    portalocker-2.7.0          |   py39h6e9494a_0          31 KB  conda-forge
    spylon-0.3.0               |             py_1          53 KB  conda-forge
    spylon-kernel-0.4.1        |          py_1000          17 KB  conda-forge
    ------------------------------------------------------------
                                           Total:         483 KB

The following NEW packages will be INSTALLED:

  ipyparallel        conda-forge/noarch::ipyparallel-8.4.1-pyhd8ed1ab_0
  metakernel         conda-forge/noarch::metakernel-0.29.4-pyhd8ed1ab_0
  portalocker        conda-forge/osx-64::portalocker-2.7.0-py39h6e9494a_0
  spylon             conda-forge/noarch::spylon-0.3.0-py_1
  spylon-kernel      conda-forge/noarch::spylon-kernel-0.4.1-py_1000



Downloading and Extracting Packages

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
$

$ sudo python -m spylon_kernel install
Password:
[InstallKernelSpec] Installed kernelspec spylon-kernel in /usr/local/share/jupyter/kernels/spylon-kernel
```

### 3. 노트북 실행 ###

Launcher 탭의 Notebook 섹션에서 spylon-kernel 을 선택해서 새로운 노트북을 연다.

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/scala-spark-0.png)

생성된 노트북에서 아래 코드를 실행하고, spark 및 scala 의 동작여부를 확인한다.

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/scala-spark.png)

스파크 웹 UI (http://localhost:4041) 를 통해 스파크의 환경 변수를 확인한다.
![](https://github.com/gnosia93/spark-on-eks/blob/main/images/spark-web-ui.png)

## 참고자료 ##

* [Set up a local Pyspark/Scala Spark Environment with Jupyter on Mac](https://www.youtube.com/watch?v=kSYtgiBIO2k)
* [HomeBrew 설치](https://brew.sh/index_ko)
