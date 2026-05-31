---
title: krb5.conf의 "rdns = false"가 Hadoop 생태계에서 동작하지 않는 사례
date: 2023-07-02T18:48:00+09:00
tags: ["hadoop", "kerberos"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

https://web.mit.edu/kerberos/krb5-1.13/doc/admin/princ_dns.html


> 운영 체제 버그로 인해 rdns = false 설정이 reverse DNS lookup을 비활성화하지 못할 수 있다. 일부 GNU libc 버전의 getaddrinfo()는 필요하지 않은 경우에도 PTR 레코드를 조회하는 버그가 있다. MIT Kerberos 릴리스 krb5-1.10.2 이상 및 krb5-1.9.x 시리즈(krb5-1.9.4 이상)에는 이 문제에 대한 해결 방법이 포함되어 있다.

Hadoop 생태계에서 krb5.conf의 "rdns = false"가 적용되지 않는 경우가 있다. `/etc/hosts`를 수정하거나 PTR 레코드를 등록하여 해결할 수 있다.

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

`metastore.thrift.uris` 또는 `hive.metastore.uris`에서 canonical hostname을 확인하는 로직이 있다. 해결된 hostname이 원하는 대로 나오지 않는다면 다음 가지 해결 방법이 있다.

- `/etc/hosts` 수정
- HiveMetastore 서버에 PTR 레코드 등록
- (확실하지 않음) `metastore.uri.resolver` 또는 `hive.metastore.uri.resolver` 시도
  - https://issues.apache.org/jira/browse/HIVE-18347

# 2. KuduClient Java lib

- https://issues.apache.org/jira/browse/KUDU-2032
- https://issues.apache.org/jira/browse/KUDU-2096
- https://issues.apache.org/jira/browse/KUDU-3415

PTR 레코드를 등록하거나 KUDU-3415 해결을 시도한다.

# 3. HBase client

`hbase-site.xml`에 다음 설정을 추가한다:

```
hbase.unsafe.client.kerberos.hostname.disable.reversedns=true
```

- https://github.com/apache/hbase/blob/25455b6fe3cbd8f093fd9bc8c51a1bab95353a62/hbase-client/src/main/java/org/apache/hadoop/hbase/security/provider/GssSaslClientAuthenticationProvider.java#L48
- https://hbase.apache.org/book.html#hbase_default_configurations

![hbase_reversedns](/images/computer-science/hadoop/hbase_reversedns.png)

# 참고

- [https://deview.kr/data/deview/session/attach/1500_T3_이영곤_대용량_멀티테넌트_시큐어_하둡_클러스터_운영_경험기.pdf](https://deview.kr/data/deview/session/attach/1500_T3_이영곤_대용량_멀티테넌트_시큐어_하둡_클러스터_운영_경험기.pdf)
