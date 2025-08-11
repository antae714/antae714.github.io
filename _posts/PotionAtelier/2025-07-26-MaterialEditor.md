---
title: "머티리얼 노드 에디터 제작"
date: 2025-07-26 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "머티리얼 에디터 제작경험에 대해 이야기 합니다."
AutoContents: false
mermaid: true
TitleVideo: "/assets/MyLittleStorage/Temp.mp4"
---


{% capture paragraph %}
## **초기 목표**

모델링 툴과 게임 엔진은 **셰이더 코드·후처리·감마 처리**가 달라 동일 색을 재현하기 어렵습니다. 
아티스트가 의도한 색을 **엔진 내에서도 동일하게** 표현하기 위해 머티리얼 노드 에디터를 제작했습니다.

오픈소스 대안으로 `MaterialX`를 검토했지만 **OpenGL 기반**이라 DirectX 기반 엔진에 바로 적용하기 어려웠습니다. 
언리얼처럼 `MaterialX` 포맷을 읽어 **엔진 규격으로 변환**하는 방안도 검토했으나, 
그 전에 엔진 내부에 **머티리얼 노드 체계와 코드 생성 기술**이 먼저 있어야 한다고 판단하여 **직접 제작**을 진행했습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **설계**

소스 엔진의 Hammer 에디터처럼, **에디터 데이터 → 바이너리 게임 데이터**로 컴파일하는 툴체인을 목표로 했습니다. 
렌더 일치성을 위해 **렌더링 코드를 공유 라이브러리**로 분리했습니다.

``` mermaid
flowchart LR
  subgraph Tools
    MNE[Material Node Editor]
  end
  
  subgraph Shared_Rendering
    RL[(Render Library)]
  end

  subgraph Runtime
    GA[Game Application]
  end

  
  MNE -->|uses| RL
  GA -->|uses| RL


```

데이터 흐름은 다음과 같습니다.

``` mermaid
flowchart LR
  subgraph NodeEditor
    ED[(Editor Data)]
    MNE[Material Node Editor]
    GBD[(Game Binary Data)]
    GA[Game Application]
  end

  ED e1@==> |uses|MNE
  MNE e2@==>|compiles| GBD
  GBD e3@==>|uses| GA
  
  e1@{ animate: true, animation: slow }
  e2@{ animate: true, animation: slow }
  e3@{ animate: true, animation: slow }
```

추가로, 타 작업자 요청으로 **맵 에디터 내에서 머티리얼 에디터를 패널 형태로 호출**할 수 있도록 임베디드 UI로 지원했습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **노드 에디터 → 픽셀 셰이더 생성**

픽셀 셰이더에서 사용하는 주요 항목을 `GBufferMaterial` 구조체로 묶고, **노드 그래프 → HLSL 문자열**을 생성해 채워 넣습니다.

``` hlsl
#ifndef __GBUFFERMATERIAL_HLSL__
#define __GBUFFERMATERIAL_HLSL__

#include "Shared.hlsli"


struct GBufferMaterial
{
	float3 albedo;
	float metallic;
	
	float3 specular;
	float ambiantOcclusion;
	
	float3 normal;
	float roughness;
	
	float3 emissive;
	float ShadingModelID;
	
	float clipAlpha;
	float alpha;
	
	float4 GBuffer[4];
};

GBufferMaterial GetDefaultGBufferMaterial(PS_INPUT input)
{
	GBufferMaterial material = (GBufferMaterial) 0;
	
	material.albedo = 0.0;
	material.alpha = 1.0;
	material.specular = 1.0;
	material.metallic = 0.0;
	material.roughness = 0.0;
	material.ambiantOcclusion = 0.2;
	
	material.normal = float3(0.0, 0.0, 1.0);
	material.emissive = 0;
	
	material.clipAlpha = 0.3333;
	
	return material;
}

#endif // __GBUFFERMATERIAL_HLSL__
```

<br><br>  
템플릿에 들어갈 **가변 슬롯**은 노드 그래프에서 생성합니다. 

{% raw %}
``` hlsl
#include "Shared.hlsli"
#include "GBufferMaterial.hlsli"

struct CustomBuffer
{{
	//CustomValue
	{0} 
}};

cbuffer CustomBuffer : register(b5)
{{
	CustomBuffer customData;
}};
// Define
{1}
// RegisterValue
{2}


GBufferMaterial GetCustomGBufferMaterial(PS_INPUT input)
{{
    GBufferMaterial material = GetDefaultGBufferMaterial(input);

	// Declaration LocalValue
	{3}

	// Excution
	{4}

    return material;
}}

#define GetGBufferMaterial GetCustomGBufferMaterial
#include "BasePassPS.hlsl"
```
{% endraw %}

### **슬롯 채우기 규칙**
포맷 슬롯에 들어갈 문자열을 **노드 기반**으로 생성합니다.
#### 노드 종류 (0–4)

| ID | 이름                         | 역할(한 줄 요약)                          |
| -: | -------------------------- | ----------------------------------- |
|  0 | **CustomValue**            | 간단한 **상수 버퍼**. CPU에서 갱신한 값을 GPU가 사용 |
|  1 | **Define**                 | 쉐이더에서 사용할 **옵션 상수** 정의              |
|  2 | **RegisterValue**          | **텍스처 입력** 정의                       |
|  3 | **Declaration LocalValue** | **지역 변수 선언**                        |
|  4 | **Execution**              | **지역 변수 연산/계산** 수행                  |

#### Define 옵션 항목
* 머티리얼 도메인
* 블렌드 모드
* 셰이딩 모델
* 디퍼드 렌더링 여부

#### 기록(출력) 규칙

* 디테일 패널에서 직접 설정되어 **항상 기록**: `0(CustomValue)`, `1(Define)`
* **최종 출력 핀과 연결된 경우에만 기록**: `2(RegisterValue)`, `3(Declaration LocalValue)`, `4(Execution)`

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}

## **노드 팩토리 & 직렬화**
노드를 **문자열로 직렬화**하여 에디터 데이터에 **저장/로드**할 수 있도록 팩토리를 구성했습니다.

<br><br>


## 지원 노드 종류

|  # | 노드                         | 설명                      |
| -: | --------------------------| ----------------------- |
|  1 | `ConstantValueNode`        | 스칼라 상수 값                |
|  2 | `ConstantVector2Node`      | 2D 벡터 상수                |
|  3 | `ConstantVector3Node`      | 3D 벡터 상수                |
|  4 | `ConstantVector4Node`      | 4D 벡터 상수                |
|  5 | `TextureNode`              | 텍스처(샘플러) 입력             |
|  6 | `AddNode`                  | 덧셈                      |
|  7 | `SubNode`                  | 뺄셈                      |
|  8 | `MulNode`                  | 곱셈                      |
|  9 | `DivNode`                  | 나눗셈                     |
| 10 | `SinNode`                    | 사인(sin)                 |
| 11 | `CosNode`                    | 코사인(cos)                |
| 12 | `LerpNode`                 | 선형 보간(lerp)             |
| 13 | `DotNode`                  | 내적(dot)                 |
| 14 | `CrossNode`                | 외적(cross, 3D)           |
| 15 | `NormalizeNode`            | 벡터 정규화                  |
| 16 | `LengthNode`               | 벡터 길이                   |
| 17 | `SaturateNode`             | 0–1 범위 클램프              |
| 18 | `PowerNode`                | 거듭제곱(pow)               |
| 19 | `SqrtNode`                 | 제곱근(sqrt)               |
| 20 | `AbsNode`                  | 절댓값(abs)                |
| 21 | `UnPackNormalNode`         | \[0,1] 노멀맵 → \[-1,1] 언팩 |
| 22 | `TimeNode`                  | 시간 값 출력                 |
| 23 | `TexCoordNode`             | UV 좌표 제공                |
| 24 | `MakeVector2Node`          | 스칼라 → vec2 생성           |
| 25 | `MakeVector3Node`          | 스칼라/vec2 → vec3 생성      |
| 26 | `MakeVector4Node`          | 스칼라/vec3 → vec4 생성      |
| 27 | `CustomValueNode`           | CPU에서 전달한 상수 버퍼 값 참조    |
| 28 | `BreakVector2Node`         | vec2를 컴포넌트로 분해          |
| 29 | `BreakVector3Node`         | vec3를 컴포넌트로 분해          |
| 30 | `BreakVector4Node`         | vec4를 컴포넌트로 분해          |
| 31 | `FlipBook`                 | 플립북(스프라이트 시트) 샘플        |
| 32 | `FlipBookInterpolated`     | 보간형 플립북 샘플              |
| 33 | `FmodeNode`                | 부동소수 나머지(fmod) 연산       |
| 34 | `ParticleSurvivalTimeNode`  | 파티클 생존/경과 시간 값          |
| 35 | `RGBtoSRGBNode`             | RGB → sRGB(감마 인코딩)      |
| 36 | `SRGBtoRGBNode`             | sRGB → RGB(감마 디코딩)      |
| 37 | `RGBtoHSVNode`              | RGB → HSV 변환            |
| 38 | `HSVtoRGBNode`              | HSV → RGB 변환            |


{% endcapture %}
{% include paragraph.html content=paragraph %}




{% capture paragraph %}
## **에디터 제작 시 배운 점**
* 기능을 많이 넣는 것보다, **실사용 흐름에서 꼭 필요한 것**부터 병렬적으로 개발하는 편이 효율적입니다.
* 에디터 제작은 **사용자 피드백**을 반영하여 **점진적으로 개선**하는 것이 중요합니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}
