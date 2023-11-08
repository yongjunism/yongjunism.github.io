---
layout: post
title: "denied: requested access to the resource is denied 해결 방법"
subtitle: "How to solve 'denied: requested access to the resource is denied'"
category: dev
tags: docker
---
docker hub에 이미지를 푸시하려고 할 때, *denied: requested access to the resource is denied* 에러가 발생할 수 있습니다.

## 원인

이유는 크게 두 가지입니다.

1. docker hub에 로그인하지 않았을 경우
2. docker tag를 할 때 username이 틀렸을 경우

## 해결 방법

1번의 경우, docker hub의 계정이 없다면 [여기][여기]서 회원가입을 하고
docker login을 통해 로그인합니다.

2번의 경우에는, docker tag를 올바르게 했는지 확인합니다.

```bash
docker tag [image]:[tag] [username]/[repository]:[tag]
```

[image]:[tag] 는 docker images 커맨드로 확인합니다.
[username] 은 docker hub에서 사용하고 있는 account 이름,`<br>`
[repository]:[tag] 은  push할 repo명과 명시할 버전 tag를 넣어줍니다.



---



```bash
docker build -t yj-iris-preprocessing .
```

현위치를 의미하는 .을 빠뜨리지 않도록 주의하고, yj-iris-preprocessing 이라는 이미지를 빌드합니다.

docker images를 통해 yj-iris-preprocessing 이라는 이미지가 잘 빌드된 것을 볼 수 있습니다.

tag는 따로 명시하지 않아서 latest로 만들어주네요.


```bash
docker tag yj-iris-preprocessing:latest yongjuncho/yj-iris-preprocessing:0.5
```

docker tag를 하고


```bash
docker push yongjuncho/yj-iris-preprocessing:0.5
```

이렇게 push까지 해주면 docker hub에 이미지가 잘 올라간 것을 확인할 수 있습니다.

[여기]: https://hub.docker.com
