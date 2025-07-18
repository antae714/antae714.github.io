---
title: "인벤토리 시스템 제작"
description: "언리얼엔진에서 인벤토리 시스템 제작경험을 이야기합니다"
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

### 🤔 언리얼 오브젝트 기반 아이템 제작
아이템의 구조를 단순한 구조체 대신 언리얼 오브젝트 기반으로 설계하여, 
언리얼 엔진의 블루프린트 기능을 적극 활용하고자 했습니다. 
각 아이템을 별도의 블루프린트 클래스로 정의하고, 
상속을 통해 아이템별로 고유한 함수를 오버라이드함으로써 확장성과 유지보수성을 높일 수 있기 때문입니다.
<br><br>

### 😨 리플리케이션 문제의 발견
하지만 언리얼 엔진의 네트워크 리플리케이션은 액터 기반으로 이루어지며, 
기본적으로 언리얼 오브젝트는 리플리케이션이 되지 않는다는 사실을 간과했습니다. 
언리얼 오브젝트 대신 구조체를 사용하면 쉽게 리플리케이션이 가능했지만, 
구조체는 상속 구조를 지원하지 않기에 원하는 설계와 맞지 않았습니다. 
RPC를 통한 동기화 방법도 검토했지만, 객체 상태의 일관성 유지가 어렵다는 근본적인 문제가 있었습니다.
<br><br>

### 😮 서브오브젝트를 통한 리플리케이션 해결
추가적인 리서치를 통해 언리얼 엔진에서는 언리얼 오브젝트를 액터의 서브오브젝트로 등록하면 리플리케이션이 가능하다는 것을 알게 되었습니다. 
이 방법을 활용하여 인벤토리 컴포넌트에 아이템이 추가되거나 제거될 때마다 해당 아이템을 서브오브젝트로 등록하여 상태를 서버와 클라이언트 간에 안정적으로 동기화할 수 있었습니다. 
이로써 일반 아이템뿐 아니라 던지기형, 설치형 아이템 등 다양한 아이템 유형을 효과적으로 관리할 수 있게 되었습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
# **FastArraySerializer**

### 🤔 배열 직렬화 과정의 의문
언리얼에서 아이템을 배열로 관리하는 인벤토리 컴포넌트를 개발하면서 문득 
"배열의 요소 하나만 변경되어도 전체 배열이 직렬화되는 것인가?"라는 의문이 생겼습니다. 
조사를 진행한 결과, 실제로 일반적인 배열은 하나의 요소만 변경되어도 배열 전체가 다시 직렬화되는 비효율적인 구조라는 것을 알게 되었습니다.
<br><br>

### 😮 FastArraySerializer를 통한 최적화
이러한 비효율성을 해결하기 위한 방법으로 `FastArraySerializer`를 찾게 되었습니다. 
`FastArraySerializer`를 사용하면 배열의 전체를 직렬화하지 않고, 
변경된 요소만 선택적으로 직렬화하여 네트워크 부하를 크게 줄일 수 있습니다.
<br><br>

### 🤔 FastArraySerializer는 결국 극한의 RPC?
`FastArraySerializer`의 내부 동작 방식은 배열의 요소가 변경될 때마다 해당 요소에 더티 마크를 표시하고, 
변경된 항목만을 ReplicationID를 기반으로 RPC 형태로 클라이언트에 전송합니다. 
다만, `FFastArraySerializerItem`을 상속받은 구조체를 통째로 Swap할 때는 ReplicationID가 유지되기 때문에 실제 변경된 데이터가 있어도 클라이언트로의 전송이 이루어지지 않을 수 있는 주의점이 있습니다. 
이는 효율성을 높이지만, 구현 시 주의가 필요한 부분입니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
# **어빌리티 시스템사용**
### 📄 어빌리티 시스템의 도입
팀 프로젝트를 진행하면서 언리얼 엔진의 어빌리티 시스템을 도입하게 되었으며, 
이를 **아이템 제작**에도 적극적으로 활용했습니다. 
어빌리티 시스템은 언리얼 엔진이 제공하는 강력한 기능으로, 
아이템의 사용 방식을 모듈화하고 체계적으로 관리할 수 있도록 도와줍니다.
<br><br>

### ✅ 어빌리티 시스템의 장점
어빌리티 시스템을 사용하면 아이템의 사용 방식과 효과, 
지속 시간 등의 세부 사항을 블루프린트 또는 C++ 코드로 손쉽게 정의할 수 있습니다. 
이를 통해 **아이템의 기능**을 추가하거나 수정할 때 기존 코드를 변경하지 않고도 새로운 어빌리티를 추가하거나 기존 어빌리티를 간단하게 수정할 수 있어, 
확장성과 유지보수성이 크게 향상됩니다.
<br><br>

### 😀 어빌리티 시스템의 실무 활용
아이템 데이터 테이블을 구성할 때, 각 아이템의 데이터 행에 어빌리티 클래스 참조를 추가할 수 있습니다. 이를 통해
- 패시브 효과
- 착용 효과
- 사용 효과

등 다양한 기능을 CSV 형식의 데이터로 효율적으로 관리할 수 있습니다.

이 방식은 데이터 중심의 아이템 관리가 가능하도록 도와줍니다. 따라서 게임 개발 과정에서 생산성을 높이고 관리 효율성을 극대화할 수 있었습니다.

아이템 테이블 예시

|ItemID|CurrentHoldAbilityID|PrePareUseAbilityID|UseAbilityID|
|-|
|ManaStone_A|None|GA_ManaStoneUse_A|None|
|Pickaxe_Steel|GA_Pickaxe_Steel|GA_PlayerPrePareAttack|GA_PlayerAttack|
|Throwable_Dynamite|None|GA_ShowProjectilePath|GA_ThrowItem|
|Installable_Sensor|None|GA_PreViewInstallMesh|GA_InstallItem|
|Consumable_HPBig|None|GA_Healing_Big|None


<br><br>
던지기 아이템 테이블 예시

|ItemID|ImpactAbilityID|
|-|
Throwable_Shock|GA_Shock
Throwable_Paint|GA_Paint
Throwable_Dynamite|GA_Damage_Dynamite

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
