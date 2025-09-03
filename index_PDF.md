---
layout: MainPostPDF
title: 안태현
subtitle: 
  - "소개"
  - "프리뷰"
  - "참여 프로젝트"
  - "언리얼 프로젝트"
  - "자체엔진 프로젝트"
  - "써드파티"
mermaid: true
target_posts:
  - "인벤토리 시스템 제작"
  - "AI 시스템 제작"
  - "언리얼 매칭 시스템 제작"
  - "언리얼 게임 관전하기"
  - "언리얼 게임 진행도 저장하기"
  - "머티리얼 노드 에디터 제작"
  - "그래픽스 렌더링 엔진 제작"
  - "턴제관리 시스템 제작"
---

{% capture LinkCapture %}

[Link_InventorySystem_Develop]: #인벤토리-시스템-제작
[Link_AI]: #ai-시스템-제작
[Link_UnrealMatching]: #언리얼-매칭-시스템-제작
[Link_Spectator]: #언리얼-게임-관전하기
[Link_SaveGame]: #언리얼-게임-진행도-저장하기
[Link_MaterialEditor]: #머티리얼-노드-에디터-제작
[Link_GrapicsRendering]: #그래픽스-렌더링-엔진-제작
[Link_TurnManager]: #턴제관리-시스템-제작

{% endcapture %}


{% include index_content.md Link=LinkCapture %}
<br> <br> <br> <br>


{% assign ordered_posts = '' | split: '' %}

{% for title in page.target_posts %}
  {% assign found_post = site.posts | where: "title", title | first %}
  {% if found_post %}
    {% assign ordered_posts = ordered_posts | push: found_post %}
  {% endif %}
{% endfor %}

{% for post in ordered_posts %}
<div class="post-content">

# {{ post.title }} {#{{ post.title | slugify }}}
---
{{ post.content }}  

</div>
{% endfor %}


