---
title: "게임 어빌리티 시스템 사용/분석"
description: "언리얼 GAS의 사용기, 분석결과를 소개합니다"
date: 2025-06-28 00:00:00
layout: post
image: "images/AbilitySystem.png"
subtitle: 
 - "1️⃣ 언리얼 게임플레이 어빌리티 시스템 (GAS)"
 - "2️⃣ UAbilitySystemComponent (ASC)"
 - "3️⃣ 게임플레이 어빌리티 (Gameplay Ability)"
 - "4️⃣ 게임플레이 어트리뷰트 (Gameplay Attribute)"
 - "5️⃣ 게임플레이 이펙트 (Gameplay Effect)"
 - "6️⃣ 게임플레이 태그 (Gameplay Tag)"
published: true
order: 9900
AutoContents: true
---

{% capture paragraph %}
## **1️⃣ 언리얼 게임플레이 어빌리티 시스템 (GAS)**

### 🔍 GAS 구성 요소
언리얼의 **Gameplay Ability System(GAS)** 은 크게 세 가지 요소로 구성됩니다.

1. **Gameplay Ability**  
   - 캐릭터가 수행할 수 있는 행동 정의  
   - 예: 공격, 방어, 회복
2. **Gameplay Effect**  
   - 어빌리티의 결과 정의  
   - 예: 피해 적용, 상태 변경
3. **Gameplay Attribute**  
   - 캐릭터의 상태/속성 정의  
   - 예: 체력, 마나, 공격력

**C++ 관점 비유**
- Gameplay Ability → **함수 객체**
- Gameplay Effect → **상태 변경용 Get/Set 인터페이스**
- Gameplay Attribute → **변수**

이 세 가지 요소를 조합하면 다양한 게임 콘텐츠를 모듈화해 제작할 수 있습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **2️⃣ UAbilitySystemComponent (ASC)**
`UAbilitySystemComponent` 는 GAS의 핵심으로, **캐릭터의 어빌리티 실행 및 효과 적용**을 담당합니다.

- 사용 조건: 액터가 `IAbilitySystemInterface` 구현
- 주요 기능
  - 네트워크 리플리케이션
  - 어빌리티 실행 관리
  - 이펙트 적용 및 관리
  
> 이후 내용에서는 **ASC** 로 표기합니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}

## **3️⃣ 게임플레이 어빌리티 (Gameplay Ability)**

- ASC를 통해 캐릭터에 부여  
- 태그나 이벤트 기반으로 실행  
- 즉시 실행 또는 조건부 활성화 가능

### ⚙️ 실행 정책 (`EGameplayAbilityNetExecutionPolicy`)
어빌리티의 실행 위치(서버/클라이언트)를 결정합니다.

| 정책 | 설명 |
|------|------|
| **LocalPredicted** | 로컬 요청 시 로컬+서버 모두 실행 |
| **LocalOnly** | 로컬에서만 실행 |
| **ServerInitiated** | 서버 요청 시 로컬+서버 모두 실행 |
| **ServerOnly** | 서버에서만 실행 |

### 🛠️ 유용한 어빌리티 태스크
- `AbilityTask_NetworkSyncPoint` : 네트워크 동기화 지점 설정
- `AbilityTask_WaitDelay` : 지정 시간 대기
- `AbilityTask_WaitGameplayEvent` : 특정 이벤트 대기
- `UAbilityTask_PlayMontageAndWait` : 몽타주 실행 후 완료 대기
- `AbilityTask_WaitTargetData` : 타겟 데이터 입력 대기
- `AbilityTask_WaitConfirm` : 실행 확인 대기

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}

## **4️⃣ 게임플레이 어트리뷰트 (Gameplay Attribute)**
- 어트리뷰트 세트를 통해 정의 및 관리
- 초기값: `DefaultStartingData`로 설정
- 값 변경 감지:  
  `UAbilitySystemComponent::GetGameplayAttributeValueChangeDelegate` 사용

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}

## **5️⃣ 게임플레이 이펙트 (Gameplay Effect)**
어빌리티 실행 결과로 발생하는 **효과**를 정의합니다.

- **DurationPolicy** : 지속성 설정
- **Modifier** : 어트리뷰트 값 변경 & 태그 추가/제거
- **커브 테이블** : 레벨 기반 효과 강도 조정
- **게임플레이 큐 태그** : 이펙트 실행 제어  

<br><br>

### **GEComponents**
`GEComponents`는 이펙트의 구성 요소로, 이펙트에 추가적으로 적용할 수 있는 기능을 제공합니다.


### **Modifier**
모디파이어는 지속시간 정책에 따라 수정하는 방법이 달라집니다.
**DurationPolicy**에 따른 모디파이어 적용 방식은 다음과 같습니다.
  - **즉시(Instant)** : 현재값에 즉시 적용
  - **지속(Duration)** : 일정 시간 동안 베이스값에 적용
  - **무제한(Infinite)** : 제거 전까지 지속 적용  

방식, 수치또한 다양하게 설정할 수 있습니다.
수정방식은 다음과 같습니다.
  - **Add(Base)** : 현재값에 추가
  - **Multiply** : 현재값에 곱하기
  - **Add(Final)** : 마지막값에 추가
  - **Override** : 현재값을 새로운 값으로 대체

수치는 다음과 같이 설정할 수 있습니다.
  - **Scalable Float** : 단순수치
  - **Attribute Based** : 어트리뷰트 값 참조
  - **Custom Calculation** : 사용자 정의 계산식 사용
  - **Set by Caller** : 호출 시 값 설정

수치에 추가적으로 커브 테이블을 사용하여 레벨에 따라 효과를 조정할 수 있습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}
## **6️⃣ 게임플레이 태그 (Gameplay Tag)**
- 어빌리티, 이펙트, 어트리뷰트에 적용 가능한 메타데이터  
- ASC가 관리하며 게임 로직과 흐름을 유연하게 제어 가능

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **요약**
GAS는 거창한게 아니라 유연한 구조로 게임 콘텐츠를 모듈화하는 시스템입니다.


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

