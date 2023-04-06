---
layout: post
title: "CKA study Day9"
subtitle: "cka study day9"
category: study
tags: CKA
---

 ### 멀티 컨테이너 파드 생성
 pod-definition.yaml
 ```yaml
 apiVersion: v1
 kind: Pod
 metadata:
   name: simple-webapp
   labels:
     name: simple-webapp
spec:
  containers:
  - name: simple-webaapp
    image: simple-webapp
    ports:
      - containerPort: 8080
  - name: log-agent
    image: log-agent
```

### Multi-container Pods Design Patterns
3 common patterns
- Sidecar
- Adapter
- Ambassador
CKAD 범위의 내용이지만 다음 [아티클]에 잘 정리되어있으니 참고하면 좋겠다.

### InitContainers
컨테이너에서 완료되는 프로세스를 실행하고 싶을 때가 있다. 예를 들어, 메인 웹 애플리케이션에서 사용될 레포지토리에서 코드 또는 바이너리를 가져오는 경우.
이는 파드가 처음 생성될 때 한 번만 실행되는 작업이다. 또는 실제 애플리케이션이 시작되기 전에 외부 서비스나 데이터베이스가 작동하길 기다리는 프로세스가 있다.
이것들이 InitContainers가 필요한 이유이다.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application>; done;']
```


[아티클]: https://matthewpalmer.net/kubernetes-app-developer/articles/multi-container-pod-design-patterns.html