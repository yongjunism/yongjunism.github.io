---
layout: post
title: "CKA study Day1"
subtitle: "CKA study Day1"
category: study
tags: CKA
---

# Section 2: Core Concepts

## Cluster Architecture
<img src="https://www.redhat.com/rhdc/managed-files/kubernetes_diagram-v3-770x717_0.svg" width="600">
강의에서는 컨테이너를 싣은 배(파드)에 훌륭하게 비유했습니다. 

## etcd
- distributed, reliable key-value store that is simple, secure, and fast
- node, pod, config, secret, account, role, role binding 등 클러스터와 관련된 정보를 저장함
- -etcd-servers 옵션은 etcd 서버의 위치를 명시하고, 이를 통해 etcd 서버와 통신합니다.

## kube-apiserver
kube-apiserver는 쿠버네티스에서 필수적인 주요 컴포넌트들 중 하나죠.<br>
kubectl 명령어를 날릴 때도, kube-apiserver가 요청을 확인하고 검증하는 절차를 수행합니다.<br>
그 다음에 etcd에서 데이터를 받아 원하는 정보를 출력합니다.<br>
kubectl 명령어 뿐만 아니라, POST 요청을 보내서 API를 직접 호출할 수도 있습니다.<br>

## kube-controller-manager
controller의 역할은 다음과 같습니다.<br>
1. continuously monitors the state of various components
2. works towards bringing the whole system to the desired functioning system

k8s에는 수많은 컨트롤러들이 있고, kube-controller manager가 다양한 컨트롤러를 관리합니다.<br>
node controller는 kube-apiserver를 통해 node의 상태를 모니터링하다가 애플리케이션이 잘 동작하기 위한 액션을 수행합니다.<br>
5초마다 노드의 상태를 체크하고, 40초동안 소식이 없으면 가용하지 않은 것으로 간주해서 비로소 5분이 경과했을 때 파드를 축출하고 다른 healthy한 복제 파드에 컨테이너를 올립니다.<br>
replication controller는 replica set의 파드 갯수가 정해진 만큼 가용한지 수시로 확인하고, 하나가 죽으면 다시 띄우는 작업을 반복합니다.<br>
두 컨트롤러 외에도 수많은 다른 컨트롤러들이 있고, kube-controller-manager라는 하나의 프로세스에 집약되어 있습니다.<br>

service를 살펴보면,
node-monitor-period=5s, node-monitor-grace-period=40s<br>
pod-eviction-timeout=5m0s, controllers에 관한 옵션을 상세히 확인할 수 있습니다.<br>
노드 가용성과 관련해서는 나리님이 [포스팅]도 한 번 참고하실 것을 권합니다.

## kube-scheduler
스케줄러는 일정 기준에 따라 어느 노드에 파드를 할당할지 결정합니다. 예를 들어, 리소스 요구량이 서로 다른 파드들이 있을 때, 스케줄러는 각 파드에 맞는 최적의 노드를 찾으려고 합니다.<br>
어떻게? 먼저, 파드와 맞지 않는 노드는 걸러내고, 남은 노드들 중에 할당하고 남은 가용한 리소스가 많은 쪽에 더 높은 랭크를 주는 방식으로요.<br>
이러한 방식은 커스텀해서 쓸 수도 있고, 추후 resource requirements and limites, taints and tolerations, node selectors, affinity rules에서 더 심도 있게 다룰텐데요.<br>
제가 요즘 실무에서 적용하고자 공부하고 있는 부분이기도 해서 조만간 포스팅하도록 하겠습니다.

[포스팅]: https://waspro.tistory.com/777