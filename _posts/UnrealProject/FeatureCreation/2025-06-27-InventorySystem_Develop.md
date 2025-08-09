---
title: "인벤토리 시스템 제작"
description: "언리얼엔진에서 인벤토리 시스템 제작 경험을 공유합니다"
date: 2025-06-27 00:00:00
layout: post
image: "images/UObjectReplicated.png"
hover_image: "images/UObjectReplicated_Hover.png"
subtitle: 
 - "언리얼 리플리케이션"
 - "FastArraySerializer"
 - "어빌리티 시스템사용"
published: true
order: 10002
AutoContents: true
---

{% capture paragraph %}
# **언리얼 리플리케이션**

### 🎨 언리얼 오브젝트 기반 아이템 제작
프로젝트 초기에 **아이템을 간단한 구조체 대신 언리얼 오브젝트로 설계**하려 했습니다. 블루프린트의 장점을 활용하기 위함입니다.  
각 아이템을 고유한 블루프린트 클래스로 정의하고 **상속을 통해 기능을 오버라이드** 하면 새 아이템의 추가나 수정이 쉬워지고, 전체 시스템의 확장성과 유지보수성이 높아집니다.
<br><br>

### ⚠️ 리플리케이션 문제의 발견
언리얼 엔진의 네트워크 리플리케이션은 액터 기반입니다.  
일반적인 언리얼 오브젝트는 기본적으로 복제되지 않기 때문에,
오브젝트 기반 설계를 고수하면 클라이언트와 서버 사이의 상태 동기화가 어렵습니다.
단순 구조체를 사용하면 쉽게 복제할 수 있지만 상속을 지원하지 않고, RPC를 통해 상태를 보내려 해도 일관성을 유지하기 힘들다는 한계가 있었습니다.
<br><br>

### ✅ 서브오브젝트를 통한 리플리케이션 해결
리서치 끝에 **액터의 서브오브젝트**로 등록하면 언리얼 오브젝트도 리플리케이션이 가능하다는 사실을 알게 되었습니다.  
인벤토리 컴포넌트에 아이템을 추가하거나 제거할 때마다 해당 아이템을 **서브오브젝트로 등록**해 서버와 클라이언트 간 상태를 안정적으로 동기화했습니다.  
이 방식 덕분에 일반 아이템뿐 아니라 **던지기형, 설치형 등의 아이템**도 효과적으로 관리할 수 있었습니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
# **FastArraySerializer**

### ❓배열 직렬화 과정의 의문
인벤토리 컴포넌트가 아이템을 배열로 관리하는데,  
“**배열의 한 요소만 바뀌어도 전체 배열을 다시 직렬화해야 하나?**”라는 의문이 들었습니다.  
조사해 보니 일반 배열은 한 요소가 달라져도 **전체를 다시 직렬화**하기 때문에 네트워크 비용이 불필요하게 늘어나는 구조였습니다.
<br><br>

### 🚀 FastArraySerializer를 통한 최적화
이 비효율성을 개선하기 위해 `FastArraySerializer`를 도입했습니다.  
`FastArraySerializer`는 변경된 요소에 더티 마크를 남기고 **수정된 항목만 선택적으로 직렬화**합니다.  
그 결과 배열 전체를 재전송할 필요가 없어 네트워크 부하를 크게 줄일 수 있었습니다.
<br><br>

### 🛠️ 내부 동작과 주의점
`FastArraySerializer`는 각 요소가 `FFastArraySerializerItem`을 상속할 때 **ReplicationID**를 부여하고,  
변경 시 해당 항목만 RPC 형태로 전송합니다.  
다만 구조체 전체를 `Swap`할 때 기존 ReplicationID가 유지되면 **실제로 바뀐 데이터가 있어도 클라이언트로 전송되지 않을 수 있습니다**.  
성능은 좋지만 이러한 구현상의 주의가 필요합니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
# **어빌리티 시스템사용**
### 📄 도입 배경
팀 프로젝트 과정에서 언리얼 엔진의 **어빌리티 시스템**을 도입했고, 인벤토리 아이템에도 적극 활용했습니다.  
어빌리티 시스템은 아이템 사용 방식을 모듈화하고 체계적으로 관리할 수 있게 해주는 강력한 도구입니다.
<br><br>

### 🌟 장점
* **사용 방식과 효과를 손쉽게 정의** – 블루프린트나 C++로 아이템의 사용 방법, 효과, 지속 시간을 세세하게 지정할 수 있습니다.  
* **유연한 확장** – 새로운 기능을 추가할 때 기존 아이템 코드를 거의 건드리지 않고 **어빌리티 클래스만 새로 만들어 등록**하면 됩니다.  
* **높은 유지보수성** – 아이템 기능을 독립된 어빌리티로 분리해 두면 수정·추가 작업이 훨씬 깔끔합니다.
<br><br>

### ✅ 어빌리티 시스템의 실무 활용

데이터 테이블의 각 행에 **어빌리티 클래스 참조**를 추가하는 방식으로  
패시브 효과, 착용 효과, 사용 효과 등을 **CSV 데이터**로 관리했습니다.  
이로써 아이템 데이터 관리가 체계화되어 제작 속도와 생산성이 크게 높아졌습니다.

**아이템 테이블 예시**

| ItemID               | CurrentHoldAbilityID | PrePareUseAbilityID     | UseAbilityID    |
|:--------------------:|:--------------------:|:-----------------------:|:---------------:|
| ManaStone_A          | None                 | GA_ManaStoneUse_A       | None            |
| Pickaxe_Steel        | GA_Pickaxe_Steel     | GA_PlayerPrePareAttack  | GA_PlayerAttack |
| Throwable_Dynamite   | None                 | GA_ShowProjectilePath   | GA_ThrowItem    |
| Installable_Sensor   | None                 | GA_PreViewInstallMesh   | GA_InstallItem  |
| Consumable_HPBig     | None                 | GA_Healing_Big          | None            |

**던지기 아이템 테이블 예시**

| ItemID              | ImpactAbilityID   |
|:-------------------:|:-----------------:|
| Throwable_Shock     | GA_Shock          |
| Throwable_Paint     | GA_Paint          |
| Throwable_Dynamite  | GA_Damage_Dynamite|


{% endcapture %}
{% include paragraph.html content=paragraph %}

### 🔎 더보기
{% assign other_post = site.posts | where: "title", "게임 어빌리티 시스템 사용/분석" | first %}
<a class="Link" href="{{ other_post.url | relative_url }}">{{ other_post.title }}</a>

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
