---
layout: post
title: "denied: requested access to the resource is denied 해결 방법"
subtitle: "How to solve 'denied: requested access to the resource is denied'"
category: docker-kubernetes
tags: docker 
---

docker hub에 이미지를 푸시하려고 할 때 *denied: requested access to the resource is denied* 에러가 발생할 수 있습니다.

이유는 크게 두 가지입니다.
1. docker hub에 로그인하지 않았을 경우
2. user name과 docker hub의 id가 일치하지 않았을 경우

1번의 경우, docker hub의 계정이 없다면 회원가입을 하고
```bash
docker login
```
command로 로그인하면 됩니다.

2번의 경우에는,