---
title: "About me"
ShowToc: true
---

My name is Yubi Lee.\
I work as a Data Engineer.


- github: https://github.com/eubnara
- linkedin: https://kr.linkedin.com/in/yubi-lee-40a158130


---

# Open source contribution

## Hadoop

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

- [RANGER-4068](https://issues.apache.org/jira/browse/RANGER-4068): fix error caused by missing dnsjava library
- [RANGER-3924](https://issues.apache.org/jira/browse/RANGER-3924): fix unnecessary sync caused by incremeting timestamp value typo
- [RANGER-3858](https://issues.apache.org/jira/browse/RANGER-3858): On dev-support, service creation and ranger-kafka-plugin setup are failed

## Apache Spark

- [SPARK-40964](https://issues.apache.org/jira/browse/SPARK-40964) (TBD): Cannot run spark history server with shaded hadoop jar
- [SPARK-40072](https://issues.apache.org/jira/browse/SPARK-40072): fix build failure when using `make-distributions.sh`

## Apache Impala

- [IMPALA-10408](https://issues.apache.org/jira/browse/IMPALA-10408): Support Apache components to build Apache Impala to reduce dependencies of CDH or CDP
  - https://gerrit.cloudera.org/#/c/18977/

## Cloudera HUE

- [https://github.com/cloudera/hue/pull/1134](https://github.com/cloudera/hue/pull/1134) (not merged): fix bug in handling `+` character in filebrowser
- [https://github.com/cloudera/hue/pull/1135](https://github.com/cloudera/hue/pull/1135): fix bug in substituting `liboozie.remote_deployement_dir` variable


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
