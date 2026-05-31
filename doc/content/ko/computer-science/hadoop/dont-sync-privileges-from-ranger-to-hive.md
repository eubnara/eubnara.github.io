---
title: Ranger에서 Hive로 권한 동기화 비활성화
date: 2024-10-19T16:26:00+09:00
tags: ["ranger", "hive"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

Secure Hadoop 클러스터에서 Apache Ranger로 인가를 수행할 때, Hive 4.x.x 미만 버전은 기본적으로 모든 Ranger Hive 정책을 Hive용 RDBMS에 동기화한다. 이는 불필요하며 DB에 높은 부하를 유발할 수 있다. (https://issues.apache.org/jira/browse/HIVE-25391) HiveServer2에서 다음 설정으로 비활성화할 수 있다.

```
hive.privilege.synchronizer=false
```
