---
title: "About me"
ShowToc: true
comments: false
---

My name is Yubi Lee.\
I work as a Data Engineer.


- email: eubnara@gmail.com
- github: https://github.com/eubnara
- linkedin: https://kr.linkedin.com/in/yubi-lee-40a158130


---

# Open source contribution

## Hadoop

- [HADOOP-18806](https://issues.apache.org/jira/browse/HADOOP-18806): Document missing property (ipc.server.read.threadpool.size) in core-default.xml
- [HADOOP-18666](https://issues.apache.org/jira/browse/HADOOP-18666): A whitelist of endpoints to skip Kerberos authentication doesn't work for ResourceManager and Job History Server
- [HDFS-16883](https://issues.apache.org/jira/browse/HDFS-16883): fix duplicate field name in hdfs-default.xml
- [HADOOP-18585](https://issues.apache.org/jira/browse/HADOOP-18585): DataNode's internal infoserver redirects with http scheme, not https when https enabled.
- [HADOOP-18398](https://issues.apache.org/jira/browse/HADOOP-18398): Prevent AvroRecord*.class from being included non-test jar
- [HADOOP-18087](https://issues.apache.org/jira/browse/HADOOP-18087): fix bugs when looking up record from upstream DNS servers.
  - When query A record which is chained by CNAME, YARN Registry DNS Server does not properly respond. Some CNAME records are missing.
- [HADOOP-17861](https://issues.apache.org/jira/browse/HADOOP-17861): improve YARN Registry DNS Server qps
- [HDFS-13259](https://issues.apache.org/jira/browse/HDFS-13259): fix file preview bug in NameNode UI
- [HDFS-13260](https://issues.apache.org/jira/browse/HDFS-13260): fix guide about HA with QJM

## Apache Bigtop

- [BIGTOP-3906](https://issues.apache.org/jira/browse/BIGTOP-3906): Wrapper script for hive sources wrong default file
- [BIGTOP-3850](https://issues.apache.org/jira/browse/BIGTOP-3850): fix conflict when installing ranger-hdfs-plugin and ranger-yarn-plugin on the same machine

## Apache Ambari

- [AMBARI-25891](https://issues.apache.org/jira/browse/AMBARI-25891): Enhancements when regenerating keytabs and changing Kerberos configurations
- [AMBARI-25797](https://issues.apache.org/jira/browse/AMBARI-25797): Fail to add a component on the same machine with ambari-server of a new service with no kerberos identity when kerberos enabled
- [AMBARI-25788](https://issues.apache.org/jira/browse/AMBARI-25788): Ambari server keeps generating keytabs even with KerberosServerAction#OperationType.CREATE_MISSING option.
- AMBARI-25624: optimize creating kerberos keytab
  - branch-2.7: https://github.com/apache/ambari/pull/3353
  - trunk: https://github.com/apache/ambari/pull/3588
- [AMBARI-25720](https://issues.apache.org/jira/browse/AMBARI-25720): Support Apache Directory Server
- [AMBARI-25719](https://issues.apache.org/jira/browse/AMBARI-25719): Fix bug when enabling kerberos in Ambari 2.7.6
  - trunk: https://github.com/apache/ambari/pull/3589
- [AMBARI-25619](https://issues.apache.org/jira/browse/AMBARI-25619): improve "Prepare delete identities" process when deleting component in host in kerberized cluster
- [AMBARI-25422](https://issues.apache.org/jira/browse/AMBARI-25422): optimize loading the first page for Ambari UI
- [AMBARI-25491](https://issues.apache.org/jira/browse/AMBARI-25491): newline characters are ignored on custom property in Ambari Web editor

## Apache Ranger

- [RANGER-4418](https://issues.apache.org/jira/browse/RANGER-4418): Upgrade hadoop version and use shaded hadoop client artifacts
- [RANGER-4247](https://issues.apache.org/jira/browse/RANGER-4247): auditPolicyEvaluators should be set before logging it
- [RANGER-4236](https://issues.apache.org/jira/browse/RANGER-4236): enhance Ranger JSON audit to HDFS by compressing as gzip
- [RANGER-4068](https://issues.apache.org/jira/browse/RANGER-4068): fix error caused by missing dnsjava library
- [RANGER-3924](https://issues.apache.org/jira/browse/RANGER-3924): fix unnecessary sync caused by incremeting timestamp value typo
- [RANGER-3858](https://issues.apache.org/jira/browse/RANGER-3858): On dev-support, service creation and ranger-kafka-plugin setup are failed

## Apache Spark

- [SPARK-44976](https://issues.apache.org/jira/browse/SPARK-44976): Utils.getCurrentUserName should return the full principal name
- [SPARK-40964](https://issues.apache.org/jira/browse/SPARK-40964): (TBD) Cannot run spark history server with shaded hadoop jar
- [SPARK-40072](https://issues.apache.org/jira/browse/SPARK-40072): fix build failure when using `make-distributions.sh`

## Apache Impala

- [IMPALA-10408](https://issues.apache.org/jira/browse/IMPALA-10408): Support Apache components to build Apache Impala to reduce dependencies of CDH or CDP
  - https://gerrit.cloudera.org/#/c/18977/

## Cloudera HUE

- [https://github.com/cloudera/hue/pull/1134](https://github.com/cloudera/hue/pull/1134) (not merged): fix bug in handling `+` character in filebrowser
- [https://github.com/cloudera/hue/pull/1135](https://github.com/cloudera/hue/pull/1135): fix bug in substituting `liboozie.remote_deployement_dir` variable

## Apache Zeppelin

- [ZEPPELIN-5594](https://issues.apache.org/jira/browse/ZEPPELIN-5594): HDFS file id should be read as "long", not "int".


## Apache Avro

- [AVRO-3877](https://issues.apache.org/jira/browse/AVRO-3877): [doc] fix wrong configuration for avro-maven-plugin in java example

## ETC

- Spring-shell
  - [https://github.com/spring-projects/spring-shell/pull/432#issuecomment-1144522860](https://github.com/spring-projects/spring-shell/pull/432#issuecomment-1144522860): add a feature to terminate spring-shell with `CTRL+D`
- gotty-client
  - [https://github.com/moul/gotty-client/pull/99](https://github.com/moul/gotty-client/pull/99): fix disconnection caused by not handling `syscall.EINTR`
  - fix basic auth. token base64 encoding
    - [https://github.com/moul/gotty-client/pull/105](https://github.com/moul/gotty-client/pull/105)
    - [https://github.com/moul/gotty-client/pull/108](https://github.com/moul/gotty-client/pull/108)
  - [https://github.com/moul/gotty-client/pull/110](https://github.com/moul/gotty-client/pull/110): get password without revealing it
- react-native-cache
  - [https://github.com/timfpark/react-native-cache/pull/25](https://github.com/timfpark/react-native-cache/pull/25): add a feature to support cache expiration
- discovery-zookeeper
  - [https://github.com/naver/discovery-zookeeper](https://github.com/naver/discovery-zookeeper)
  - elasticsearch plugin to discovery hosts in cluster based on information in znode of Zookeeper
- winstonjs
  - [https://github.com/winstonjs/winston/pull/1559](https://github.com/winstonjs/winston/pull/1559): fix confusing log message

---

# Presentation

- NAVER DEVIEW 2017: Advanced Experiences in Multi-tenant Hadoop Cluster Operation
  - https://deview.kr/2017/schedule/193?lang=en
  - https://www.slideshare.net/deview/234-80881396
  - https://tv.naver.com/v/2297124
- NAVER DEVIEW 2021: Large scale multi-tenant secure Hadoop cluster growth experience sharing
  - https://deview.kr/2021/sessions/459
  - https://deview.kr/data/deview/session/attach/10_%EC%B4%88%EB%8C%80%EC%9A%A9%EB%9F%89%20%EB%A9%80%ED%8B%B0%ED%85%8C%EB%84%8C%ED%8A%B8%20%EC%8B%9C%ED%81%90%EC%96%B4%20%ED%95%98%EB%91%A1%20%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%20%EC%84%B1%EC%9E%A5%ED%86%B5%20%EA%B2%BD%ED%97%98%EA%B8%B0.pdf
  - https://tv.naver.com/v/23650574
