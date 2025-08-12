---
title: "퀵슬롯 HUD 제작"
date: 2025-08-12 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "소개"
AutoContents: false
---

{% capture paragraph %}
## **내구도 표시**
플레이어가 도구 아이템을 사용할시 내구도의 변화를 보여주기위해 제작되었습니다.
프로그래스바를 사용하였으며 프로퍼티 바인딩으로 `percent`를 표시합니다.

![사진]()


{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **갯수 표시**
도구가 아닌 중첩가능한 아이템을 표현시 몇개를 가지고있는지를 보여주기위해 제작되었습니다.

![사진]()

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **선택창 표시**
아이템이 선택되었다는 느낌을 주기위해 제작되었습니다.  
[UI 머티리얼 랩](https://www.fab.com/listings/69680f34-e5d2-44e6-b023-f054bbf629eb)
으로 제작되었으며 스케일또한 키워서 선택된 느낌을 주도록 하였습니다.
스케일을 키울때 비율문제로 스케일박스를 사용하였습니다.

![움짤]()


{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **쿨타임 표시**
아이템이 사용되었을때 쿨타임이 얼마나 남았는지를 보여주기위해 제작되었습니다.

![움짤]()

{% endcapture %}
{% include paragraph.html content=paragraph %}


