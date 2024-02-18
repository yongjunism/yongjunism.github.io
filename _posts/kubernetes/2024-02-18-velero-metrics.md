---
layout: post
title: "Velero가 제공하는 메트릭의 종류"
subtitle: "Velero metrics"
category: dev
tags: kubernetes prometheus grafana velero
image:
  path: /assets/img/2023-12-10/velero_img1.png
---

 Grafana에 대시보드를 띄우려면 Velero 메트릭에 어떤게 있는지 알아야겠죠. 
 Velero 공식 문서가 다소 친절하지 않아 제공하는 메트릭의 모든 종류를 정리해보았습니다.

```
- velero_backup_attempt_total
- velero_backup_deletion_attempt_total
- velero_backup_deletion_failure_total
- velero_backup_deletion_success_total
- velero_backup_duration_seconds_bucket
- velero_backup_duration_seconds_count
- velero_backup_duration_seconds_sum
- velero_backup_failure_total
- velero_backup_items_errors
- velero_backup_items_total
- velero_backup_last_status
- velero_backup_last_successful_timestamp
- velero_backup_partial_failure_total
- velero_backup_success_total
- velero_backup_tarball_size_bytes
- velero_backup_total
- velero_backup_validation_failure_total
- velero_backup_warning_total
- velero_csi_snapshot_attempt_total
- velero_csi_snapshot_failure_total
- velero_csi_snapshot_success_total
- velero_restore_attempt_total
- velero_restore_partial_failure_total
- velero restore_success_total
- velero_restore_total
- velero_restore_validation_failed_total
- velero_volume_snapshot_attempt_total
- velero_volume_snapshot_failure_total
- velero_volume_snapshot_success_total
```

제공하는 메트릭이 참 다양합니다. 메트릭의 이름도 딱 직관적이어서 좋습니다.
만약 위 메트릭이 전부 뜨지 않는다면, 아직 백업을 한 번도 수행하지 않아서 메타데이터에 남지 않아서 그렇습니다.
manual하게든 schedule 백업이든 오브젝트 스토리지에 흔적을 만들어주세요.

제게 쓸만한 메트릭은 다음 6가지 정도였습니다.
- velero_backup_failure_total -> 완전히 실패한 백업이 있는지
- velero_backup_last_status -> 가장 최근 백업 파일의 상태가 온전한지
- velero_backup_last_successful_timestamp -> 가장 최근에 백업이 언제 성공했는지
- velero_backup_partial_failure_total -> 부분 실패한 백업이 있는지
- velero_backup_success_total -> 백업이 잘 성공했는지
- velero_backup_tarball_size_bytes -> 백업 파일의 용량은 어떠한지

백업은 어디까지나 유사시를 대비하는 것이므로 total이 가지는 의미가 무색한 경우도 있습니다. 
특정 시점으로 되돌릴 것이 아니라면 가장 최근의 백업에만 관심을 가지는 경우도 있을 것 같아요.
저는 가장 최근의 부분 실패에 초점을 맞춰서 그 이전 백업과의 증분으로 가장 최근 백업이 성공했는지 확인합니다.
가령, increase(velero_backup_partial_failure_total{schedule!=""}[1d])
으로 promQL을 작성할 수 있습니다.

또한, 메트릭을 조합해 새로운 관점으로 바라보는 것도 좋습니다. 
예를 들어, velero_backup_failure_total{schedule!=""} / velero_backup_attempt_total{schedule!=""}
라고 promQL을 작성하면 백업 실패율을 나타낼 수 있습니다.
스냅샷을 남기는 과정에서 많은 에러가 발생하는 Velero인 만큼, 어떤 백업이 문제가 많은지 확인할 수 있어요.

하지만 이렇게 많은 메트릭을 제공함에도 불구하고 아쉬운 점은 있었습니다.
백업이 실패한 시점과 실패한 백업 파일의 이름 등을 테이블 형태로 보여주고 싶었으나 메트릭으로 제공하지 않습니다. 
나아가, 백업이 실패한 원인도 알 수 있으면 좋은데 모두 로그에서만 확인할 수 있는 내용들이었습니다.
커스텀 로그 수집기를 이용하면 가능하지만 저는 공식 오픈소스만을 활용하는 쪽으로 결정했습니다.

그래서 다음 글에서는 alertManager를 통해 MS Teams로 alert을 받아보는 방법을 다뤄볼까 합니다.
이렇게 하면 alert이 날라오는 시간을 백업이 실패한 시점으로 갈음할 수도 있고, 
prometheusRule에 조건을 설정하여 특정 상황에 보다 빠르게 대응할 수 있는 장점이 있겠네요!