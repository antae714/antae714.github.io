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

  subgraph Runtime
    GA[Game Application]
  end

  subgraph Shared_Rendering
    RL[(Render Library)]
  end

  GA -->|uses| RL
  MNE -->|uses| RL


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
## **어떻게**

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **과정**





### 표면 머티리얼


### UI머티리얼

### 에디터 작업시 필수사항
에디터제작 작업은 사용자가 작업을 하면서 병렬적으로 개발이 진행되어야 한다는것을 알게되었습니다.
에디터에 아무리 많은 기능이 있다고 해도 사용자가 다른기능을 원하면 도로묵이되며 시간이라는 비용을 다른곳에 쓴꼴이 됩니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}
