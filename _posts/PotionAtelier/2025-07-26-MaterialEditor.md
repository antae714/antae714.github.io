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
모델링툴에서 사용하는 쉐이더코드와 게임엔진의 쉐이더코드, 후처리 절차가 다르기에
게임엔진에서는 모델링툴과	동일한 색이표현되지 않는 문제를 알게 되었습니다.  
아트작업자가 게임엔진에서 원하는 색표현을 위해, 
의도된 표현을 정확히 표현하기위해 꼭 필요한 기술이라 생각되어 제작하게 되었습니다.

오픈소스 라이브러리를 찾아보았으나 `MaterialX`는 OpenGL기반으로 제작되어있어
DirectX기반의 엔진에서 사용하기에는 무리가 있어 직접 제작하게 되었습니다.

언리얼 엔진처럼 `MaterialX`의 포맷을 읽어서 엔진에맞게 생성할수도있지만
그전에 머티리얼관련 기술이 엔진에 있어야 이식할수 있다고 생각햇기떄문에
머티리얼 노드 에디터를 제작하게 되었습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **설계**

소스엔진의 Hammer에디터처럼
에디터 데이터를 컴파일하는 과정을거쳐 게임에서 쓸수있는 바이너리데이터로 변환하는 툴을 만드는것이 목표였습니다.
하지만 렌더링에 관련된 라이브러리가 일치해야 같은 결과를 얻을수있기에 렌더링 관련 코드를 라이브러리로 분리하였습니다.

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

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **노드 에디터로 픽셀쉐이더 생성하는방법**

기존 픽셀 쉐이더 스테이지에서 사용되는 데이터를 `GBufferMaterial`하나의 구조체로 모아서

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

<br>
`GBufferMaterial` 구조체를 어떻게 구성할지를 텍스트 생성하게 하였습니다.

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

### 노드 에디터로 문자열 생성
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

* **디테일 패널에서 설정:** `0(CustomValue)`, `1(Define)`
* **최종 노드 핀에 연결되어 있을 때만 기록:** `2(RegisterValue)`, `3(Declaration LocalValue)`, `4(Execution)`

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}

## 노드 팩토리 제작
노드를 스트링으로 생성하여 **에디터용 데이터**로 **저장, 로드**할수있도록 제작하였습니다.

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

### 에디터 작업시 필수사항
에디터제작 작업은 사용자가 작업을 하면서 병렬적으로 개발이 진행되어야 한다는것을 알게되었습니다.
에디터에 아무리 많은 기능이 있다고 해도 사용자가 다른기능을 원하면 도로묵이되며 시간이라는 비용을 다른곳에 쓴꼴이 됩니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}
