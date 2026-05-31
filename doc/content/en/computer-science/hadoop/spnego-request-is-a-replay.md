---
title: SPNEGO-enabled Hadoop DataNode misjudges kerberos "replay attack".
date: 2023-02-05T16:01:17+09:00
tags: ["hadoop", "spnego"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---



- references
    - [https://docs.cloudera.com/cloudera-manager/7.5.5/security-troubleshooting/cm-security-troubleshooting.pdf](https://docs.cloudera.com/cloudera-manager/7.5.5/security-troubleshooting/cm-security-troubleshooting.pdf)
    - [https://search-guard.com/elasticsearch-kibana-kerberos/](https://search-guard.com/elasticsearch-kibana-kerberos/)

I guess that this is caused by sharing the same kerberos keytab (`/etc/security/keytabs/spnego.service.keytab`) and principal(`HTTP/_HOST@{REALM}`) among Hadoop daemons (NameNode, DataNode, JournalNodes, ResourceManager, NodeManager ...). I assume that DataNode misjudges it is a replay attack in certain circumstances.

Adding the following jvm system properties to Hadoop daemons will fix this issue as a workaround. It means java process will not use replay cache.

```
-Dsun.security.krb5.rcache=none
```
