---
title: "Hadoop 명령어 모음"
date: 2023-02-05T17:02:26+09:00
tags: ["hadoop"]
categories: ["troubleshooting", "knowledge"]
ShowToc: true
ShowBreadCrumbs: true
---

# HDFS

## 재시작 없이 설정 변경 (reconfigure)

https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html

- 재시작 없이 변경 가능한 key는 제한적임

```
$ hdfs dfsadmin -reconfig namenode nn1.example.com:8020 properties
Node [nn1.example.com:8020] Reconfigurable properties:
dfs.block.placement.ec.classname
dfs.block.replicator.classname
dfs.heartbeat.interval
dfs.image.parallel.load
dfs.namenode.avoid.read.slow.datanode
dfs.namenode.block-placement-policy.exclude-slow-nodes.enabled
dfs.namenode.heartbeat.recheck-interval
dfs.namenode.max.slowpeer.collect.nodes
dfs.namenode.replication.max-streams
dfs.namenode.replication.max-streams-hard-limit
dfs.namenode.replication.work.multiplier.per.iteration
dfs.storage.policy.satisfier.mode
fs.protected.directories
hadoop.caller.context.enabled
ipc.8020.backoff.enable
```
