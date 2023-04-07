---
layout: post
title: "CKA study Day10"
subtitle: "cka study day10"
category: study
tags: CKA
---

이제 벌써 섹션 6에 접어들었네요.

## Section 6: Cluster Maintenance

클러스터 유지보수와 관련한 주제를 논의합니다.
- Cluster Upgrading Process
- Operating System Upgrades
- Backup and Restore Methodologies

pod-eviction-timeout은 controller manager에 5분으로 설정되어있다.
5분을 넘기면 기존 파드에 있던 애플레케이션은 사라지고 빈 파드로 올라온다.

노드를 drain하면 pod가 해당 노드에서 정상적으로 종료되고 다른 노드에서 다시 생성된다. 노드는 cordon되거나 unschedulabled로 표시된다.

### Kubernetes Softeware Versions

모든 버그 수정 및 개선 사항은 알파 릴리즈로 먼저 이동한다. 기능이 default로 비활성화돼있고 버그가 있을 수 있다.
코드가 잘 테스튿되고 새 기능이 default로 활성화되는 베타 릴리스로 읻동하고, 마지막으로 메인 stable 릴리스로 이동한다.

### Cluster Upgrade Process

컴포넌트는 다른 릴리즈 버전을 가질 수 있다. kube-apiserver는 컨트롤 플레인의 핵심 컴포넌트므로 다른 컴포넌트들은 이보다 버전이 낮아야 한다.
반면, kubectl 유틸리티는 apiserver보다 높은 버전일 수 있다. 이제는 skew를 통해 업그레이드 가능

업그레이드를 해야할 시기?