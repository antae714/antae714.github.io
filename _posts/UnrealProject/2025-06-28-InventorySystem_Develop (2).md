---
title: "인벤토리 시스템 제작 (2)"
description: "언리얼 배열 네트워크 직렬화에 대해 이야기합니다"
date: 2025-06-27 00:00:01
layout: post
image: "images/icon_6.gif"
subtitle: 
 - "언리얼 배열 네트워크 직렬화"
published: true
order: 103
---

{% capture paragraph %}

## **언리얼 배열 네트워크 직렬화**

### 배열 직렬화 과정의 의문
언리얼에서 아이템을 배열로 관리하는 인벤토리 컴포넌트를 개발하면서 문득 
"배열의 요소 하나만 변경되어도 전체 배열이 직렬화되는 것인가?"라는 의문이 생겼습니다. 
조사를 진행한 결과, 실제로 일반적인 배열은 하나의 요소만 변경되어도 배열 전체가 다시 직렬화되는 비효율적인 구조라는 것을 알게 되었습니다.
<br><br>

### FastArraySerializer를 통한 최적화
이러한 비효율성을 해결하기 위한 방법으로 `FastArraySerializer`를 찾게 되었습니다. 
`FastArraySerializer`를 사용하면 배열의 전체를 직렬화하지 않고, 
변경된 요소만 선택적으로 직렬화하여 네트워크 부하를 크게 줄일 수 있습니다.
<br><br>

### FastArraySerializer는 결국 극한의 RPC?
`FastArraySerializer`의 내부 동작 방식은 배열의 요소가 변경될 때마다 해당 요소에 더티 마크를 표시하고, 
변경된 항목만을 ReplicationID를 기반으로 RPC 형태로 클라이언트에 전송합니다. 
다만, `FFastArraySerializerItem`을 상속받은 구조체를 통째로 Swap할 때는 ReplicationID가 유지되기 때문에 실제 변경된 데이터가 있어도 클라이언트로의 전송이 이루어지지 않을 수 있는 주의점이 있습니다. 
이는 효율성을 높이지만, 구현 시 주의가 필요한 부분입니다.
<br><br>

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
