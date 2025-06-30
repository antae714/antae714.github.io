---
layout: MainPost
title: 메인 페이지
---

{% include head.html %}

<div style="
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  z-index: -1;
  overflow: hidden;
">
  <!-- 블러 이미지 -->
  <img src="images/CoverImage.png" 
       style="width: 100%; height: 100%; object-fit: cover; filter: blur(4px);" />

  <!-- 순백색 오버레이 (밝은 분위기용) -->
  <div style="
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background-color: rgba(255, 255, 255, 0.5); /* 밝기 조절: 0.2 ~ 0.5 */
    mix-blend-mode: lighten; /* 또는 screen */
  "></div>
</div>



{% capture paragraph %}

<img src="images/ProfileImage2.png" 
     width="300" height="300"
     style="border-radius: 50%; border: 5px solid white; " />
### **즐거운 게임을 즐겁게, 재밌는 게임을 재밌게 만들고 싶은 프로그래머입니다.**

## 연혁
- 2020.01.26 ~ 2021.08.01: 육군 병장 만기제대
- 2021.08.01 ~ 2024.03.01: 학점은행제 게암프로그래밍과 수료
- 2024.03.01 ~ 현재: 게임인재원
<br><br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}



<div class="Paragraph">
<h1>참여 프로젝트</h1>



</div>




{% assign filtered_posts = site.posts | where_exp: "post", "post.path contains 'UnrealProject'" %}
{% assign sorted_posts = filtered_posts | sort: "order" | reverse  %}
<div class="Paragraph">
    <h1>언리얼 프로젝트</h1>
    <div class="card-container">
        {% for post in sorted_posts %}
        <a href="{{ post.url }}" class="card">
        <div class="card-image-wrapper">
            <img class="card-image default-image" src="{{ post.image | default: 'ㅁㄹ.gif' }}" alt="{{ post.title }}">
            <img class="card-image hover-image" src="{{ post.hover_image | default: post.image }}" alt="{{ post.title }}">
        </div>
        <div class="card-text">
            <div class="card-Title">
                <h3>{{ post.title }}</h3>
            </div>
        <div class="card-Content">
            <desc>{{ post.description | default: post.excerpt | strip_html | truncate: 80 }}</desc>
        </div>
    </div>
</a>
    {% endfor %}
    </div>
</div>
  

<div class="Paragraph">
<h1>자체엔진 프로젝트</h1>
</div>


<div class="Paragraph">
<h1>써드파티</h1>
</div>

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
