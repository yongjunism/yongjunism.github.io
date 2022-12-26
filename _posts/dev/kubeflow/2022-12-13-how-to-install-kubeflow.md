---
layout: post
title: "Mac에서 kubeflow 설치하기"
subtitle: "How to install kubeflow on mac"
category: dev
tags: minikube mac
---

회사에서 온프레미스에 kubeflow를 설치하는 과정에서 당장 진행할 수 없는 부분이 있어서
Mac 환경에서 kubeflow를 버전을 테스트해보았습니다.


전체적인 흐름은 다음과 같습니다.
1. Kubeflow Component와 K8s의 버전 호환성 확인
2. Docker Desktop 설치
3. Minikube 설치
4. kubectl, 
5. Kustomize setting
6. 

## kubeflow, k8s, kustomize
가장 먼저, kubeflow 컴포넌트와 K8s의 버전이 잘 호환되는지 확인합니다.
k8s 1.20, 1.21 버전은 kubeflow 1.5 버전, k8s 1.22 버전은 kubeflow 1.6 버전과 호환됩니다.

저는 k8s 1.22.17, kubeflow 1.61, kustomize 3.2.0을 설치했습니다.
kustomize 4 버전대는 아직 호환되지 않으니 3.2.0을 권장합니다.
docker 20.10.20

docker desktop이 설치되어 있으면 minikube에서 docker driver를 사용할 수 있습니다.
minikube는 기본적으로 hyperkit을 제공하는데, docker로 지정해서 구동하는게 더 편리하더라고요.


## kubectl 설치
```bash
brew install kubernetes-cli (또는 kubectl)
```

## minikube 설치
```bash
brew install minikube
```
