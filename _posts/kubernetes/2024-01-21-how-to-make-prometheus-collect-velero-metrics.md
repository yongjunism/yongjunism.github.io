---
layout: post
title: "프로메테우스로 Velero 메트릭 수집하기"
subtitle: "How to make Prometheus collect Velero metrics"
category: dev
tags: kubernetes prometheus velero
image:
  path: /assets/img/2023-12-10/velero_img1.png
---

Velero를 구축하니 백업이 잘 되었는지 대시보드 형태로 확인하고 싶은 충동이 생긴다면 지극히 정상입니다.
매번 번거롭게 노드에서 *velero backup get*으로 백업 파일이 잘 생성되었는지 보는 것은 너무 manual한 작업이거든요.

그래서 이번 글은 Prometheus가 velero metric을 수집하는 방법을 다뤄볼까 합니다.
이어질 글에서는 Velero metric에 어떤 것들이 있는지 알아보고, Grafana에 대시보드로 나타내는 법을 살펴보도록 할게요.

제가 구성한 환경에서는 Prometheus를 클러스터에 두지 않고 별도 VM에 구축되어 있습니다.
그래서 이 글을 보고 따라하시는 분들은 VM <-> 클러스터 노드 간의 방화벽도 열어주는 과정이 선행되어야 합니다.

큰 틀로 보면 순서는 다음과 같습니다.
1. VM <-> 클러스터 노드 간의 방화벽 오픈(생략)
2. 헬름의 metric 관련 enabled 설정 true로 변경
3. 8085번 port로 velero metric을 push하는 서비스가 잘 생성되었는지 확인
4. hostPort 설정
5. Prometheus 설정(scrape_config 및 target.d)
6. Grafana에서 대시보드 구성

## 헬름 차트의 metric 관련 enabled 설정 변경
Velero의 metric을 서비스 형태로 내보낼 수 있게 헬름 차트의 values.yaml의 일부를 다음과 같이 수정합니다.
```yaml
metrics:
  enabled: true
  nodeAgentPodMonitor:
    enabled: true
  prometheusRule:
    enabled: true
  serviceMonitor:
    enabled: true
```

## velero metric을 8085 port로 잘 push하고 있는지 확인
<http-monitoring 사진 추가>
배포하고 나면 http-monitoring이라는 이름으로 8085 port로 서비스하고 있음을 확인할 수 있습니다.

## Prometheus 설정
Prometheus가 어디를 바라보면서, 얼마만큼의 주기로 metric을 수집할 것인지 설정합니다.
/usr/local/prometheus/target.d에 yml 파일을 생성하고, /usr/local/prometheus.yml에 scrape_config를 추가해줍시다.
```yaml
$ vi /usr/local/prometheus/target.d/mgr-velero.yml
- targets:
  - '<loadbalancer-ip>:8085'
  labels:
    host: <loadbalancer-name>
    services: prod
    cluster: mgr
    alert: true
```

```yaml
$ vi /usr/local/prometheus/prometheus.yml
scrape_configs:
  - job: "mgr-velero"
    file_sd_configs:
      - files:
        - /usr/local/prometheus/target.d/mgr-velero.yml
```
## hostPort 설정
저는 nginx-ingress-controller에 8085를 hostPort로 설정하여 외부에 노출시켰습니다.
환경에 따라 nodePort로 port-forward해서 쓸 수도 있겠죠. 일반적으로 hostPort로 노출하는 것은 권장하지 않습니다.
```yaml
apiVersion: apps/v1
kind: DaemonSet
spec:
  template:
    spec:
      name: controller
      ports:
      - containerPort: 8085
        hostPort: 8085
        name: prod-velero
        protocol: TCP
```
컨피그맵에도 노출시키고 있는 TCP 서비스의 정보를 넣어줍니다.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: tcp-services
  namespace: ingress-nginx
data:
  "8085": velero/prod-velero:8085
```

<사진>
이제 Prometheus에서 위 사진처럼 Velero metric을 잘 수집하고 있습니다.
 
이어질 포스팅에서는 Velero가 가지는 metric에 어떤 것들이 있는지 알아보고, metric을 이용하여 Grafana 대시보드를 어떻게 구성헀는지 살펴봅시다.