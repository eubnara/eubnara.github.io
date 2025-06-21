---
title: Proxy repository for cargo/rust
date: 2025-06-21T15:13:00+09:00
tags: ["rust", "nexus"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---


# Sonatype Nexus community edition 사용 예

- 3.77 이상의 버전을 써야 ce 버전에서도 cargo repository 를 쓸 수 있다. 단, 3.77 버전부터는 사용량 hard limit 이 존재하게 된다. 다음 명령으로 간단히 로컬에 구동
```
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```

- `cargo-proxy` 로 repository 를 만들었다고 가정, remote url 은 `https://index.crates.io` 로 한다.

![nexus](/doc/static/images/computer-science/rust/cargo_repository_proxy/nexus.png)


- `.cargo/config.toml` 에 다음과 같이 내용 추가.
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

https://index.crates.io/ 에 써있는 설명을 보니 `sparse` 프로토콜을 쓰는 것 같다. 위에 config.toml 에서도 `sparse+` 를 빼먹으면 정상동작하지 않는다.
![index_crates_io](/doc/static/images/computer-science/rust/cargo_repository_proxy/index_crates_io.png)

- 빌드
```
❯ cargo build
```


참고

- https://gist.github.com/mashintsev/3e6ab7840d6233ab7932565d056b8158
- https://doc.rust-lang.org/cargo/reference/source-replacement.html
