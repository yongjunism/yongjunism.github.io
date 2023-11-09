---
layout: post
title: "validation failure list: page in query must be type of int64: 'NaN'page_size in query must be of type int64: '[object Object]' 에러"
subtitle: "validation failure list: page in query must be type of int64: 'NaN'page_size in query must be of type int64: '[object Object]' error"
category: dev
tags: harbor
---
요즘 Velero로 k8s를 백업하면서 그 중 harbor를 먼저 손대고 있다.

<사진>

harbor는 레지스트리가 전부라고 봐도 무방하기에 개발계에 테스트용 harbor를 구축하고, 기존에 운영하던 harbor에서 간단한 이미지 몇 개만 replicate해서 백업을 진행하는데, 끌어오는 과정에서 위와 같은 에러가 발생한다.

Replication Rule을 설정하고 Replicate를 클릭하면, 제목의 에러가 계속 뜨는데.. 잘 되다가 갑자기 이러니까 심히 당황스럽다.

구글링을 해도 *validation failure list*로 뜨는 이슈로 딱 하나 뿐이고, tag와 관련됐을 수도 있겠다는 생각을 했다.

그래서 docs에 있는 복제 룰도 들춰봤지만 아직 근본적으로 해결하진 못했고 임시방편으로 1.12버전 이상으로 올리니 일단은 잘 복제되는 것을 확인했다.

harbor가 요즘 안되겠다 싶을 정도로 이슈가 많은데 안 그래도 한 술 더 뜨고 있는 느낌..

(그러기엔 운영계 harbor 1.10도 내가 이미지 싹 다 이관했었는데 참으로 이상한 일이다...)
