---
title: "그래픽스 렌더링 엔진 제작"
date: 2025-08-19 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "소개"
AutoContents: false
mermaid: true
---


{% capture paragraph %}
## **렌더링 라이브러리 제작**
3D 그래픽 API를 기반으로 **게임 렌더링 라이브러리**를 제작하였습니다.  
단순히 그래픽 API를 래핑하는 수준이 아니라, **게임 렌더링에 필요한 단위(Command) 중심**으로 구성하는 것을 목표로 했습니다.

예를 들어, 간단한 메쉬를 그릴 때 엔진이 저수준 API 호출을 직접 쓰는 것이 아니라,
`MeshDrawCommand`를 생성해 렌더러에 제출하면, 렌더러가 **절두체 컬링, 디퍼드 여부, 환경광 처리** 등 필요한 옵션을 자동으로 적용해 렌더링이 진행되도록 설계했습니다.

``` mermaid
classDiagram
    class MeshData {
	    +vertexBuffer : RendererBuffer
	    +indexBuffer : RendererBuffer
	    +indexCounts : uint32_t
	    +vertexStride : uint32_t
	    +vertexShader : VertexShader
	    +shaderResources : BinadbleVector

	    +boundingBox : DirectX::BoundingOrientedBox 
    }
    
    class MaterialData {
	    pixelShader : PixelShader
	    shaderResources : BinadbleVector
    }

    class MeshDrawCommand {
        +meshData : MeshData
        +materialData : MaterialData
    }
    
    MeshDrawCommand o-- MeshData
    MeshDrawCommand o-- MaterialData

```

라이브러리는 다음과 같은 다양한 명령(Command)을 제공합니다. 
* `MeshDrawCommand`
* `UIDrawCommand`
* `UIMaterialDrawCommand`
* `PostProcessCommand`
* `ParticleDrawCommand`
* `TextDrawCommand`

기존 그래픽 API는 GPU를 자유롭게 다루기 위해 가능한 모든 기능을 제공하지만,
**게임 렌더링 라이브러리**는 3D 게임 렌더링 패러다임에 맞춰 **적절한 수준의 추상화**를 제공하도록 설계했습니다.
이를 통해 엔진 구조가 더욱 깔끔해지고, 실제 게임 제작에서 효율적인 렌더링 파이프라인을 구축할 수 있다고 생각합니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}
## **PBR**
금속성과 거칠기만을 이용해 **빛의 반사**를 계산하는  
Cook–Torrance 반사모델(BRDF)을 사용하여 금속성, 거칠기를 이용한 재질을 표현 하였습니다.  

![morning]({{ '/assets/MyLittleStorage/Grapic.png' | relative_url }}){: style="width: 100%;" }

<br>

**환경광은 IBL을 이용하였으며** 환경광의 **난반사는 Irradiance Map**, **정반사는 Prefiltered Environment Map**과 **BRDF LUT**로 처리해  
자연스럽고 환경광 효과를 구현했습니다.


IBL을 씬마다 다르게 사용하여
아침, 저녁, 밤 등 다양한 시간대의 분위기를 연출할 수 있었습니다.
![morning]({{ '/assets/MyLittleStorage/morning.png' | relative_url }}){: style="width: 32%;" }
![eveningnight]({{ '/assets/MyLittleStorage/eveningnight.png' | relative_url }}){: style="width: 32%;" }
![night]({{ '/assets/MyLittleStorage/night.png' | relative_url }}){: style="width: 32%;" }

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **쉐도우맵핑**
직접 광선 추적 대신 **쉐도우맵**을 이용해 그림자를 계산했습니다.  
광원에서 깊이버퍼를 기록하고, 렌더링 시 해당 픽셀이 광원에서 차단되는지를 확인했습니다.  

초기에는 **PCF 3x3**으로 부드러운 그림자를 구현했습니다.  
거기에, 더 부드러운 그림자를 위해 **VSM**도 검토했으나 **개발 기간**과 **파이프라인 변경 위험**을 고려해  
**PCF 7x7**로 확장 적용하여 안정적인 결과를 얻었습니다.  

![PCF7x7]({{ '/assets/MyLittleStorage/PCF7x7.png' | relative_url }}){: style="width: 100%;" }


{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **지연된 렌더링**

PCF7x7과 BRDF를 사용함에 따라 **픽셀당 연산량이 크게 증가**했습니다.
이를 해결하기 위해 **지연 렌더링**을 도입했습니다.
즉, 실제 광원 계산은 필요한 픽셀에 대해서만 수행하도록 하고, 연산에 필요한 모든 데이터를 **GBuffer**에 저장한 뒤, 모든 불투명 오브젝트가 렌더링된 후 광원 계산을 수행합니다.

GBuffer 구성은 다음과 같습니다.

* **Albedo (RGB) + Metalness (A)**
* **Specular (XYZ) + Ambient Occlusion (A)**
* **Normal (XYZ) + Roughness (A)**
* **Emissive (RGB) + ShadingModelID (A)**

추가로, 남은 4개의 버퍼는 콘텐츠 확장을 위해 남겨두었고, 그중 하나는 외곽선 색상을 저장하는 데 사용했습니다.

**포지션 정보**는 GBuffer에 포지션을 저장하지 않고, 각 픽셀의 텍셀 위치와 깊이버퍼 값을 이용해 **뷰-투영 행렬의 역행렬**을 적용하여 복구하였습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **후처리 구조**
**좋은 퀄리티**의 게임을위해 렌더링 결과물을 변형해주는
**이미지 후처리** 시스템을 제작했습니다.

**팩토리 패턴**을 사용하여 `PostProcessData`를 상속받은 클래스를 **툴에서 생성**하여 후처리 컴포넌트에	서 사용할 수 있도록 했습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
### 외곽선 추출

외곽선 추출에는 다양한 방법이 있지만, 아트팀의 요구사항은 **오브젝트 단위로 덩어리진 외곽선**이었습니다.
이를 위해 GBuffer의 남는 버퍼에 **게임 오브젝트의 뷰좌표계 Z값**을 기록하고, 이 Z값을 **라플라시안 필터**로 처리하여 외곽선을 추출했습니다.

또한 캐주얼 타이쿤 게임의 특성에 맞게, **외곽선의 색상과 두께를 아트작업자가 지정할 수 있도록** 구현했습니다.
외곽선의 두께는 **Box Blur**를 사용해 원하는 만큼 확장하여 적용했습니다.
<br>

박스 블러시에 샘플링 횟수를 줄이기위해 가로, 세로로 나누어 2회에 걸쳐 블러를 적용했습니다.

<br>

![Edge]({{ '/assets/MyLittleStorage/Edge.png' | relative_url }}){: style="width: 100%;" }
{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
### 블룸
블룸은 블룸 커브를 사용하여 **밝은 부분을 추출**후,
**다운샘플링, 가우시안 블러, 업샘플링**을 거쳐
원본 이미지에 더해주는 방식으로 구현했습니다.

이때 블룸 커브는 다양한 효과를 위해 **3가지 블룸 커브**를 제공했습니다.
* **헤일로3**: 밝은 영역만 부드럽게 강조
* **Quadratic**: 밝기 제곱으로 밝은 영역을 강하게 강조
* **Threshold Squared**: 일정 밝기 이상만 극단적으로 강조

![Bloom1]({{ '/assets/MyLittleStorage/Bloom1.png' | relative_url }}){: style="width: 100%;" }

```cpp
//sudocode
void Bloom()
{
    //1. 밝은 영역 추출
    ExtractBloomCurve();

    //2. 다운샘플
    DownSample6x6();
    DownSample6x6();

    //3. 블러 + 업샘플링/합성 반복
    AddTexture();        
    GaussianBlur5x5();
    
    AddTexture();        
    GaussianBlur5x5();
    
    AddTexture();        
    GaussianBlur5x5();
	
    // 4. 최종 블룸 결과를 원본 이미지에 합성
    BloomAdd();
}
```

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
### 톤 매핑
톤 매핑은 **HDR로 렌더링된 결과물**을 **LDR로 변환**하는 과정입니다.
이를위해 **다양한 톤 매핑 알고리즘**을 제공했습니다.
* **Reinhard**: 간단하고 빠르며, 밝은 영역을 부드럽게 압축
* **ACES**: 영화 산업 표준, 넓은 색역과 높은 명암비 지원
* **Uncharted 2**: 게임 'Uncharted 2'에서 사용, 밝은 영역을 선명하게 유지하면서도 자연스러운 압축
* **None**: 톤 매핑 비활성화

톤매핑 함수에는 **감마 인코딩도 포함**되어있어
None시에는 감마 인코딩만 수행하여 톤맵핑 결과물은 감마인코딩된 LDR 이미지가 됩니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **파티클 GPU 계산, 렌더링**

게임의 다양한 표현을 위해 파티클 시스템을 제작하게되었습니다.  
빌보드 텍스처를 수많은 점에서 렌더링을 할필요가 있었습니다.
버텍스버퍼를 세팅하지않고 파티클갯수만큼 그리기명령후 `SV_VERTEXID`를 이용하여 파티클 배열의 인덱스접근하였습니다. 

```cpp
struct Particle
{
	float3 position;
	float lifeTime;
	
	float3 velocity;
	float elapsedTime;
	
	float3 acceleration;
	float pad;
	
	float3x3 rotation;
};

```
그다음 점위좌표에서 GS를 이용하여 빌보드텍스처로 변환 하였습니다.
PS에서는 머티리얼에셋을 셋팅하여 렌더링 하였습니다.

Particle의 포지션, 생명주기는  CS를 이용하여 계산해주었습니다.
```cpp
UINT bindUAVCount[3]{ drawCommands.instanceCount, 0, 0 };
bindUAV[0] = drawCommands.addParticleBuffer[0];
bindUAV[1] = drawCommands.particleBuffer;
bindUAV[2] = drawCommands.deadParticleBuffer;

bindSRV[0] = drawCommands.addParticleBuffer[1];
bindCB[0] = drawCommands.optionBuffer;

immediateContext->CSSetShader(particleComputeShader, nullptr, 0);
immediateContext->CSSetUnorderedAccessViews(0, std::size(bindUAV), bindUAV, bindUAVCount);
immediateContext->CSSetShaderResources(0, std::size(bindSRV), bindSRV);
immediateContext->CSSetConstantBuffers(4, std::size(bindCB), bindCB);

immediateContext->Dispatch(drawCommands.addParticleCount / 64 + 1, 1, 1);

immediateContext->CSSetUnorderedAccessViews(0, std::size(nullUAV), nullUAV, nullptr);
immediateContext->CSSetShaderResources(0, std::size(nullSRV), nullSRV);
immediateContext->CSSetConstantBuffers(0, std::size(nullCB), nullCB);
```


![Edge]({{ '/assets/MyLittleStorage/Particle.gif' | relative_url }}){: style="width: 100%;" }

<br>

빌보드뿐아니라 이동방향으로 텍스처를 회전시키는것까지 만들었습니다.

![Edge]({{ '/assets/MyLittleStorage/Particle3.gif' | relative_url }}){: style="width: 100%;" }

![Edge]({{ '/assets/MyLittleStorage/Particle2.gif' | relative_url }}){: style="width: 100%;" }
{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **스키닝**
``` mermaid
---
  config:
    class:
      hideEmptyMembersBox: true
---
classDiagram
    class MeshComponent
    class MeshAsset
    class SkeletonAsset
    
    class MeshComponent {
        +meshAsset : MeshAsset [0..1]
        +tranformBuffer : GraphicsBuffer
    }
    class MeshAsset {
        +skeletonAsset : SkeletonAsset [0..1]
    }

    MeshComponent o-- "0..1" MeshAsset
    MeshAsset o-- "0..1" SkeletonAsset
```

메쉬컴포넌트에서 메쉬에셋이 스켈레톤에셋을 가지고있는지 확인하여 **SkeletalMeshVS**을 사용할지 **StaticMeshVS**을 사용할지 선택하게 하였습니다.
`TranformBuffer`또한 **스켈레톤에셋여부**에따라 오브젝트데이터를 담은**상수버퍼**를 사용할지 본정보를담은 **구조화된 버퍼**를 사용할지 선택하게 하였습니다.

<br>

### 애니메이션

``` mermaid
---
  config:
    class:
      hideEmptyMembersBox: true
---
classDiagram
    class MeshComponent
    class AnimationComponent
    class AnimationAsset

    class AnimationComponent {
        +meshComponent : MeshComponent
        +animationAsset : AnimationAsset
    }

    class MeshComponent {
        +tranformBuffer : GraphicsBuffer
    }

    AnimationComponent o-- MeshComponent
    AnimationComponent o-- AnimationAsset
```

애니메이션컴포넌트에서 애니메이션에셋을이용해 메쉬컴포넌트의 본정보를 업데이트하게 하였습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}
