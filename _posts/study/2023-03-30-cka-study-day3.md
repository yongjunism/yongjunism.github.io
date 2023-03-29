---
layout: post
title: "CKA study Day3"
subtitle: "CKA study Day3"
category: study
tags: CKA
---

## Section 2: Core Concepts

### ReplicaSet
#### Replication controller가 필요한 이유
애플리케이션을 실행하는 단일 파드가 있고, 어떤 이유로 파드가 죽었다고 합시다. 사용자는 더 이상 어플리케이션에 접근할 수 없습니다. 이런 상황에서도 사용자가 접근 권한을 잃지 않도록 동시에 둘 이상의 인스턴스 또는 파드가 있다면, 하나가 죽어도 여전히 다른 파드에 실행중인 애플리케이션이 있습니다.

Replication controller는 클러스터에서 파드의 여러 인스턴스를 실행하는 데 도움이 되고, 곧 고가용성(High Availability)을 제공한다는 뜻입니다. 파드가 하나만 있더라도 그것이 죽었을 때, Replication controller는 정해진 수 만큼의 파드가 동시에 실행되고 있을 것을 보장합니다.

#### Load Balancing & Scaling
Relication controller가 필요한 또 다른 이유는 부하를 분산시키기 위해 여러 파드를 만들어야 하기 때문입니다. 사용자가 많아지면 로드밸런싱을 위해 파드를 더 배포합니다. 그러다가 노드의 리소스가 부족해지면 다른 노드에 파드를 또 배포할 수 있습니다. 이처럼, Replication controller는 여러 노드에 걸쳐있기 때문에 애플리케이션을 확장하는 것 뿐만 아니라 다른 노드에 있는 파드의 로드밸런싱을 하는 데 도움이 됩니다.

#### Replication controller와 ReplicaSet의 차이
- Replication controller가 더 오래된 기술이며, ReplicaSet에 의해 대체되었다.
- ReplicaSet이 새로이 권장되는 방법이다.


### Deployments
앞서 배운 개념들을 내려놓고, 프로덕션 환경에서 애플리케이션을 어떻게 배포하면 좋을까요?
배포해야할 웹 서버가 있고, 많은 인스턴스가 필요하다고 합시다. 그리고 새 버전이 나올 때마다 도커 인스턴스를 원활하게 업그레이드하고 싶습니다. 하지만 한 번에 업그레이드하면 사용자에게 영향을 미칠 수 있으므로 하나씩 업그레이드하고 싶습니다. 이러한 형태의 업그레이드를 롤링 업데이트라고 합니다.
만약 업그레이드 도중에 예기치 못한 에러가 발생하면 최근 변경 사항을 롤백해야 할 것입니다. 또, 여러 가지를 한 번에 업그레이드하기 보다 잠시 중단하고, 변경하고, 재개하는 등 이 모든 것들은 Deployments로 할 수 있습니다.