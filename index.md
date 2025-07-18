---
layout: MainPost
title: 안태현
subtitle: 
  - "소개"
  - "프리뷰"
  - "참여 프로젝트"
  - "언리얼 프로젝트"
  - "자체엔진 프로젝트"
  - "써드파티"
---

{% comment %}

<video width="640" height="360" autoplay loop muted>
  <source src="/assets/MyLittleStorage/AA.mp4" type="video/mp4">
  브라우저가 video 태그를 지원하지 않거나 MKV 포맷을 지원하지 않습니다.
</video>

{% endcomment %}


<!-- 팝업 본체 -->
<div class="popup-overlay" id="popup" onclick="closePopup()">
<div class="popup-content" onclick="event.stopPropagation()">
<!-- iframe wrapper -->
<div class="iframe-wrapper">
<!-- 우측 상단 버튼 -->
<div class="popup-buttons">
<button onclick="expandPopup()" title="전체 페이지로 이동">🗖</button>
<button onclick="closePopup()" title="닫기">🗙</button>
</div>
<!-- 실제 iframe -->
<iframe id="popup-iframe" src=""></iframe>
</div>
</div>
</div>

{% comment %}
<a href="#" class="popup-link"  onclick="openPopup(this)" data-url="2025/06/28/Ability_System.html">팝업으로 열기</a>

{% endcomment %}


<a id="소개"></a>
{% capture paragraph %}
  
  


<img src="images/ProfileImage2.png" 
     width="300" height="300"
     style="border-radius: 50%; border: 5px solid white; " />
### **즐거운 게임을 즐겁게, 재밌는 게임을 재밌게 만들고 싶은 프로그래머입니다.**

## 연혁
- **2020.01.26 ~ 2021.08.01: 육군 병장 만기제대**
- **2021.08.01 ~ 2024.03.01: 학점은행제 게임프로그래밍과 수료**
- **2024.03.01 ~ 현재: 게임인재원**

<br>

## 기술
**C, C++, C#, WINAPI, Direct11, Unreal Engine, SVN, FMod**

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}
# **F Rank Hunter**

<p align="center">
 <img src = "images/CoverImage.png" style="width: 100%;">
</p>
<p style="text-align: right;">
  헌터물 생존 게임입니다.
</p>

## 작업내용
- **[게임플레이 어빌리티 시스템(GAS)을 이용한 캐릭터 능력 제작](2025/06/28/Ability_System.html)**
- **[아이템시스템 제작](2025/06/27/InventorySystem_Develop.html)**
- **[언리얼 데이터 테이블 응용](2025/07/04/DataTableApplication)**
- **[카오스디스트럭션 & 피직스필드를 이용한 파괴 시스템 제작](2025/07/08/StoneDestroy.html)**
- **[UI 제작](2025/06/28/UI)**
- **[게임 진행도 저장 시스템 제작](2025/06/28/SaveGame.html)**
- **[현지화 구현](2025/06/28/Localize.html)**
- **[AI 시스템 제작](2025/07/15/AI.html)**
- **더 나은 개발환경을 위한 치트매니저**
<br><br>

## 트러블 슈팅
- **후처리 머티리얼 해상도이슈**
<br><br>


## 협업 이슈
- **[버전관리툴 바이너리 컨플릭트](2025/07/17/UnrealSVN.html)**


{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
# **Porion Atelier**
<p align="center">
 <img src = "images/PorionAtelier.png" style="width: 100%;">
</p>
<p style="text-align: right;">
  음
</p>

## 작업내용
- **머티리얼 노드 에디터 제작**
- **파티클 시스템 제작**
- **PBR & IBL 구현**
- **블룸 제작**
- **외곽선 추출 알고리즘 제작 (Laplacian Filter)**
- **쉐도우 맵핑 제작**
- **FBX 임포트 파이프라인 제작**
- **스키닝 제작**
- **자원관리 기법 구현**
- **텍스처에 압축 툴 제작**
- **Fmod Sound Bank 사용**
<br><br>


## 트러블 슈팅
<br><br>


## 협업 이슈



{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# **Rail Way To Hell**
<p align="center">
 <img src = "images/2.png" style="width: 100%;">
</p>

## 작업내용
-  **턴제관리 시스템 제작**

<br><br>
## 트러블 슈팅
<br><br>

## 협업 이슈

{% endcapture %}
{% include paragraph.html content=paragraph %}
