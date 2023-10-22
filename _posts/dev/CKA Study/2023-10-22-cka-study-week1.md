# CKA Study Week 1-1

## 쿠버네티스 아키텍쳐 이해

### 문제1. 쿠버네티스 클러스터 정보 보기

-console에 user 계정으로 로그인한 후 hk8s 클러스터로 이동하시오.

```
$ kubectl config use-context hk8s
```

-hk8s 클러스터의 control-plane, worker node의 이름을 찾아서 /var/CKA2022/hk8s-node-info.txt 파일로 저장하시오.

```kubectl
$ kubectl get nodes | cut -d' ' -f1 | grep -v NAME > /var/CKA2022/hk8s-node.info.txt
```

-hk8s 클러스터에서 ready 상태인 노드의 이름만 추출하여 /var/CKA2022/hk8s-node-ready.txt 파일에 저장하시오.

```
$ kubectl get nodes | grep -i -w ready | cut -d' ' -f1 > /var/CKA2022/hk8s-node-ready.txt
```

### 문제2. 멀티 클러스터 정보 보기

-현재 클러스터를 확인하시오.

```
$ kubectl config current-context
```

-k8s 클러스터로 이동하시오.

```
$ kubectl config use-contesxt k8s
```

-k8s 클러스터 상태를 확인합니다.

```
$ kubectl cluster-info
```

-k8s 클러스터에서 동작중인 모든 CNI 이름을 /var/CKA2022/k8s_cni_name.txt에 저장하시오.

```
$ ssh k8s-master //패스워드 인증 없이 로그인 가능
$ ls /etc/cni/net.d/
>> 10-flannel.conflist
$ echo "flannel" > /var/CKA2022/k8s_cni_name.txt

# 다음 방법으로도 확인할 수 있다.
$ ps -ef | grep cni
$ ps -ef | grep flannel 
```

-k8s 클러스터에서 ready 상태인 노드 이름을 추출하여 /var/CKA2022/k8s_node_ready.txt에 저장하시오.

```
$ kubectl get nodes | grep -i -w ready | cut -d' ' -f1 > /var/CKA2022/k8s_node_ready.txt
```
