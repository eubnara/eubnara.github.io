---
title: '"jute.maxbuffer" 관련 모니터링 지표'
date: 2024-05-25T22:34:00+09:00
tags: ["hadoop", "hive", "zookeeper"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

ZooKeeper 사용 시 "jute.maxbuffer"라는 설정이 있다. ZooKeeper 클라이언트 또는 서버 측에서 설정 가능하며, 클라이언트 측 설정값은 서버 측보다 낮아야 한다.
클라이언트가 이 설정값보다 큰 데이터를 받으면 오류가 발생한다.

관련 이슈:
- https://issues.apache.org/jira/browse/HIVE-21993
- https://issues.apache.org/jira/browse/YARN-2962

이러한 오류를 방지하기 위해 ZooKeeper에서 다음 지표를 모니터링해야 한다.

- `last_client_response_size` 또는 `max_client_response_size`
  - `client_response_size`는 ZooKeeper 서버에서 클라이언트로의 응답 크기(바이트)다.
- `last_proposal_size` 또는 `max_proposal_size`
  - `proposal_size`는 ZooKeeper 서버 리더가 팔로워로 보내는 proposal 크기(바이트)다.
  - `proposal`에 대한 자세한 내용은 https://zookeeper.apache.org/doc/r3.7.1/zookeeperInternals.html 참고.

이 값들은 `jute.maxbuffer`보다 낮아야 한다. 이 설정은 `-Djute.maxbuffer=10485760` (10MB)와 같은 JVM 인자로 설정할 수 있다.
