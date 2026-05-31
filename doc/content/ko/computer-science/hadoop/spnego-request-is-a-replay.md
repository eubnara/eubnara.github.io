---
title: SPNEGO 활성화 Hadoop DataNode가 Kerberos "replay attack"을 오판하는 문제
date: 2023-02-05T16:01:17+09:00
tags: ["hadoop", "spnego"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

- 참고
  - https://docs.cloudera.com/cloudera-manager/7.5.5/security-troubleshooting/cm-security-troubleshooting.pdf
  - https://search-guard.com/elasticsearch-kibana-kerberos/

Hadoop 데몬(NameNode, DataNode, JournalNode, ResourceManager, NodeManager 등)이 동일한 kerberos keytab(`/etc/security/keytabs/spnego.service.keytab`)과 principal(`HTTP/_HOST@{REALM}`)을 공유하기 때문에 발생하는 문제로 추정된다. 특정 상황에서 DataNode가 이를 replay attack으로 오판한다.

다음 JVM 시스템 속성을 Hadoop 데몬에 추가하면 해결된다. Java 프로세스가 replay cache를 사용하지 않게 된다.

```
-Dsun.security.krb5.rcache=none
```
