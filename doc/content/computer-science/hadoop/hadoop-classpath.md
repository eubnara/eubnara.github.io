---
title: 'About "HADOOP_CLASSPATH" environment variable'
date: 2023-02-05T16:54:58+09:00
tags: ["hadoop", "hive"]
categories: ["knowledge"]
ShowToc: true
ShowBreadCrumbs: true
---

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/UnixShellGuide.html#HADOOP_CLASSPATH

In Hadoop ecosystem, `HADOOP_CLASSPATH` environment variable is commonly used in many places. `Hive` is use this variable, too.
I wonder that how the `HADOOP_CLASSPATH` variable is used in a script like `beeline`. I cannot find `HADOOP_CLASSPATH` variable in `Hive` source codes.
I finally figure out that when executing `beeline` it uses `hadoop jar` command. (https://github.com/apache/hive/blob/rel/release-3.1.3/bin/ext/beeline.sh#L35)
It uses `RunJar.java` where `HADOOP_CLASSPATH` is used to set `CLASSPATH`. (https://github.com/apache/hadoop/blob/rel/release-3.3.4/hadoop-common-project/hadoop-common/src/main/java/org/apache/hadoop/util/RunJar.java#L347-L351)

If something in Hadoop ecosystem uses `RunJar#main`, it probably repect `HADOOP_CLASSPATH` environment variable.
