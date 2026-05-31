---
title: MySQL 사용 시 Hive Metastore 체크리스트
date: 2023-10-12T08:34:00+09:00
tags: ["hadoop", "hive", "mysql"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

# MySQL Index

Hive Metastore가 RDBMS에 메타데이터를 저장/조회할 때 비용이 큰 작업이 있다.\
다음은 관련 공식 Hive 패치의 인덱스 생성 SQL이다.

```
-- HIVE-21063
CREATE UNIQUE INDEX `NOTIFICATION_LOG_EVENT_ID` ON NOTIFICATION_LOG (`EVENT_ID`) USING BTREE;

-- HIVE-21487
CREATE INDEX COMPLETED_COMPACTIONS_RES ON COMPLETED_COMPACTIONS (CC_DATABASE,CC_TABLE,CC_PARTITION);

-- HIVE-27165
DROP INDEX TAB_COL_STATS_IDX ON TAB_COL_STATS;
CREATE INDEX TAB_COL_STATS_IDX ON TAB_COL_STATS (DB_NAME, TABLE_NAME, COLUMN_NAME, CAT_NAME) USING BTREE;
DROP INDEX PCS_STATS_IDX ON PART_COL_STATS;
CREATE INDEX PCS_STATS_IDX ON PART_COL_STATS (DB_NAME,TABLE_NAME,COLUMN_NAME,PARTITION_NAME,CAT_NAME) USING BTREE;
```

Hive 업그레이드 시 RDBMS 테이블에 변경사항이 있을 수 있다. 버전별 필요한 SQL은 https://github.com/apache/hive/tree/master/standalone-metastore/metastore-server/src/main/sql/mysql 에서 확인 가능하다.

추가로, `NOTIFICATION_LOG` 테이블의 `EVENT_TIME` 컬럼에 대해 slow query가 발견되었다. 공식 패치는 없지만 다음 인덱스를 생성하여 해결했다:

```
CREATE INDEX `NOTIFICATION_LOG_EVENT_TIME` ON NOTIFICATION_LOG (`EVENT_TIME`) USING BTREE;
```

# MySQL 8.x 사용 시

MySQL >= 8.0부터 기본 문자셋과 collation이 `utf8mb4`와 `utf8mb4_0900_ai_ci`로 변경되었다.
Apache Hive에서 관련 패치가 있지만 완전히 해결되지는 않았다. (예: https://issues.apache.org/jira/browse/HIVE-18083)
따라서 Hive 3.1.3과 MySQL >= 8.0을 함께 사용하려면 JDBC URL에 다음과 같은 mysql connection 옵션을 설정하는 것을 권장한다:

```
jdbc:mysql://{your_mysql_host}/{database}?characterEncoding=latin1&connectionCollation=latin1_bin
```

참고:
- https://dev.mysql.com/doc/connector-j/8.1/en/connector-j-reference-charsets.html
- https://dev.mysql.com/doc/connector-j/8.1/en/connector-j-connp-props-session.html#cj-conn-prop_connectionCollation
