---
title: "플레이어정보 저장시 가방 액터 미저장"
date: 2025-08-09 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "소개"
AutoContents: false
---
{% capture paragraph %}
### **1. 문제 상황**

- **목표:** 플레이어 정보 저장 시, *착용 중인 가방 액터*도 함께 저장 → 로드 시 그대로 복원.
- **초기 설계:** `APlayerState::SeamlessTravel`에서  
  - `OldPlayerState` 저장 → `NewPlayerState` 로드.
- **문제:** 가방을 착용한 뒤 `SeamlessTravel`을 수행하면 **가방 액터가 저장되지 않음**.
{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
### **2. 원인 분석**

- `SeamlessTravel` 과정에서 **착용 중인 가방 액터가 `Destroy`**됨.
- 그 결과 포인터가 **`nullptr`**가 되어 저장 단계에서 **누락**.
{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
### **3. 해결 방법**

- **저장 시점 변경(책임 분리):**  
  `APlayerState` 내부가 아니라, **맵 이동 직전 외부에서 명시적으로 저장**하도록 수정.
  - 예: 트래블 호출부(또는 `GameInstanceSubsystem`)에서 **먼저 Save → 이후 SeamlessTravel**.
- **효과:** 트래블 중 `Destroy`되기 전에 저장되어 **가방 액터 정보가 정상 보존**.
{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
### **4. 변경 전·후 비교**

- **변경 전:** `SeamlessTravel` 중 저장 → 가방 액터가 이미 `Destroy`되어 **저장 실패**  
- **변경 후:** **트래블 직전 저장** → 가방 액터 유효 상태로 **저장 성공**
{% endcapture %}
{% include paragraph.html content=paragraph %}

