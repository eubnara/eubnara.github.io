# eubnara.github.io

## installation

- https://hub.docker.com/r/klakegg/hugo
- https://gohugo.io/installation/linux/

```
$ sudo apt install hugo
```

## 로컬 서버 테스트

```
$ cd doc/
$ hugo server --buildDrafts
$ hugo server
```


## 새 문서 작성

https://github.com/adityatelange/hugo-PaperMod/wiki/Installation
https://gohugo.io/getting-started/quick-start/#add-content

```
$ cd doc/
$ hugo new posts/my-first-post.md
$ hugo new computer-science/hadoop/commands.md
```
기본적으로 frontmatter 에 들어가는 기본 template 은 `doc/archetypes/default.md` 에서 변경 가능.


## Publish

```
$ hugo
```

github page 에서 `doc/public` 서빙


## cautions

- Hugo does not clear the `public` directory before building your site.


## todos

- [ ] setup github pages for hugo: https://gohugo.io/hosting-and-deployment/hosting-on-github/