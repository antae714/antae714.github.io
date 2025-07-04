---
title: "언리얼 데이터 테이블 응용"
date: 2025-07-04 00:00:00
layout: post
image: "images/UnrealLogo.png"
subtitle: 
 - "📄 CSV 기반 데이터테이블의 에셋 경로 불편함 해결"
 - "😊 ID → 경로 매핑 방식"
 - "😨 수동 매핑의 한계"
 - "😀 에셋 레지스트리 기반 자동 매핑"
description: "언리얼 데이터 테이블을응용한 경험에대해 설명합니다"
published: true
order: 9700
AutoContents: true
---

{% capture paragraph %}

# 📄 CSV 기반 데이터테이블의 에셋 경로 불편함 해결
언리얼엔진의 데이터테이블은 기본적으로 UClass나 UObject를 직접 행에 저장할 수 있습니다. 
하지만 이 데이터를 CSV로 익스포트하거나 외부에서 편집하려고 할 때, 경로(예: /Game/...) 형식으로 노출되기 때문에 편집이 번거로워지고, 
CSV 편집의 이점이 줄어듭니다.

기획자가 직접 CSV를 수정해야 하는 경우,
에셋 경로를 일일이 적는 것보다 의미 있는 ID를 작성하고 그 ID로 에셋을 간접 참조할 수 있다면 더 직관적이고 안전한 워크플로우가 될 수 있습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# 😊 ID → 경로 매핑 방식
이 문제를 해결하기 위해 ID와 에셋 경로를 연결하는 맵을 만들었습니다.
``` cpp
TMap<FName, FSoftObjectPath> AssetPathMap;
```
- `FName` ID를 키로 사용하고,
- `FSoftObjectPath`로 경로를 저장합니다.

덕분에 기획자가 CSV를 편집할때는 ID만 입력하면 되고,
맵에서 ID에 해당하는 경로를 찾아서
후에 해당 경로를 이용하여 `UClass`, `UObject`로 변환할수있게 되었습니다.

> 💡 Tip!
> 1. 이때 ID는 `FName`으로 저장하는것을 추천합니다.
> `FName`은 문자열을 해시값으로 저장하기 때문에, 문자열을 비교할때 성능이 좋습니다.
> 2. `UDeveloperSettingss`를 이용하여 맵을 저장하면,
> 프로젝트 설정에서 쉽게 수정할수있습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# 😨 수동 매핑의 한계
하지만 이 방식에도 단점이 있습니다. `TMap`을 블루프린트에서 수동 편집할 경우:
- 오타나 경로 누락 등 실수가 발생하기 쉽고
- 에셋 변경 시마다 수동으로 동기화해야 합니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}


# 😀 에셋 레지스트리 기반 자동 매핑
이 문제를 해결하기 위해 에셋 레지스트리를 활용해 매핑 정보를 자동 생성하는 방법을 도입했습니다.

`AssetRegistry`는 프로젝트 내 모든 에셋의 정보를 관리하며,
`FAssetData` 구조체를 통해 최소한의 정보를 제공합니다.

일반적으로 `AssetRegistrySearchable` 속성이 부여된 프로퍼티는
CDO 기준으로 `FAssetData`에 노출됩니다. 그러나 이 방식은 텍스처, 메시 등
비오브젝트 에셋에는 적용되지 않으므로, 다른 전략을 택했습니다.

📌 이름 기반 자동 매핑 규칙
- `ID == 에셋` 이름 규칙을 정해두고,
- 레지스트리에서 이름이 일치하는 에셋을 검색해 자동 매핑합니다.

또한 `CallInEditor` 메타를 이용해 에디터에서 버튼 클릭으로 매핑을 갱신할 수 있게 만들었습니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}
