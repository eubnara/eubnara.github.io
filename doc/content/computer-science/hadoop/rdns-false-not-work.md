---
title: Some cases where "rdns = false" in krb5.conf does not work in Hadoop ecosystem
date: 2023-07-02T18:48:00+09:00
tags: ["hadoop", "kerberos"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

https://web.mit.edu/kerberos/krb5-1.13/doc/admin/princ_dns.html


> Operating system bugs may prevent a setting of rdns = false from disabling reverse DNS lookup. Some versions of GNU libc have a bug in getaddrinfo() that cause them to look up PTR records even when not required. MIT Kerberos releases krb5-1.10.2 and newer have a workaround for this problem, as does the krb5-1.9.x series as of release krb5-1.9.4.


There are some cases where "rdns = false" in krb5.conf is not respected in Hadoop ecosystem. You can try to modify `/etc/hosts` or register PTR records to fix this kind of issues.

# 1. HiveMetaStoreClient

https://github.com/apache/hive/blob/rel/release-3.1.3/standalone-metastore/src/main/java/org/apache/hadoop/hive/metastore/HiveMetaStoreClient.java#L246

```java
if (uriResolverHook != null) {
  metastoreURIArray.addAll(uriResolverHook.resolveURI(tmpUri));
} else {
  metastoreURIArray.add(new URI(
          tmpUri.getScheme(),
          tmpUri.getUserInfo(),
          HadoopThriftAuthBridge.getBridge().getCanonicalHostName(tmpUri.getHost()),
          tmpUri.getPort(),
          tmpUri.getPath(),
          tmpUri.getQuery(),
          tmpUri.getFragment()
  ));
}
```

There is some logic to resolve canonical hostname from `metastore.thrift.uris` or `hive.metastore.uris`. If the hostname resolved is not you wanted, you can 3 workarounds.

- Modify `/etc/hosts`
- Register PTR record for HiveMetastore servers
- (Not sure) Try `metastore.uri.resolver` or `hive.metastore.uri.resolver`.
  - https://issues.apache.org/jira/browse/HIVE-18347



# 2. KuduClient java lib

- https://issues.apache.org/jira/browse/KUDU-2032
- https://issues.apache.org/jira/browse/KUDU-2096
- https://issues.apache.org/jira/browse/KUDU-3415

Register PTR records or try to fix KUDU-3415.


# 3. HBase client

```
hbase.unsafe.client.kerberos.hostname.disable.reversedns=true
```

Set the configuration above in `hbase-site.xml`.

- https://github.com/apache/hbase/blob/25455b6fe3cbd8f093fd9bc8c51a1bab95353a62/hbase-client/src/main/java/org/apache/hadoop/hbase/security/provider/GssSaslClientAuthenticationProvider.java#L48
- https://hbase.apache.org/book.html#hbase_default_configurations

![hbase_reversedns](images/computer-science/hadoop/hbase_reversedns.png)


# References

- https://deview.kr/data/deview/session/attach/1500_T3_이영곤_대용량_멀티테넌트_시큐어_하둡_클러스터_운영_경험기.pdf

