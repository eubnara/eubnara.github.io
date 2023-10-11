---
title: Checklist for hive metastore when using mysql
date: 2023-10-12T08:34:00+09:00
tags: ["hadoop", "hive", "mysql"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

# MySQL Index

There are some expensive operations for hive metastore when accessing or storing metadatas on RDBMS.\
Here are some official hive patches for indexing.

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

When you upgrade your hive, there are some changes on tables in rdbms. You can find needed SQLs depending on version at https://github.com/apache/hive/tree/master/standalone-metastore/metastore-server/src/main/sql/mysql.

Additionally, slow query has been found for column `EVENT_TIME` on table `NOTIFICATION_LOG`. There is no official patch related to this but I create an index as follows:

```
CREATE INDEX `NOTIFICATION_LOG_EVENT_TIME` ON NOTIFICATION_LOG (`EVENT_TIME`) USING BTREE;
```


# When using MySQL 8.x

Since MySQL >= 8.0, `utf8mb4` and `utf8mb4_0900_ai_ci` are used as default character and collation.\
On apache hive, there are some related patches but not fully resolved. (e.g. https://issues.apache.org/jira/browse/HIVE-18083)\
Therefore, if you want to use MySQL >= 8.0 with Hive 3.1.3, I advise you to set mysql connection options on jdbc url like this:

```
jdbc:mysql://{your_mysql_host}/{database}?characterEncoding=latin1&connectionCollation=latin1_bin
```

Refer to:
- https://dev.mysql.com/doc/connector-j/8.1/en/connector-j-reference-charsets.html
- https://dev.mysql.com/doc/connector-j/8.1/en/connector-j-connp-props-session.html#cj-conn-prop_connectionCollation
