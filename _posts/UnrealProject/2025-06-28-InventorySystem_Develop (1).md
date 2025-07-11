---
title: "인벤토리 시스템 제작 (1/3)"
description: "언리얼 오브젝트 리플리케이션 처리에대해 이야기합니다"
date: 2025-06-27 00:00:00
layout: post
image: "images/UObjectReplicated.png"
hover_image: "images/UObjectReplicated_Hover.png"
subtitle: 
 - "🤔 언리얼 오브젝트 기반 아이템 제작"
 - "😨 리플리케이션 문제의 발견"
 - "😮 서브오브젝트를 통한 리플리케이션 해결"
published: true
order: 10002
AutoContents: true
---

{% capture paragraph %}

## **🤔 언리얼 오브젝트 기반 아이템 제작**
아이템의 구조를 단순한 구조체 대신 언리얼 오브젝트 기반으로 설계하여, 
언리얼 엔진의 블루프린트 기능을 적극 활용하고자 했습니다. 
각 아이템을 별도의 블루프린트 클래스로 정의하고, 
상속을 통해 아이템별로 고유한 함수를 오버라이드함으로써 확장성과 유지보수성을 높일 수 있기 때문입니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **😨 리플리케이션 문제의 발견**
하지만 언리얼 엔진의 네트워크 리플리케이션은 액터 기반으로 이루어지며, 
기본적으로 언리얼 오브젝트는 리플리케이션이 되지 않는다는 사실을 간과했습니다. 
언리얼 오브젝트 대신 구조체를 사용하면 쉽게 리플리케이션이 가능했지만, 
구조체는 상속 구조를 지원하지 않기에 원하는 설계와 맞지 않았습니다. 
RPC를 통한 동기화 방법도 검토했지만, 객체 상태의 일관성 유지가 어렵다는 근본적인 문제가 있었습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **😮 서브오브젝트를 통한 리플리케이션 해결**
추가적인 리서치를 통해 언리얼 엔진에서는 언리얼 오브젝트를 액터의 서브오브젝트로 등록하면 리플리케이션이 가능하다는 것을 알게 되었습니다. 
이 방법을 활용하여 인벤토리 컴포넌트에 아이템이 추가되거나 제거될 때마다 해당 아이템을 서브오브젝트로 등록하여 상태를 서버와 클라이언트 간에 안정적으로 동기화할 수 있었습니다. 
이로써 일반 아이템뿐 아니라 던지기형, 설치형 아이템 등 다양한 아이템 유형을 효과적으로 관리할 수 있게 되었습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


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
