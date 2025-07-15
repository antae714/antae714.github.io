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

{% include head.html %}


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
- **[머티리얼 노드 에디터 제작]()**
- **[파티클 시스템 제작]()**
- **[PBR & IBL 구현]()**
- **[헤일로3 블룸 제작]()**
- **[외곽선 추출 알고리즘 제작 (Laplacian Filter)]()**
- **[쉐도우 맵핑 제작]()**
- **[FBX 임포트 파이프라인 제작]()**
- **[스키닝 제작]()**
- **[자원관리 기법 구현]()**
- **[텍스처에 압축 툴 제작]()**
- **[Fmod Sound Bank 사용]()**




{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# **Rail Way To Hell**
<p align="center">
 <img src = "images/2.png" style="width: 100%;">
</p>
## 작업내용
-  **[턴제관리 시스템 제작]()**



{% endcapture %}
{% include paragraph.html content=paragraph %}




{% comment %}
{% assign filtered_posts = site.posts | where_exp: "post", "post.path contains 'UnrealProject'" %}
{% assign sorted_posts = filtered_posts | sort: "order" | reverse  %}
<div class="Paragraph">
    <h1 id = "언리얼-프로젝트">언리얼 프로젝트</h1>
    <div class="card-container">
        {% for post in sorted_posts %}
        <div class="card" onclick="openPopup(this)" data-url="{{ post.url }}" style="cursor: pointer;">
  <div class="card-image-wrapper">
      <img class="card-image default-image" src="{{ post.image | default: 'images/UnrealLogo.png' }}" alt="{{ post.title }}" onerror="this.onerror=null; this.src='images/UnrealLogo.png';">
      <img class="card-image hover-image" src="{{ post.hover_image | default: post.image }}" alt="{{ post.title }}" onerror="this.onerror=null; this.src='images/UnrealLogo.png';">
  </div>
  <div class="card-text">
      <div class="card-text-Title">{{ post.title }}</div>
      <div class="card-text-Content">
          {{ post.description | default: post.excerpt | strip_html | truncate: 80 }}
      </div>
  </div>
</div>
        {% endfor %}
    </div>
</div>

{% assign filtered_posts = site.posts | where_exp: "post", "post.path contains 'PotionAtelier'" %}
{% assign sorted_posts = filtered_posts | sort: "order" | reverse  %}
<div class="Paragraph">
    <h1 id="자체엔진-프로젝트">자체엔진 프로젝트</h1>
    <div class="card-container">
        {% for post in sorted_posts %}
        <a href="{{ post.url }}" class="card">
        <div class="card-image-wrapper">
            <img class="card-image default-image" src="{{ post.image | default: 'images/UnrealLogo.png' }}" alt="{{ post.title }}"  onerror="this.onerror=null; this.src='images/UnrealLogo.png';">
            <img class="card-image hover-image" src="{{ post.hover_image | default: post.image }}" alt="{{ post.title }}"  onerror="this.onerror=null; this.src='images/UnrealLogo.png';">
        </div>
        <div class="card-text">
            <div class="card-text-Title">
                {{ post.title }}
            </div>
            <div class="card-text-Content">
                {{ post.description | default: post.excerpt | strip_html | truncate: 80 }}
            </div>
        </div>
        </a>
        {% endfor %}
    </div>
</div>

<div class="Paragraph">
<h1 id="써드파티">써드파티</h1>
</div>

{% endcomment %}
{% comment %}
{% assign filtered_posts = site.posts | where_exp: "post", "post.path contains 'UnrealProject'" %}
{% assign sorted_posts = filtered_posts | sort: "order" | reverse  %}
<div class="Paragraph">
<h1>테스트용 언리얼 프로젝트</h1>
<div class="post-list-container">
  {% for post in sorted_posts %}
    <div class="post-list-element">
        <a href="{{ post.url }}" class="Link">
            <div class="post-list-image">
	            <img src="{{ post.image | default: 'ㅁㄹ.gif' }}" alt="{{ post.title }}" style="float: right;">
            </div>
            <h3>{{ post.title }}</h3>
            <p>{{ post.description | default: post.excerpt | strip_html | truncate: 80 }}</p>
        </a>
    </div>
  {% endfor %}
</div>
</div>
{% endcomment %}


<!--
> 1. [언리얼 인벤토리 시스템 제작](UnrealProject/InventorySystem_Develop.md)
> 1. [언리얼 어빌리티 시스템 사용/분석](UnrealProject/)
> 2. [언리얼 현지화 시스템 사용/분석](UnrealProject/)
> 1. [언리얼 게임 매칭 하기](UnrealProject/)
> 1. [언리얼 게임 진행도 저장하기](UnrealProject/)
> 1. [언리얼 UI 팁](UnrealProject/)
> 1. [언리얼 애니메이션 팁](UnrealProject/)
> 1. [언리얼 에셋 C++에서 사용하기](UnrealProject/)

# 자체엔진 프로젝트
> <img src="ㅁㄹ.gif" width="200" />
>
> 1. [머리티얼 노드 에디터 제작](InHouseEngineProject/없음.md)
> 1. [파티클 시스템 제작](InHouseEngineProject/없음.md)
> 1. [후처리 기법 구현](InHouseEngineProject/없음.md)
> 1. [FBX파일 임포트 파이프라인 제작](InHouseEngineProject/없음.md)
> 1. [스키닝 구현](InHouseEngineProject/없음.md)
> 1. [쉐도우 맵핑 구현](InHouseEngineProject/없음.md)
> 1. [물리 기반 렌더링 구현](InHouseEngineProject/없음.md)
> 1. [자원관리 기법](InHouseEngineProject/없음.md)
> 1. [텍스처에 대하여](InHouseEngineProject/없음.md)

# 써드파티
> 1. [FMOD](ThirdParty/없음.md)
> 1. [Git Hub](ThirdParty/없음.md)
> 1. [SVN](ThirdParty/없음.md)

-->
