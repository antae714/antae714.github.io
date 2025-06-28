---
layout: default
title: 메인 페이지
---


{% include head.html %}

<img src="images/ProfileImage.webp" width="200" />

**즐거운 게임을 즐겁게, 재밌는 게임을 재밌게 만들고 싶은 프로그래머입니다.**

# 언리얼 프로젝트
{% assign filtered_posts = site.posts | where_exp: "post", "post.path contains 'UnrealProject'" %}
{% assign sorted_posts = filtered_posts | sort: "order" | reverse  %}

<div class="card-container">
  {% for post in sorted_posts %}
    <a href="{{ post.url }}" class="card">
      <img src="{{ post.image | default: 'ㅁㄹ.gif' }}" alt="{{ post.title }}">
      <div class="card-text">
        <div class="card-Title">
          <h3>{{ post.title }}</h3>
          <desc>{{ post.description | default: post.excerpt | strip_html | truncate: 80 }}</desc>
        </div>
        <div class="card-Content">
          {% for SubTitleItem in post.subtitle %}
            <p>1. {{ SubTitleItem | default: post.excerpt | strip_html | truncate: 80 }}</p>
          {% endfor %}
        </div>
      </div>
    </a>
  {% endfor %}
</div>

# 자체엔진 프로젝트


# 써드파티

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
