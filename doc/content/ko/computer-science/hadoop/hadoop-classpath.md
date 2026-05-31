---
title: '"HADOOP_CLASSPATH" 환경 변수에 대해'
date: 2023-02-05T16:54:58+09:00
tags: ["hadoop", "hive"]
categories: ["knowledge"]
ShowToc: true
ShowBreadCrumbs: true
---

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/UnixShellGuide.html#HADOOP_CLASSPATH

Hadoop 생태계에서 `HADOOP_CLASSPATH` 환경 변수는 다양한 곳에서 사용된다. `Hive`도 이 변수를 사용한다.
`beeline` 같은 스크립트에서 `HADOOP_CLASSPATH` 변수가 어떻게 사용되는지 궁금했다. Hive 소스 코드에서는 `HADOOP_CLASSPATH` 변수를 찾을 수 없었다.
알고 보니 `beeline` 실행 시 `hadoop jar` 명령을 사용한다. (https://github.com/apache/hive/blob/rel/release-3.1.3/bin/ext/beeline.sh#L35)
이때 `RunJar.java`가 사용되며, 여기서 `HADOOP_CLASSPATH`가 `CLASSPATH` 설정에 사용된다. (https://github.com/apache/hadoop/blob/rel/release-3.3.4/hadoop-common-project/hadoop-common/src/main/java/org/apache/hadoop/util/RunJar.java#L347-L351)

Hadoop 생태계에서 `RunJar#main`을 사용하는 경우, 대부분 `HADOOP_CLASSPATH` 환경 변수를 따르게 된다.
