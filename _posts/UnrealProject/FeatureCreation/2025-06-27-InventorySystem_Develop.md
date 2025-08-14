---
title: "인벤토리 시스템 제작"
description: "언리얼엔진에서 인벤토리 시스템 제작 경험을 공유합니다"
date: 2025-06-27 00:00:00
layout: post
image: "images/UObjectReplicated.png"
hover_image: "images/UObjectReplicated_Hover.png"
subtitle: 
 - "언리얼 플러그인"
 - "언리얼 리플리케이션"
 - "FastArraySerializer"
 - "어빌리티 시스템사용"
 - "📄 CSV 기반 데이터테이블의 에셋 경로 불편함 해결"
 - "아이템 물리 오브젝트의 의도치 않은 플랫폼 효과 개선"
 - "파괴 가능한 프랍"
 - "플레이어 상호작용 아이템 카트"
 - "플레이어 상호작용 아이템 가방"
 - "가방 저장 안되는 문제"
 - "오너십 없는 객체의 서버 상호작용 경로 설계"
published: true
order: 10002
AutoContents: false
---

{% capture paragraph %}
# **언리얼 플러그인**
인벤토리 시스템을 제작하면서 다른 게임에서 사용할수 있는 부분은 **언리얼 플러그인**으로 분리하였습니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}


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



{% capture paragraph %}

# **📄 CSV 기반 데이터테이블의 에셋 경로 불편함 해결**
아이템의 정보들을 구성하기위해 기획자가 CSV로 작성한 데이터를 **데이터테이블**로 읽어와 관리할수있게 하였습니다.  
하지만 `UClass`나 `UObject`같은 데이터를 CSV로 편집할때,  
경로(예: `/Game/...`) 형식으로 노출되기 때문에 편집이 번거롭고 **CSV 편집의 장점이 퇴색**듭니다.

<br>

### **😊 ID → 경로 매핑 방식**
이 문제를 해결하기 위해 기획자는 경로를 입력하는것이 아니라 ID를 입력하여 **ID와 에셋 경로를 연결하는 맵**을 만들었습니다.
``` cpp
TMap<FName, FSoftObjectPath> AssetPathMap;
```

맵은 쉽게 접근가능하며 싱글턴같은 효과를 내야하기때문에 `UDeveloperSettings`를 사용하였습니다.  
덕분에 기획자가 CSV를 편집할 때는 ID만 입력하면 되고,
맵에서 ID에 해당하는 경로를 찾아 UClass/UObject로 변환할 수 있습니다.

<br>

### 수동 매핑의 문제점
그러나 이 방식에는 단점이 있습니다.

- 오타나 경로 누락 등 실수가 발생하기 쉽다.
- 에셋 변경 시마다 수동으로 동기화해야 한다.

<br>

### 자동 매핑으로 해결
이 문제를 해결하기 위해 **에셋 레지스트리**를 활용한 자동 매핑 방식을 도입했습니다.

`ID == 에셋 이름`이라는 규칙을 정의하였고
`AssetRegistry`를사용하여 이름이 일치하는 에셋을 검색해 자동 매핑하였습니다.
`AssetRegistry`검색시 `AssetRegistrySearchable`프로퍼티를 이용할수 있었지만,
텍스처, 메시 등 비오브젝트 에셋에는 적용되지 않기 때문에 위와 같은 방법을 사용했습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
# **아이템 물리 오브젝트의 의도치 않은 플랫폼 효과 개선**

작은 물리 오브젝트(파편, 디스트럭션 파티클 등) 위를 밟거나 접촉할 경우,  
플레이어가 이를 플랫폼처럼 인식하여 물리 상호작용 중 **위치·회전이 갑작스럽게 이동하는** 현상이 발생했습니다.  
이는 플레이 감각과 조작 안정성을 저해하는 문제였습니다.

<video controls autoplay muted loop playsinline preload="metadata" style="width:100%;height:auto;">
  <source src="{{ '/assets/MyLittleStorage/Platform.mp4' | relative_url }}" type="video/mp4">
</video>

<br>

## **콜리전 채널 변경**
플레이어가 작은 물리 오브젝트 위에 설 수 없도록, 해당 오브젝트의 콜리전 채널을 변경했습니다.  
그러나 이 방식은 부작용이 있었는데, **플레이어가 이동 중 물리 오브젝트를 밀어내지 못하는 문제**가 발생했습니다.  
즉, 플랫폼 효과는 사라졌지만 물리 상호작용이 손실되었습니다.  

<video controls autoplay muted loop playsinline preload="metadata" style="width:100%;height:auto;">
  <source src="{{ '/assets/MyLittleStorage/NoPlatform.mp4' | relative_url }}" type="video/mp4">
</video>


<br>

## **전용 물리 캡슐 콜리전 추가**
플레이어의 이동 처리는 기본적으로 **캡슐 콜리전**을 사용합니다.  
이를 응용하여, 이동용 캡슐과 별도로 **물리 전용 캡슐 콜리전**을 추가하였습니다.  

- **이동용 캡슐** → 작은 물리 오브젝트와의 플랫폼 충돌 무시
- **물리 전용 캡슐** → 오브젝트 밀기, 파편 반응 등 물리 상호작용 전담

이 방식으로 **의도치 않은 플랫폼 효과를 제거하면서도**
물리 오브젝트와의 자연스러운 상호작용을 유지할 수 있었습니다.

<video controls autoplay muted loop playsinline preload="metadata" style="width:100%;height:auto;">
  <source src="{{ '/assets/MyLittleStorage/Physics.mp4' | relative_url }}" type="video/mp4">
</video>

<br>


{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
# **파괴 가능한 프랍**

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
# **플레이어 상호작용 아이템 카트**

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
# **플레이어 상호작용 아이템 가방**

### 가방 저장 안되는 문제

### 오너십 없는 객체의 서버 상호작용 경로 설계

{% endcapture %}
{% include paragraph.html content=paragraph %}


