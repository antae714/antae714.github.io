---
title: "언리얼 프랍 단계적 파괴시스템 제작"
date: 2025-07-08 00:00:00
layout: post
image: "images/Image.png"
subtitle: 
 - "단계적 파괴를위한 밑준비"
 - "지오메트리 컬렉션만들기"
 - "피직스 필드 구성하기"
 - "캐싱 하기"
description: "?에 대하여 이야기합니다."
order: 9950
AutoContents: true
---


{% capture paragraph %}
## **단계적 파괴를위한 밑준비**
광석이 채굴되는 느낌을 주기위해 단계적으로 메쉬를 변화하는 기능이 필요했습니다.
고려사항으로는 아래와같습니다.
1. 메쉬 바꾸기 
 - 메쉬를 사전에 여러개 준비하여 단계적으로 바꾸는 방법
2. 카오스 디스트럭션 사용
 - 카오스 디스트럭션을 사용하여 단계적으로 파괴하는 방법
3. 프로시저럴 메쉬 사용
 - 프로시저럴 메쉬를 사용하여 메쉬를 자르는방법
  
이중에 카오스 디스트럭션이 플레이어 경험, 유지보수측면에서 가장 적합하다고 판단했습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **지오메트리 컬렉션만들기**
언리얼에서 카오스 디스트럭션을 사용하기 위해서는 지오메트리 컬렉션이 필요합니다.
선택툴에서 프랙쳐모드를선택하고, 지오메트리 컬렉션을 생성합니다.
물체 파괴, 조작과 관련된 모든 설정은 지오메트리 컬렉션에서 이루어집니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **피직스 필드 구성하기**

지오메트리 컬렉션을 파괴, 고정, 슬립 비활성화하기 위해서는 피직스 필드를 사용합니다.

필드를 생성하는 방법은 아래와 같습니다.

| 생성 방식 | 설명 |
|-|-|
| Add Transient Field | 일시적인 필드로, 특정 타이밍에만 작동합니다. |
| Add Persistent Field | 지속적으로 작동하는 필드입니다. |
| Add Construction Field | 지오메트리 컬렉션 생성 시점에 적용되는 필드입니다. |

<br>
또한 필드 생성시 필드의 타입을 지정하여 다양한 효과를 줄 수 있습니다.

| 필드명 | 설명                                     | 필드 타입 | 비고
|-|-|-|-|
| 앵커 필드 | 특정 조각을 고정시켜 파괴되지 않도록 만듭니다. | `DynamicState` | Static 상태로 지정됨
| 슬립 필드 | 조각이 슬립 상태로 전환되는 민감도를 조절합니다. | `SleepingThreshold` | 값이 낮을수록 쉽게 슬립됨
| 비활성화 필드 | 조각을 완전히 비활성화시켜 물리, 렌더링, 상호작용 모두 제거합니다. | `DisableThreshold` | 충돌 및 트레이스도 비활성화됨                   |
| 파괴 필드 | 강제 파괴를 유도하거나 내부 응력을 증가시켜 파괴 조건을 만듭니다. | (Strain / Force 조합) | 별도 타입 없음, RadialFalloff + Strain 등 |

// 파괴는 부적확함...
{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}
## **캐싱 하기**


{% endcapture %}
{% include paragraph.html content=paragraph %}




{% capture paragraph %}
## **네트워크 동기화**


{% endcapture %}
{% include paragraph.html content=paragraph %}