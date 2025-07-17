---
title: "언리얼 버전관리툴 바이너리 컨플릭트"
date: 2025-07-17 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "언리얼 버전관리 방법에대해 이야기합니다."
AutoContents: false
---

{% capture paragraph %}
## **바이너리파일의 문제점**
SVN, Git, Perforce 등 버전관리툴은 텍스트파일의 변경사항을 추적하는데 최적화되어 있습니다.
바이너리 파일은 텍스트파일과 달리 변경사항을 추적하기 어렵습니다.
때문에 바이너리 파일을 버전관리툴로 관리할 때는 작업자 간의 충돌이 발생할 수 있습니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **리비전 컨트롤**
언리얼에서는 해당 문제를 해결하기 위해 리비전 컨트롤 시스템을 사용합니다.
바이너리파일(블루프린트)끼리 비교하고 수정사항을 추적할 수 있는 시스템입니다.
SVN에서는 타 작업자의 파일의 변경을 막는 락기능으로 체크인, 체크아웃을 사용합니다.
그럼에도 컨플릭트가 발생할 수 있습니다.
그때는 변경사항끼리 합치는 머지 기능을 사용합니다.


```
 리비전 컨트롤 사진
```

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **SVN 브랜치 머지시 이슈**
하지만 언리얼의 블루프린트 머지기능은 브랜치로 나누어 작업할 때 이슈가 발생합니다.
SVN의 타 브랜치간의 작업이 머지될 때 일어난 충돌은 언리얼의 머지툴로 해결할 수 없습니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **결국엔 옛날방법**
바이너리 파일이 충돌날때마다 각각 어떤 작업을 했는지 확인하고
작업자간의 회의후에 수동으로 머지하는 방법을 사용해야 했습니다.

블루프린트노드같은경우엔 코멘트로 어떤 작업을 했는지 남겨두어 어떤작업자가 어떤 작업을 했는지 한눈에 볼 수 있도록 했습니다.

```
 코멘트 사진
```


{% endcapture %}
{% include paragraph.html content=paragraph %}
