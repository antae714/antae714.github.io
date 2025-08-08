---
title: "언리얼 데이터 테이블 경로데이터 CSV이용시 애로사항"
date: 2025-07-04 00:00:00
layout: post
image: "images/UnrealLogo.png"
subtitle: 
 - "📄 CSV 기반 데이터테이블의 에셋 경로 불편함 해결"
 - "😊 ID → 경로 매핑 방식"
 - "😨 수동 매핑의 한계"
 - "😀 에셋 레지스트리 기반 자동 매핑"
description: "언리얼 데이터 테이블에서 경로데이터을 CSV이용시 애로사항에 대해 이야기 합니다."
published: true
order: 9700
AutoContents: true
---

{% capture paragraph %}

# **📄 CSV 기반 데이터테이블의 에셋 경로 불편함 해결**
언리얼 엔진의 데이터테이블은 기본적으로 `UClass`나 `UObject`를 직접 행에 저장할 수 있습니다.  
하지만 이 데이터를 CSV로 익스포트하거나 외부에서 편집할 때, 경로(예: `/Game/...`) 형식으로 노출되기 때문에 편집이 번거롭고 CSV 편집의 장점이 줄어듭니다.

기획자가 직접 CSV를 수정해야 하는 경우, 에셋 경로를 일일이 입력하는 것보다 의미 있는 **ID**를 작성하고 그 ID로 에셋을 간접 참조할 수 있다면 더 직관적이고 안전한 워크플로우가 됩니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# **😊 ID → 경로 매핑 방식**
이 문제를 해결하기 위해 **ID와 에셋 경로를 연결하는 맵**을 만들었습니다.
``` cpp
TMap<FName, FSoftObjectPath> AssetPathMap;
```
- `FName` ID를 키로 사용
- `FSoftObjectPath`로 경로 저장

덕분에 기획자가 CSV를 편집할 때는 ID만 입력하면 되고,
맵에서 ID에 해당하는 경로를 찾아 UClass/UObject로 변환할 수 있습니다.

💡 **Tip**
1. ID는 `FName`으로 저장하는 것을 추천합니다. `FName`은 문자열을 해시값으로 저장하기 때문에 비교 성능이 좋습니다.
2. `UDeveloperSettings`를 이용하면 프로젝트 설정에서 쉽게 수정할 수 있습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# **😨 수동 매핑의 한계**
그러나 이 방식에는 단점이 있습니다.

- 오타나 경로 누락 등 실수가 발생하기 쉽다.
- 에셋 변경 시마다 수동으로 동기화해야 한다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}


# **😀 에셋 레지스트리 기반 자동 매핑**
이 문제를 해결하기 위해 **에셋 레지스트리**를 활용한 자동 매핑 방식을 도입했습니다.

- **AssetRegistry**는 프로젝트 내 모든 에셋 정보를 관리합니다.
- `FAssetData` 구조체를 통해 최소한의 정보(이름, 경로 등)를 제공합니다.
- `AssetRegistrySearchable` 속성을 가진 프로퍼티는 CDO 기준으로 `FAssetData`에 노출되어 검색이 가능해집니다.
- 하지만 텍스처, 메시 등 비오브젝트 에셋에는 적용되지 않기 때문에 다른 방법을 사용했습니다.

📌 **이름 기반 자동 매핑 규칙**
- `ID == 에셋 이름` 규칙을 정의
- 레지스트리에서 이름이 일치하는 에셋을 검색해 자동 매핑

또한, `CallInEditor` 메타를 이용해 **에디터 버튼 클릭만으로 매핑 갱신**이 가능하게 만들었습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}
