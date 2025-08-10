---
title: "언리얼 매칭 시스템 제작"
date: 2025-07-31 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "언리얼 매칭 시스템 제작에 대해 이야기합니다"
AutoContents: false
---

{% capture paragraph %}
## **온라인 서브시스템**
언리얼의 **온라인 서브시스템(OSS)**은 공통 **인터페이스**를 통해 플랫폼에 종속되지 않는 온라인 기능을 제공합니다.  
Steam, Xbox Live, PlayStation Network 등 다양한 플랫폼을 동일한 코드 구조로 다룰 수 있어 **플랫폼 전환 비용**을 줄일 수 있습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **Advanced Sessions 플러그인**
기본 OSS의 세션 설정은 항목이 많고, **한두 가지 옵션만 누락돼도 검색이 실패**하는 경우가 잦습니다.  
`Advanced Sessions` 플러그인은 블루프린트/코드 양쪽에서 **세션 생성·검색·조인에 필요한 옵션을 한 곳에서 관리**하도록 도와 누락과 불일치를 줄여줍니다.

> 단, 플러그인이 모든 것을 대신해 주진 않습니다. **사용 중인 서브시스템(예: Steam) 플러그인 활성화**와 **설정 파일(Engine.ini) 구성**은 여전히 필수입니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}


{% capture paragraph %}
## **세션 세팅 (Create → Find → Join → Travel)**

### 1) 세션 생성
`IOnlineSessionPtr`을 통해 세션을 만들 때 상황에 맞게 다음 **핵심 키**를 설정합니다.
- 연결 수: `NumPublicConnections`  
- 광고/검색 허용: `bShouldAdvertise`  
- LAN/온라인: `bIsLANMatch`  
- 진행 중 참가: `bAllowJoinInProgress`  
- 프레즌스/로비: `bUsesPresence`, `bUseLobbiesIfAvailable`  
- 커스텀 속성(메타데이터): `ExtraSettings` (예: 비밀방 여부, 해시 등)  

<br>

### 2) 호스트 맵 이동 (Travel)
세션을 만든 뒤 맵을 바꾸려면 **`OpenLevel`을 사용하지 말고**,  
**`bSeamlessTravel = true`** 설정 후 **`ServerTravel`**을 호출하세요.  
> `OpenLevel`은 세션 컨텍스트를 끊어 검색/조인 흐름을 무너뜨릴 수 있습니다.  

<br>

### 3) 세션 검색
검색 시 `FOnlineSessionSearch`의 `QuerySettings`에 **프레즌스/로비 여부** 등 호스트와 **동일한 기준**을 넣어야 합니다.  
설정이 다르면 **보여도 조인이 실패**하거나 **아예 검색되지 않을 수** 있습니다.  

<br>

### 4) 세션 조인 & 클라이언트 이동
`JoinSession` 성공 후 제공되는 **ConnectString**을 사용해 **`ClientTravel`**로 서버 맵에 합류합니다.  
> 조인 성공이라도 ConnectString 처리나 Travel 파라미터가 잘못되면 **로딩 실패**가 발생합니다.

<br>

### **비밀번호(비밀방) 처리 — 해시로 안전하게**
세션을 검색하면 해당 세션의 `ExtraSettings`에 추가 프로퍼티를 붙일 수 있습니다. 
여기에는 비밀번호처럼 민감한 값은 평문 대신 OpenSSL로 생성한 `SHA-256` 해시를 저장해 노출을 막습니다. 
클라이언트는 조인 요청 전에 입력한 비밀번호의 해시를 세션에 저장된 해시와 비교해 접속 가능 여부를 확인하고, 
서버는 `PreLogin` 단계에서 같은 방식으로 한 번 더 검증해 조인을 허용합니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}



