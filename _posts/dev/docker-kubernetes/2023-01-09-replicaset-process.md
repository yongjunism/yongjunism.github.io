---
layout: post
title: "denied: requested access to the resource is denied 해결 방법"
subtitle: "How to solve 'denied: requested access to the resource is denied'"
category: dev
tags: docker 
---

Pod만 단독으로 올리면 어떤 문제로 Pod이 죽었을 때 정상적으로 서비스가 잘 이루어지지 않겠죠.
그래서 Pod이 정해진 수만큼 유지될 수 있게 관리하는 것이 Replicaset입니다.

Replicaset을 만드는 yaml 파일은 다음과 같습니다.
```yaml
apiVersion: apps/v1
kind: Replicaset
metadata:
  name: 
```

Replicaset은 label을 체크해서 
### 스케일 아웃



실제 개발을 할 때 Replicaset을 단독으로 쓰는 경우는 거의