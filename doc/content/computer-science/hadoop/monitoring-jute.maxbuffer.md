---
title: Monitoring metrics related to "jute.maxbuffer"
date: 2024-05-25T22:34:00+09:00
tags: ["hadoop", "hive", "zookeeper"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---


There is a configuration named as "jute.maxbuffer" when using zookeeper. This can be set on zookeeper client side or server side. On zookeeper client side, the setting should be lower than that on zookeeper server.
If a client gets data bigger than the setting, it will get an error.

There are some related issue.

- https://issues.apache.org/jira/browse/HIVE-21993
- https://issues.apache.org/jira/browse/YARN-2962



In order to avoid this errors. Some metrics should be monitored on zookeeper.

- `last_client_response_size` or `max_client_response_size`
    - `client_response_size` is a response size in bytes from zookeeper server to client.
- `last_proposal_size` or `max_proposal_size`
    - `proposal_size` is a proposal size in bytes from zookeeper server leader to follower.
    - `proposal`?: refer to https://zookeeper.apache.org/doc/r3.7.1/zookeeperInternals.html.

These values should be lower than `jute.maxbuffer`.
