---
title: cargo/rust 용 Proxy 저장소
date: 2025-06-21T15:13:00+09:00
tags: ["rust", "nexus"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

# Sonatype Nexus Community Edition 사용 예

- 3.77 이상 버전을 써야 CE 버전에서도 cargo repository를 사용할 수 있다. 단, 3.77 버전부터는 사용량 hard limit이 존재한다. 다음 명령으로 간단히 로컬에 구동:

```
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```

- `cargo-proxy`로 repository를 만들었다고 가정, remote URL은 `https://index.crates.io`로 설정한다.

![nexus](/images/computer-science/rust/cargo_repository_proxy/nexus.png)

- `.cargo/config.toml`에 다음과 같이 내용 추가:

```
❯ cat .cargo/config.toml
[registries.nexus]
index = "sparse+http://localhost:8081/repository/cargo-proxy/"

[registry]
default = "nexus"

[source.crates-io]
replace-with = "nexus"

[source.nexus]
registry = "sparse+http://localhost:8081/repository/cargo-proxy/"
```

https://index.crates.io/의 설명을 보면 `sparse` 프로토콜을 사용한다. 위 config.toml에서 `sparse+`를 빼먹으면 정상 동작하지 않는다.

![index_crates_io](/images/computer-science/rust/cargo_repository_proxy/index_crates_io.png)

- 빌드:

```
❯ cargo build
```

참고:
- https://gist.github.com/mashintsev/3e6ab7840d6233ab7932565d056b8158
- https://doc.rust-lang.org/cargo/reference/source-replacement.html
