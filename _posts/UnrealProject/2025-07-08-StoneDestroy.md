---
title: "언리얼 프랍 단계적 파괴시스템 제작"
date: 2025-07-08 00:00:00
layout: post
image: "images/Image.png"
subtitle: 
 - "🛠️ 단계적 파괴를위한 밑준비"
 - "🧱 지오메트리 컬렉션 만들기"
 - "🌐 피직스 필드 구성하기"
 - "🔄 네트워크 동기화"
description: "프랍 단계적으로 파괴하는방법에 대하여 이야기합니다."
order: 9950
AutoContents: true
---


{% capture paragraph %}
## **🛠️ 단계적 파괴를 위한 밑준비**
광석이 채굴되는 느낌을 주기 위해 단계적으로 메쉬를 변화시키는 기능이 필요했습니다. 이를 위한 고려 사항은 다음과 같습니다.
- 메쉬 바꾸기: 미리 준비된 여러 개의 메쉬로 단계적으로 변경하는 방법
- 카오스 디스트럭션 사용: 카오스 디스트럭션을 활용하여 단계적으로 파괴하는 방법
- 프로시저럴 메쉬 사용: 프로시저럴 메쉬를 이용하여 실시간으로 메쉬를 자르는 방법
  
이 중 카오스 디스트럭션이 플레이어 경험 및 유지보수 측면에서 가장 적합하다고 판단했습니다.

<br>
{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **🧱 지오메트리 컬렉션 만들기**
언리얼에서 카오스 디스트럭션을 사용하려면 지오메트리 컬렉션을 생성해야 합니다. 
선택 툴에서 프랙처 모드를 선택하여 지오메트리 컬렉션을 생성할수있습니다
추가로 지오메트리 컬렉션을 생성할 때 다음과 같은 설정을 고려해야 합니다:
- 물체가 파괴된 후 파편을 제거하는 옵션
- 단면 UV 설정
- 모든 파괴 및 물리적 상호작용 설정은 이 지오메트리 컬렉션에서 관리됩니다.

<br>
{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **🌐 피직스 필드 구성하기**
지오메트리 컬렉션의 파괴, 고정, 슬립 상태 및 비활성화를 관리하기 위해 피직스 필드를 사용합니다.

피직스 필드 생성 방법


지오메트리 컬렉션을 파괴, 고정, 슬립 비활성화하기 위해서는 피직스 필드를 사용합니다.

### ⚙️ 피직스 필드 생성 방법

| 생성 방식 | 설명 |
|-|-|
| Transient Field | 한 프레임(틱) 동안만 적용되며 자동 제거됩니다. |
| Persistent Field | FieldSystemComponent가 유지되는 동안 계속 적용됩니다. 명시적 제거 필요. |
| Construction Field | 레벨 로드 또는 액터 스폰 시 단 한 번만 적용됩니다. |

<br><br>

### 📋 피직스 필드 종류 및 설정

| 필드명 | 설명                                     | 피직스 타입 | 비고
|-|-|-|-|
| 슬립 필드 | 조각이 슬립 상태로 전환되는 민감도 조절 | `SleepingThreshold` | 값이 낮을수록 쉽게 슬립 상태로 전환 |
| 비활성화 필드 | 조각을 완전히 비활성화하여 물리, 렌더링, 상호작용 모두 제거 | `DisableThreshold` | 충돌 및 트레이스 비활성화 |
| 파괴 필드 | 파괴를 유도하거나 내부 응력을 증가시켜 파괴 조건 형성 | `ExternalClusterStrain` | 단일 실행 시 한 단계만 파괴 |

<br><br>

### ⚓ 앵커 필드 설정하기
앵커 필드는 특정 영역 내 오브젝트의 상태를 `Kinematic`으로 설정해 고정된 효과를 줍니다. 이를 구현하려면 다음 방법을 사용합니다.
- `UUniformInteger`를 활용하여 상태 변경
- `UCullingField`를 사용하여 평가 영역 지정
- 평가 필드를 `DynamicState`로 설정하여 적용
  

First Term
: This is the definition of the first term.

Second Term
: This is one definition of the second term.
: This is another definition of the second term.


``` cpp
void AAnchorFieldSystemActor::Enable()
{
	// Initialize the BoxFalloff
	{
		const float Magnitude{ 1.0f };
		const float MinRange{ 1.0f };
		const float MaxRange{ 1.0f };
		const float Default{ 0.0f };
		const FTransform Transform{ AnchorBox->GetComponentTransform() };
		const EFieldFalloffType Falloff{ Field_FallOff_None };

		BoxFalloff->SetBoxFalloff(Magnitude, MinRange, MaxRange, Default, Transform, Falloff);
	}

	// Initialize the UniformInteger
	{
		EObjectStateTypeEnum Magnitude{ EObjectStateTypeEnum::Chaos_Object_Kinematic };
		UniformInteger->SetUniformInteger(1);
	}

	// Initialize the CullingField
	{
		CullingField->SetCullingField(BoxFalloff, UniformInteger, Field_Culling_Outside);
	}

	// Add the CullingField to the FieldSystemComponent
	if (GetFieldSystemComponent())
	{
		bool Enabled{ true };
		EFieldPhysicsType Target{ EFieldPhysicsType::Field_DynamicState };
		UFieldSystemMetaData* MetaData{ nullptr };

		GetFieldSystemComponent()->AddPersistentField(Enabled, Target, MetaData, CullingField);
	}
}
```

<!-- 
<br>
앵커필드는 조금 복잡합니다. <br>
[피직스 필드 레퍼런스 가이드](physicsfieldDoc)
을 참조해야합니다.
언리얼 피직스 필드를 조합하여 앵커의 효과를 얻어야하기때문입니다.
특정 영역 내부의 오브젝트 상태를 `Kinematic`으로 설정하면 앵커와 같은 효과를 얻을 수 있습니다.

문서에따르면 피직스 타입이 `DynamicState`인 경우 인트타입을 평가 결과로 사용할시 상태를변경할수 있습니다.
인트타입을 평가 결과로 사용하기위해 `UFieldNodeInt`를 상속받은 `UUniformInteger` 노드를 사용합니다.
그러나 `UFieldNodeInt`는 영역을 설정할수 없기때문에,
`UCullingField`를 사용하여 `UUniformInteger`를 평가 필드로,
영역을 지정할수있는 필드를이용하여 공간을 설정할수있습니다.
해당 컬링필드를 피직스 필드로사용하고 피직스타입을 `DynamicState`로 설정하면 앵커필드와 같은 효과를 얻을수 있습니다.


<details>
<summary>코드 예제</summary>
<div markdown="1">
``` cpp
void AAnchorFieldSystemActor::Enable()
{
	// Initialize the BoxFalloff
	{
		const float Magnitude{ 1.0f };
		const float MinRange{ 1.0f };
		const float MaxRange{ 1.0f };
		const float Default{ 0.0f };
		const FTransform Transform{ AnchorBox->GetComponentTransform() };
		const EFieldFalloffType Falloff{ Field_FallOff_None };

		BoxFalloff->SetBoxFalloff(Magnitude, MinRange, MaxRange, Default, Transform, Falloff);
	}

	// Initialize the UniformInteger
	{
		EObjectStateTypeEnum Magnitude{ EObjectStateTypeEnum::Chaos_Object_Kinematic };
		UniformInteger->SetUniformInteger(1);
	}

	// Initialize the CullingField
	{
		CullingField->SetCullingField(BoxFalloff, UniformInteger, Field_Culling_Outside);
	}

	// Add the CullingField to the FieldSystemComponent
	if (GetFieldSystemComponent())
	{
		bool Enabled{ true };
		EFieldPhysicsType Target{ EFieldPhysicsType::Field_DynamicState };
		UFieldSystemMetaData* MetaData{ nullptr };

		GetFieldSystemComponent()->AddPersistentField(Enabled, Target, MetaData, CullingField);
	}
}
```
</div>
</details>
-->

자세한 사항은 [피직스 필드 레퍼런스 가이드][physicsfieldDoc]를 참조하십시오.

<br>

[physicsfieldDoc]: https://dev.epicgames.com/documentation/ko-kr/unreal-engine/reference-guide-for-physics-field-in-unreal-engine
{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **🔄 네트워크 동기화**
지오메트리 컬렉션의 본 상태는 기본적으로 네트워크로 동기화되지 않으므로 다음 방법 중 하나를 선택해 동기화해야 합니다.
- 본 상태 수동 동기화
- 클라이언트에서 동일 시뮬레이션 실행
단계적 파괴는 클라이언트에서도 동일하게 실행하며, 최종 파괴 상태는 `bIsDie` 변수를 `ReplicatedUsing`하여 처리합니다. 이를 통해 `CrumbleActiveClusters`를 호출하여 모든 클러스터를 완전히 파괴할 수 있습니다.

<br>
{% endcapture %}
{% include paragraph.html content=paragraph %}



