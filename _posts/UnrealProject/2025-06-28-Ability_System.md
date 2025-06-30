---
title: "게임 어빌리티 시스템 사용/분석"
description: "언리얼 GAS의 사용기, 분석결과를 소개합니다"
date: 2025-06-28 00:00:00
layout: post
image: "images/icon_37.gif"
subtitle: 
 - "언리얼 게임플레이 어빌리티 시스템 (GAS)"
 - "UAbilitySystemComponent (ASC)"
 - "게임플레이 어빌리티 (Gameplay Ability)"
 - "게임플레이 어트리뷰트 (Gameplay Attribute)"
 - "게임플레이 이펙트 (Gameplay Effect)"
 - "게임플레이 태그 (Gameplay Tag)"
published: true
order: 9900
---

{% capture paragraph %}

## **언리얼 게임플레이 어빌리티 시스템 (GAS)**

### GAS 구성 요소 간단히 알아보기
언리얼의 게임플레이 어빌리티 시스템(GAS)은 크게 3가지 요소로 구성됩니다.
- **Gameplay Ability**: 캐릭터가 수행할 수 있는 행동을 정의합니다. 예를 들어 공격, 방어, 회복 등의 기능을 구현할 수 있습니다.
- **Gameplay Effect**: 어빌리티의 결과를 정의합니다. 피해를 입히거나 상태를 변경하는 등의 효과를 표현할 수 있습니다.
- **Gameplay Attribute**: 캐릭터의 상태나 속성을 정의합니다. 체력, 마나, 공격력 등의 데이터를 표현합니다.
<br><br>
C++ 관점에서 비유하면 다음과 같습니다.
- **Gameplay Ability**: 함수 객체
- **Gameplay Effect**: 상태변화를 위한 Get/Set 인터페이스
- **Gameplay Attribute**: 변수
<br>
이 세 가지 요소를 통해 다양한 게임 콘텐츠를 모듈화하여 제작할 수 있습니다.
<br><br>

## **UAbilitySystemComponent (ASC)**
`UAbilitySystemComponent`는 GAS의 핵심으로, 캐릭터가 어빌리티를 실행하고 효과를 적용하는 기능을 담당합니다. 
액터가 `IAbilitySystemInterface`를 구현하여 사용할 수 있으며, 다음과 같은 기능을 제공합니다.
- 네트워크 리플리케이션
- 어빌리티 실행 관리
- 이펙트 적용 및 관리
이후부터는 편의상 ASC로 표기합니다.
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}
## **게임플레이 어빌리티 (Gameplay Ability)**
Gameplay Ability는 ASC를 통해 캐릭터에게 부여되며, 태그나 이벤트 기반으로 실행됩니다. 
어빌리티는 즉시 실행하거나 부여 후 조건에 따라 활성화할 수 있습니다.
<br><br>

### 어빌리티 실행 정책 (`EGameplayAbilityNetExecutionPolicy`)
어빌리티가 서버와 클라이언트 중 어디에서 실행될지 결정하는 정책입니다.
- **LocalPredicted**: 로컬에서 요청하면 로컬과 서버 모두 실행
- **LocalOnly**: 로컬에서만 실행
- **ServerInitiated**: 서버에서 요청하면 로컬과 서버 모두 실행
- **ServerOnly**: 서버에서만 실행.
<br><br>

### 유용한 어빌리티 태스크
어빌리티 실행 중 특정 작업을 수행하도록 돕는 기능입니다.
- **AbilityTask\_NetworkSyncPoint**: 네트워크 동기화 지점 설정
- **AbilityTask\_WaitDelay**: 지정된 시간 대기
- **AbilityTask\_WaitGameplayEvent**: 특정 게임플레이 이벤트 대기
- **UAbilityTask\_PlayMontageAndWait**: 몽타주 애니메이션 실행 후 완료 대기
- **AbilityTask\_WaitTargetData**: 타겟 데이터 입력 대기
- **AbilityTask\_WaitConfirm**: 어빌리티 실행 확인 대기
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}

## **게임플레이 어트리뷰트 (Gameplay Attribute)**
Gameplay Attribute는 어트리뷰트 세트를 통해 정의되고 관리됩니다. 
세트의 초기값은 `DefaultStartingData`를 통해 설정 가능합니다. 
어트리뷰트 값의 변경 사항은 `UAbilitySystemComponent::GetGameplayAttributeValueChangeDelegate`를 통해 구독할 수 있으며, 
값 변경은 ASC를 통해 관리되어 서버-클라이언트 간 동기화됩니다.
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}

## **게임플레이 이펙트 (Gameplay Effect)**
Gameplay Effect는 어빌리티 실행 결과로 발생하는 효과입니다. 다음과 같은 기능을 제공합니다.
* **DurationPolicy**를 통한 지속성 설정
- **Modifier**를 통한 어트리뷰트 값 변경 및 태그 추가/제거
  - **즉시(Instant)**: Modifier가 어트리뷰트의 현재값에 즉시 적용됩니다.
  - **지속시간(Duration)**: Modifier가 일정 시간 동안 어트리뷰트의 베이스값에 적용됩니다.
  - **무제한(Infinite)**: Modifier가 제거되기 전까지 지속적으로 어트리뷰트의 베이스값에 적용됩니다.
- **커브 테이블**을 통한 레벨 기반 효과 강도 조정
- **게임플레이 큐 태그**로 이펙트 실행 제어
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}

## **게임플레이 태그 (Gameplay Tag)**
Gameplay Tag는 어빌리티, 이펙트, 어트리뷰트에 적용할 수 있는 메타데이터로 ASC가 관리합니다. 
이를 통해 게임 로직과 실행 흐름을 유연하게 제어할 수 있습니다.
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}



<!-- 
{% comment %}
------------------------------------------------------
{% capture paragraph %}
## **제목**
<br><br>

### 배경  
<br><br>

### 문제 인식  
<br><br>

### 문제 해결 
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}
------------------------------------------------------
{% endcomment %}
-->

