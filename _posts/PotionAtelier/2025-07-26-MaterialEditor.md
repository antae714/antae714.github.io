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
---

{% capture paragraph %}
## **초기 목표**
모델링툴에서 사용하는 쉐이더코드와 게임엔진의 쉐이더코드, 후처리 절차가 다르기에
게임엔진에서는 모델링툴과	동일한 색이표현되지 않는 문제를 알게 되었습니다.
아트작업자가 게임엔진에서 원하는 색표현을 위해 꼭 필요한 기술이라 생각되었고,
상용엔진에서도 사용하는만큼 최신 게임 트랜드에 맞는 기술이라 생각되어 제작하게 되었습니다.

오픈소스 라이브러리를 찾아보았으나 `MaterialX`는 OpenGL기반으로 제작되어있어
DirectX기반의 엔진에서 사용하기에는 무리가 있어 직접 제작하게 되었습니다.

언리얼 엔진처럼 `MaterialX`의 포맷을 읽어서 엔진에맞게 생성할수도있지만
그전에 머티리얼관련 기술이 엔진에 있어야 이식할수 있다고 생각햇기떄문에
머티리얼 노드 에디터를 제작하게 되었습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **초기 목표**

소스엔진의 Hammer 에디터처럼

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}
## **과정**

### 표면 머티리얼


### UI머티리얼

### 에디터 작업시 필수사항
에디터제작 작업은 사용자가 작업을 하면서 병렬적으로 개발이 진행되어야 한다는것을 알게되었습니다.
에디터에 아무리 많은 기능이 있다고 해도 사용자가 다른기능을 원하면 힘들게 됩니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **문제점**

머티리얼 인스턴스 개념을 모르고 제작했기때문에
머티리얼 노드마다 공유가 되지않겟다라고 생각햇고 
머티리얼 리소스 재활용이 힘들다보니
머티리얼이 추가될때마다 쉐이더 컴파일시간이 늘어나 로딩시간이 급격하게 늘어났습니다.
쉐이더 컴파일이 70퍼센트 텍스처리소스 로딩이 20퍼센트를 먹는 상황이엿기에
비동기 컴파일, 컴파일된 쉐이더를 캐싱하여 재활용 하게끔 하였습니다.

``` mermaid
pie showData
    title 로딩시간 
    "쉐이더 컴파일": 75
    "텍스처 리소스 로딩": 20
    "기타": 5

```


시간이 된다면 머티리얼 인스턴스 개념을 도입하여 
쉐이더는 하나 그에상응하는 파라미터를 조절하는 방식으로 변경할 예정입니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}
