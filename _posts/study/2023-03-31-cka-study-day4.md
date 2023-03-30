---
layout: post
title: "CKA study Day4"
subtitle: "CKA study Day4"
category: study
tags: CKA
---

### Service
#### 내부 네트워크
Service는 애플리케이션 안팎에서 컴포넌트끼리 통신할 수 있게 합니다. 또한, 애플리케이션을 다른 어플리케이션이나 사용자와 연결하는 데 도움을 줍니다. 예를 들어, 애플리케이션이 프론트엔드를 사용자에게 서빙하는 퍄드 그룹, 백엔드 프로세스를 실행하는 파드 그룹, 외부 데이터 소스에 연결하는 파드 그룹이 있다고 합시다. 이 파드 그룹 간 연결을 가능하게 하는 것이 Service입니다. 프론트엔드 애플리케이션을 엔드 유저가 사용할 수 있게 하고, 백엔드와 프론트엔드 파드 간 통신을 돕고, 외부 데이터 소스로의 연결에 도움을 줍니다. 즉, Service는 애플리케이션의 마이크로서비스 간에 느슨한 결합(loose coupling)을 가능하게 합니다.

#### 외부 네트워크
쿠버네티스 노드의 IP 주소가 192.168.1.2<br>
제 랩탑이 같은 네트워크에 있고, IP 주소가 192.168.1.10<br>
내부 파드 네트워크 범위가 10.244.0.0<br>
파드의 IP 주소가 10.244.0.2 라고 합시다.<br>

이 경우, 나는 분리된 네트워크에 있으므로 파드에 ping을 날리거나 접근할 수 없습니다. 어떻게 하면 웹페이지를 볼 수 있을까요? 쿠버네티스 노드에 SSH로 연결하면 curl을 날려서 파드의 웹페이지에 접속할 수 있습니다. 또는, 노드에 GUI가 있다면 브라우저를 열어서 http://10.244.0.2로 웹페이지를 볼 수 있습니다. 그러나 이것은 쿠버네티스 노드 내부에서 접근하는 방법이고, 내 랩탑으로 SSH 연결 없이 쿠버네티스 노드의 IP로 웹 서버에 접속하고 싶다면 중간에 랩탑에서 노드로, 노드에서 웹 컨테이너가 있는 파드로 매핑하는 것을 도와줄 무언가가 필요합니다. 이 역할을 하는 것이 Service입니다. Service는 파드, ReplicaSet, deployments와 마찬가지로 쿠버네티스 오브젝트입니다.

### Service Types
1. NodePort
2. ClusterIP
3. LoadBalancer 

### ClusterIP
```yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP
  ports:
  - targetPort: 80
    port: 80
  selector:
    app: myapp
    type: back-end
```

```bash
kubectl create -f service-definition.yaml
```

```bash
kubectl get services
```

### LoadBalancer
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  ports:
  - targetPort: 80
    port: 80
    nodePort: 30008
```