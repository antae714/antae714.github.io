---
title: "엄폐시스템 제작"
date: 2025-09-09 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "소개"
AutoContents: false
---



{% capture paragraph %}
## **엄폐 시스템 컴포넌트**
TPS 캐릭터를 위한 **엄폐 시스템**을 **컴포넌트** 형태로 제작했습니다.  
**엄폐물**에 붙었을 때 캐릭터의 **위치**와 **회전**을 엄폐물에 맞추어 조정하며,  
플레이어 **입력**을 가공하여 엄폐 중 적절한 **방향 이동**이 가능하도록 설계했습니다.

엄폐 상태에서는 현재 엄폐물의 **노멀 벡터**를 기준 **좌·우 이동**으로 제한합니다.  
엄폐물이 **끊기는 지점**에서는 이동이 멈추지만, **다음 면**이 연결되어 있다면 **버튼을 길게 눌러 이동**할 수 있도록 구현했습니다.  
또한 엄폐물의 **높이**가 캐릭터 **가슴**보다 낮다면 자동으로 **엄폐/웅크리기 상태**를 전환합니다.

![사진]({{ '/assets/MyLittleStorage/Cover.gif' | relative_url }}){: style="width: 100%; height: auto;" } 

<br>

### 피킹
사격 시에는 **`EPeekingState`**를 통해 **피킹 상태**를 결정합니다.  
**`UWeaponComponent::IsWeaponBlocking`** 함수를 제작하여, 무기가 엄폐물에 **가려지면 사격을 제한**하고 **피킹을 해제**하도록 했습니다.

![사진]({{ '/assets/MyLittleStorage/Cover2.gif' | relative_url }}){: style="width: 100%; height: auto;" }

![사진]({{ '/assets/MyLittleStorage/Cover3.gif' | relative_url }}){: style="width: 100%; height: auto;" }


플레이어가 입력을 주었을 때는 화면 중앙의 엄폐물로  
**`UAIBlueprintHelperLibrary::SimpleMoveToLocation`**을 이용해 이동한 뒤,  
도착 시 **엄폐 상태**로 전환합니다. 이 과정에서  
**`UNavigationSystemV1::FindPathSync`**로 탐색한 **경로**를 **UI**에 표시해 **시각적 피드백**을 주었습니다.  


![사진]({{ '/assets/MyLittleStorage/CoverNavigation.png' | relative_url }}){: style="max-width: 100%; height: auto;" } 

<br>

플레이어뿐만 아니라 **AI**도 동일한 방식으로 **엄폐**를 수행할 수 있도록 확장했습니다.

<br>

### **되돌아보기**
언리얼 **학습**과 병행하며 제작한 만큼 **아쉬운 점**이 있습니다.  
엄폐 이동 로직을 **컴포넌트**로 분리하기보다는 **CharacterMovement 상속**으로 제작했으면 더 자연스러웠을 것 같습니다.  
또한 **AI**와 **플레이어**의 엄폐를 동일한 컴포넌트로 처리했는데,  
비슷한 기능이라도 **목적과 동작**이 다른 만큼  
**플레이어 전용 / AI 전용 컴포넌트 분리**가 더 나은 구조였다고 느꼈습니다.
{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **짐작한 댓가**
엄폐 시스템을 설계할 당시, "**캐릭터가 엄폐물에 붙으면 된다**"라는 **단순 가정**으로 구조를 만들었습니다.  
하지만 제작이 끝날 무렵, **기획자 의도**와는 큰 차이가 있었습니다.  
결국 기존 구조로는 **기획 의도 구현 불가**, 시스템을 **처음부터 다시 설계**해야 했습니다.

이 경험을 통해 **"짐작하지 말고 반드시 질문해야 한다"**는 점을 배웠습니다.  
질문을 통해 **기획 의도**를 명확히 이해할 뿐만 아니라, **더 나은 방향 제시**와 **협업 기회**도 얻을 수 있습니다.  
또한 **주기적 공유** 과정이 얼마나 중요한지 깊이 체감했습니다.
{% endcapture %}
{% include paragraph.html content=paragraph %}

