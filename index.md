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
# **게임 개발 이야기**
- **[게임인재원 텐센트 강의](2025/08/07/TencentConference.html)**

{% endcapture %}
{% include paragraph.html content=paragraph %}



{% capture paragraph %}
# **F Rank Hunter**

<p align="center">
 <img src = "images/CoverImage.png" style="width: 100%;">
</p>
<p style="text-align: right;">
  멀티플레이 헌터물 3D생존 게임입니다.
</p>

## 학습
- **[게임플레이 어빌리티 시스템](2025/06/28/Ability_System.html)**
- **[현지화 시스템](2025/06/28/Localize.html)**
- **[언리얼 UI](2025/06/28/UI.html)**
- **[매칭 시스템](2025/07/31/UnrealMatching.html)**
- **게임 관전**
- **더 나은 개발환경을 위한 치트매니저**
<br><br><br>

# **기능 제작**
- **[아이템, 인벤토리 시스템 제작](2025/06/27/InventorySystem_Develop.html)**
- **[AI 시스템 제작](2025/07/15/AI.html)**
- **[게임 진행도 저장 시스템 제작](2025/06/28/SaveGame.html)**
- **[카오스디스트럭션 & 피직스필드를 이용한 파괴 시스템 제작](2025/07/08/StoneDestroy.html)**
<br><br><br>

# **컨텐츠 작업**
- **곡괭이 아이템 제작**
- **다이너마이트 아이템 제작**
- **HP포션 아이템 제작**
- **소음발생기 아이템 제작**
- **소음장치 아이템 제작**
- **일반 크리처 제작**
- **돌진 크리처 제작**
- **타이틀UI 제작**
- **상태창UI 제작**
- **플레이어 HUD 제작**
- **아이템 툴팁UI 제작**
- **아이템 카트 제작**
- **아이템 가방 제작**
<br><br><br>

# **문제 해결**
- **후처리 머티리얼 해상도 불일치**
- **[언리얼 데이터 테이블 경로데이터 CSV이용시 애로사항](2025/07/04/DataTableApplication.html)**
- **[플레이어정보 저장시 가방 액터 미저장](2025/08/09/SaveMissing.html)**
- **어빌리티시스템 클라이언트 입력 태그부여**
- **[물리 오브젝트의 의도치 않은 플랫폼 효과 개선](2025/08/09/UnintendedPhysicsPlatformEffects.html)**
- **[오너십 없는 객체의 서버 상호작용 경로 설계](2025/08/09/ObjectsWithoutOwnership.html)**
- **자식액터 컴포넌트 리플리케이션 문제**
- **클라이언트의 접속 종료시 폰삭제 문제**
- **[버전관리툴 바이너리 컨플릭트](2025/07/17/UnrealSVN.html)**
<br><br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
# **Potion Atelier**
<p align="center">
 <img src = "images/PorionAtelier.png" style="width: 100%;">
</p>
<p style="text-align: right;">
  탑다운뷰 3D타이쿤 게임입니다.
</p>

## 기능 제작
- **[머티리얼 노드 에디터 제작](2025/07/26/MaterialEditor.html)**
- **파티클 시스템 제작**
- **PBR & IBL 구현**
- **블룸 제작**
- **외곽선 추출 알고리즘 제작 (Laplacian Filter)**
- **쉐도우 맵핑 제작**
- **FBX 임포트 파이프라인 제작**
- **스키닝 제작**
- **자원관리 기법 구현**
- **텍스처 압축 툴 제작**
- **Fmod Sound Bank 사용**
<br><br>


## 문제 해결
- **[수많은 머티리얼 쉐이더 컴파일](2025/08/10/MaterialEditorTrouble.html)**
- **ConsumeStructuredBuffer 데드락**
<br><br>


## 컨텐츠 작업
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}

# **Rail Way To Hell**
<p align="center">
 <img src = "images/2.png" style="width: 100%;">
</p>

## 기능 제작
-  **턴제관리 시스템 제작**
<br><br>

## 문제 해결
<br><br>

## 컨텐츠 작업
<br><br>

{% endcapture %}
{% include paragraph.html content=paragraph %}
