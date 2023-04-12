---
layout: post
title: "CKA study Day13"
subtitle: "cka study day13"
category: study
tags: CKA
---

낙오되지 않고 잘 완주할 수 있어야할텐데요 ㅠ
## Section 7: Security
### KubeConfig
```
kubectl get pods
    --server my-kube-playground:6443
    --client-key admin.key
    --client-certificate admin.crt
    --certificate-authority ca.crt
```
위 코드를 매 번 타이핑하는 것은 성가신 일이므로 --이하의 정보들을 kubeconfig라고 부르는 configuration file로 옮깁니다.
그리고 이 파일을 커맨드에서 kubeconfig라는 옵션으로 줄 수 있습니다. 바로 이렇게요.

```
kubectl get pods
    --kubeconfig config
```
기본적으로, kubectl이 HOME/.kube의 하위 디렉토리에 있는 config라는 이름의 파일을 찾습니다.
저 경로에 kubeconfig 파일을 만들었다면 그 경로를 명시적으로 선언할 필요는 없습니다.

kubeconfig 파일은 정해진 양식이 있습니다.
config 파일은 Clusters, Users, Contexts의 세 가지 섹션이 있는데요.
여기서 Clusters는 우리가 접근하는 개발, 테스트, 배포 환경 또는 다른 기관, 클라우드 제공자일 수 있습니다.
Users 는 이 클러스터에 접근하는 사용자 계정입니다. e.g., admin, dev/prod user
Contexts는 어느 사용자 게정이 어느 클러스터에 접근할 때 사용될지를 정의합니다.

이 과정에서 새로운 사용자를 만들거나, 클러스터 내에서 사용자 접근이나 권한을 구성하는 것이 아니라
사용자 인증서와 서버 인증서를 커맨드마다 명시할 필요가 없습니다.

실제 yaml을 다음의 형태를 가집니다.
```yaml
apiVersion: v1
kind: Config

clusters:
- name: my-kube-playground
  cluster:
    certificate-authority: ca.crt
    server: https://my-kube-playground:6443

contexts:
- name: my-kube-admin@my-kube-playground
  context:
    cluster: my-kube-admin
    user: my-kube-admin

users:
- name: my-kube-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

### API Groups
일단 강의부터,,,

### Authorization
정리는 나중에,,,

### Role Based Access Controls(RBAC)
바쁘다 바빠,,,