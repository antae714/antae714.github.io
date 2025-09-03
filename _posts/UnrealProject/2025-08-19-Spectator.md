---
title: "언리얼 게임 관전하기"
date: 2025-08-19 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "소개"
AutoContents: false
---

{% capture paragraph %}
## **관전하기**
게임진행도중 사망시 다른플레이어들의 게이트종료시점까지 관전할수있는 시스템을 제작하였습니다.

`APlayerController::ChangeState`를 사용하여 `NAME_Spectating`로 변경하여, 기존폰의 빙의를 해제하고 게임모드의 관전폰으로 빙의되었습니다.
빙의를 해제하고 관전폰으로 빙의하는 과정에서 이전폰의 IMC를 해제하고 관전폰의 IMC를 활성화하여 입력을 구분하	였습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


