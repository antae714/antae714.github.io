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
- **2000.07.14: 출생** 
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
 <img src = "images/CoverImage.png" style="width: 50%;">
</p>
<p style="text-align: right;">
  멀티플레이 헌터물 3D생존 게임입니다.
</p>

## **작업내용**
>### [인벤토리, 아이템 시스템](2025/06/27/InventorySystem_Develop.html)  
>![사진]()  
>![사진]()  
>![사진]()  
>![사진]() 

>### [AI 시스템](2025/07/15/AI.html)  
>![사진]()  
>![사진]()  
>![사진]()  
>![사진]() 

>### [매칭 시스템](2025/07/31/UnrealMatching.html)  
>![사진]()  
>![사진]()  
>![사진]()  
>![사진]() 

>### 게임 관전  
>![사진]()  
>![사진]()  
>![사진]()  
>![사진]() 

>### [게임 진행도 저장 시스템](2025/06/28/SaveGame.html)  
>![사진]()  
>![사진]()  
>![사진]()  
>![사진]() 



{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
# **Potion Atelier**
<p align="center">
 <img src = "images/PorionAtelier.png" style="width: 100%;">
</p>
<p style="text-align: right;">
  탑다운뷰 3D 타이쿤 게임입니다.
</p>



>## **[에디터/툴 제작](2025/07/26/MaterialEditor.html)**
>![사진]()  
>![사진]()  
>![사진]()  
>![사진]()  
<br>


>## **그래픽스 렌더링 엔진 제작**
>![사진]()  
>![사진]()  
>![사진]()  
>![사진]()  

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# **Rail Way To Hell**
<p align="center">
 <img src = "images/2.png" style="width: 100%;">
</p>
<p style="text-align: right;">
  D2D 퍼즐 게임입니다.
</p>

>## **기능 제작**
>-  **턴제관리 시스템 제작**
<br>

 

{% endcapture %}
{% include paragraph.html content=paragraph %}
