---
description: 'http://www.kerberos.org/software/tutorial.html 페이지를 번역한 내용입니다.'
---

# 목차

1. 소개
2. 목표
3. 구성 요소와 용어의 정의
   1. Realm
   2. Principal
   3. Ticket
   4. Encryption
      1. Encryption type
      2. Encryption key
      3. Salt
      4. Key Version Number \(kvno\)
   5. Key Distribution Center \(KDC\)
      1. Database
      2. Authentication Server \(AS\)
      3. Ticket Granting Server \(TGS\)
   6. Session key
   7. Authenticator
   8. Replay Cache
   9. Credential Cache
4. Kerberos Operation
   1. Authentication Server Request \(AS\_REQ\)
   2. Authentication Server Reply \(AS\_REP\)
   3. Ticket Granting Server Request \(TGS\_REQ\)
   4. Ticket Granting Server Replay \(TGS\_REP\)
   5. Application Server Request \(AP\_REQ\)
   6. Pre-Authentication \[4.6 Application Server Replay \(AP\_REP\) missing\]
5. Ticket 자세히 살펴보기
   1. Initial tickets
   2. Renewable tickets
   3. Forwardable tickets
6. Cross Authentication
   1. Direct trust relationships
   2. Transitive trust relationships
   3. Hierarchical trust relationships

