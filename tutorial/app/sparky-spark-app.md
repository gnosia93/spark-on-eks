인털레J로 스파크 스칼라 어플리케이션을 개발하는 데 필요한 환경 설정에 대해서 설명한다.


## 인텔리J 설치하기 ##
https://www.jetbrains.com/ko-kr/idea/download/#section=mac 으로 방문하여 커뮤니티 버전을 다운로드하여 로컬PC 에 설치한다.

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/intelij-community.png)


## sbt (Simple Build Tool) 설치 ##
```
$ brew install sbt

$ % sbt --version
sbt version in this project: 1.8.2
sbt script version: 1.8.2
```

## InteliJ sbt 플러그인 설치  ##

Preferences > Plugins 에서 인텔리J 스칼러 플러그인을 설치한다.
![](https://github.com/gnosia93/spark-on-eks/blob/main/images/intelij-scala-plugin.png)


## 스칼라 어플리케이션 개발 ##

### 1. 프로젝트 생성하기 ###

New Project 화면 덤프.
,,,,
 
### 2. build.sbt 파일 생성하기 ###

메이븐 레포지토리(https://mvnrepository.com/search?q=spark) 에서 스파크를 검색해서 스파크 및 스칼러 버전을 확인하고, 인텔리J SparkySpark 프로젝트의 build.sbt 파일을 업데이트 한다. 
![](https://github.com/gnosia93/spark-on-eks/blob/main/images/mvn-spark-scala.png)

[build.sbt]
```
ThisBuild / version := "0.1.0-SNAPSHOT"
ThisBuild / scalaVersion := "2.13.10"

lazy val root = (project in file("."))
  .settings(
    name := "SparkySpark"
  )

libraryDependencies += "org.scala-lang" % "scala-library" % "2.13.10"
val sparkVersion = "3.3.1"
libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % sparkVersion,
  "org.apache.spark" %% "spark-sql" % sparkVersion,
  "org.apache.spark" %% "spark-mllib" % sparkVersion,
  "org.apache.hadoop" % "hadoop-aws" % "3.3.4",
  "org.apache.hadoop" % "hadoop-common" % "3.3.4"
)
```

build.sbt 파일의 libraryDependencies 이 변경되는 경우 아래와 같이 터미널 창에서 sbt package 명령어를 수행하여, 
sbt 가 관련 자바 패키지를 다운로드 할 수 있도록 한다. 
![](https://github.com/gnosia93/spark-on-eks/blob/main/images/intelij-sbt-package.png)


### 3. 어플리케이션 코딩 ###

![](https://github.com/gnosia93/spark-on-eks/blob/main/images/intelij-sparky-spark.png)

### 4. 우버 Jar 생성 ###
 
 우버 jar를 생성하기 위해 sbt-asssembly 패키지를 사용합니다.
 assemby.sbt 를 신규로 생성하고, 기존 build.sbt 는 아래와 같이 Merge 룰을 설정합니다.
 
![](https://github.com/gnosia93/spark-on-eks/blob/main/images/sbt-assembly-assembly.sbt.png)

[assembly.sbt]
```
addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.15.0")
addSbtPlugin("org.jetbrains" % "sbt-ide-settings" % "1.1.0")
```
[build.sbt]
```
ThisBuild / version := "0.1.0-SNAPSHOT"
ThisBuild / scalaVersion := "2.13.10"

lazy val root = (project in file("."))
  .settings(
    name := "SparkySpark"
  )

libraryDependencies += "org.scala-lang" % "scala-library" % "2.13.10"
val sparkVersion = "3.3.1"
libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % sparkVersion,
  "org.apache.spark" %% "spark-sql" % sparkVersion,
  "org.apache.spark" %% "spark-mllib" % sparkVersion,
  "org.apache.hadoop" % "hadoop-aws" % "3.3.4",
  "org.apache.hadoop" % "hadoop-common" % "3.3.4"
)

mainClass := Some("SparkySpark")
// externalResolvers := Seq("central repository".at("https://repo1.maven.org/maven2/"))
assemblyMergeStrategy in assembly := {
  case PathList("META-INF", xs@_*) =>
    (xs map {
      _.toLowerCase
    }) match {
      case ("manifest.mf" :: Nil) | ("index.list" :: Nil) | ("dependencies" :: Nil) =>
        MergeStrategy.discard
      case ps@(x :: xs) if ps.last.endsWith(".sf") || ps.last.endsWith(".dsa") =>
        MergeStrategy.discard
      case "plexus" :: xs =>
        MergeStrategy.discard
      case "services" :: xs =>
        MergeStrategy.filterDistinctLines
      case ("spring.schemas" :: Nil) | ("spring.handlers" :: Nil) =>
        MergeStrategy.filterDistinctLines
      case _ => MergeStrategy.concat
    }
  case x => MergeStrategy.first
}
```
터미널 창에서 sbt assembly 명령어를 수행하여, fat jar 를 만든다. SparkySpark/target/scala-2.13/SparkySpark-assembly-0.1.0-SNAPSHOT.jar 파일이 생성되었다. 
```
(base) SparkySpark $ sbt assembly -J-Xmx4G -J-Xms4G

[info] welcome to sbt 1.8.2 (JetBrains s.r.o. Java 11.0.13)
[info] loading global plugins from /Users/soonbeom/.sbt/1.0/plugins
[info] loading settings for project sparkyspark-build from assembly.sbt ...
[info] loading project definition from /Users/soonbeom/IdeaProjects/SparkySpark/project
[info] loading settings for project root from build.sbt ...
[info] set current project to SparkySpark (in build file:/Users/soonbeom/IdeaProjects/SparkySpark/)
[info] Strategy 'discard' was applied to 1224 files (Run the task at debug level to see details)
[info] Strategy 'first' was applied to 6964 files (Run the task at debug level to see details)
[info] Assembly up to date: /Users/soonbeom/IdeaProjects/SparkySpark/target/scala-2.13/SparkySpark-assembly-0.1.0-SNAPSHOT.jar
[success] Total time: 18 s, completed 2023. 2. 5. 오후 6:09:15

(base) SparkySpark % ls -la /Users/soonbeom/IdeaProjects/SparkySpark/target/scala-2.13/SparkySpark-assembly-0.1.0-SNAPSHOT.jar
-rw-r--r--  1 soonbeom  staff  523054664 Feb  5 18:08 /Users/soonbeom/IdeaProjects/SparkySpark/target/scala-2.13/SparkySpark-assembly-0.1.0-SNAPSHOT.jar
(base) SparkySpark $ 
```

### 5. 어플리케이션 실행하기 ###
```
$ java -jar /Users/soonbeom/IdeaProjects/SparkySpark/target/scala-2.13/SparkySpark-assembly-0.1.0-SNAPSHOT.jar
```

## 참고자료 ##
* https://cloud.google.com/dataproc/docs/guides/manage-spark-dependencies?hl=ko
* https://jhleeeme.github.io/read-aws-s3-data-on-spark/
* https://velog.io/@msjeong97/IntelliJ%EC%97%90%EC%84%9C-scala-project-%EC%83%9D%EC%84%B1-%EB%B0%8F-sbt-%EC%84%A4%EC%A0%95
* https://jaemunbro.medium.com/spark-executor-%EA%B0%9C%EC%88%98-%EC%A0%95%ED%95%98%EA%B8%B0-b9f0e0cc1fd8