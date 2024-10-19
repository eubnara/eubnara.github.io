---
title: Don't sync privileges from ranger to hive
date: 2024-10-19T16:26:00+09:00
tags: ["ranger", "hive"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

When Apache Ranger is configured for authorization on Secure Hadoop cluster, Hive below 4.x.x synchronizes all the ranger hive policies to rdbms for Hive as default. It is unnecessary and make unnecessary high load on db. See https://issues.apache.org/jira/browse/HIVE-25391. You can disable it with the configurations as follows.

```
hive.privilege.synchronizer=false
```
