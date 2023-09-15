---
layout: post
title: "CKA study Day7"
subtitle: "cka study day7"
category: study
tags: CKA
---

커스텀 스케줄러를 사용할 수 있다는 점에서 확장성이 높다.
파드나 deployments를 생성할 때, 쿠버네티스에 특정 스케줄러로 파드를 스케줄하게끔 지시할 수 있다.

my-scheduler-config.yaml
```yaml
apiVersion: kubescehduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler
leaderElection:
  leaderElect: true
  resourceName: kube-system
  resourceName: lock-object-my-scheduler
```
configuration file에서 스케줄러의 이름을 지정할 수 있다.


### Deploy Additional Scheduler
1. kube-scheduler binary를 다운로드한다.
```bash
wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler
```

2. 서비스로 실행한다.
my-scheduler.service
```bash
ExecStart=/usr/local/bin/kube-scheduler \\
  --config=/etc/kubernetes/config/my-scheduler-config.yaml
```

### Deploy Additional Scheduler as  Pod
my-custom-scheduler.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --config=/etc/kubernetes/my-scheduler-config.yaml
    image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```

### Use Custom Scheduler
pod-definition yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  
  schedulerName: my-custom-scheduler


## Section 4: Logging & Monitoring
### Monitor
클러스터 내 노드의 갯수, healthy한 노드의 갯수, CPU/메모리/네트워크/디스크 사용률과 같은 퍼포먼스 지표를 모니터링
Metrics Server, Prometheus, Elastic Stack, Datadog, Dynatrace과 같은 툴들이 있다.

#### Metrics Server
- Heapster가 depreceated되고, slimmed-down된 버전
- 쿠버네티스 클러스터 당 하나씩 있고, 노드와 파드의 메트릭을 인-메모리에 저장하기 떄문에 과거 데이터를 볼 수 없음

노드마다 kube-apiserver로부터 지시를 받아 노드에 파드를 올리는 kubelet에는 cAdvisor라는 컴포넌트가 있다.

## Section 5: Application Lifecycle Management

### Rollout and Versioning
```bash
kubectl rollout status deployment/myapp-deployment

kubectl rollout history deployment/myapp-deployment
```

### Deployment Strategy