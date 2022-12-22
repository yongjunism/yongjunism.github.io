---
layout: post
title: "denied: requested access to the resource is denied 해결 방법"
subtitle: "How to solve 'denied: requested access to the resource is denied'"
category: dev
tags: docker 
---

## 상황

docker hub에 이미지를 푸시하려고 할 때, *denied: requested access to the resource is denied* 에러가 발생할 수 있습니다.

## 원인

이유는 크게 두 가지입니다.
1. docker hub에 로그인하지 않았을 경우
2. docker tag를 할 때 user name이 틀렸을 경우


## 해결 방법

1번의 경우, docker hub의 계정이 없다면 [여기]서 회원가입을 하고
docker login을 통해 로그인하시면 됩니다.

2번의 경우에는, 아래의 command를 통해 docker tag를 올바르게 하셨는지 다시 살펴보세요.

```bash
docker tag [image]:[tag] [username]/[target repository]:[tag]
```

[image]:[tag] 부분은 docker images 커맨드로 확인하시면 됩니다.
[username]은 docker hub에서 사용하고 계시는 account 이름,
[target repository]:[tag] 부분은 푸시하실 repo의 이름과 지정하실 tag를 넣으시면 됩니다.

그럼 저는 어떻게 했는지 보실까요?
```bash
docker build -t yj-iris-preprocessing .
```
현재 위치에 yj-iris-preprocessing 이라는 이미지를 빌드합니다.

```bash
docker images
```
를 통해 yj-iris-preprocessing 이라는 이미지가 잘 빌드된 것을 볼 수 있습니다. tag는 latest네요.

그럼 이제
```bash
docker tag yj-iris-preprocessing:latest yongjuncho/yj-iris-preprocessing:0.5
```
로 docker tag를 진행하고

```bash
docker push yongjuncho/yj-iris-preprocessing:0.5
```
푸시를 마무리해주시면 되겠습니다.

[여기]: https://hub.docker.com
