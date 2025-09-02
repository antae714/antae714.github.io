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
---

{% capture LinkCapture %}

[Link_InventorySystem_Develop]: #인벤토리-시스템-제작
{% endcapture %}

{% include index_content.md Link=LinkCapture %}

<br> <br> <br> <br>



{% assign my_post = site.posts | where:"title","인벤토리 시스템 제작" | first %}
<div class="post-content">

# {{ my_post.title }} {#{{ my_post.title | slugify }}}
---
	{{ my_post.content }}
</div>



