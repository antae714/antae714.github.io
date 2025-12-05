{% capture paragraph %}

![사진]({{ '/assets/MyLittleStorage/ProfileImage.jpg' | relative_url }}){: style="width: 200px; height: auto;" loading="lazy" } 

### **즐거운 게임을 즐겁게, 재밌는 게임을 재밌게 만들고 싶은 프로그래머입니다.**

<br>

## 연혁
- **2000.07.14: 출생** 
- **2020.01.26 ~ 2021.08.01: 육군 병장 만기제대**
- **2021.08.01 ~ 2024.03.01: 학점은행제 게임프로그래밍과 수료**
- **2024.03.01 ~ 현재: 게임인재원**

<br>

## 기술
**C, C++, C#, WINAPI, Direct11, Unreal Engine, SVN, FMod**

{% endcapture %}
{% include paragraph2.html content=paragraph %}


{% capture paragraph %}
{{ include.Link }}

<div style="display: flex;" align="center">
  <div style="flex: 1;">

# 팀프로젝트
# **F Rank Hunter**
언리얼엔진을 사용한 멀티플레이 헌터물 3D생존 게임입니다.  

  </div>
  <div style="flex: 1;">

<p align="center">
 <img src = "images/CoverImage.png" style="width: 100%;" loading="lazy">
</p>

  </div>
</div>

---

# 게임플레이

> ## **인벤토리 시스템, 아이템 컨텐츠 제작** [자세히 보기][Link_InventorySystem_Develop]{: .markdown-Link }  <br>  
>
> **멀티플레이** 생존게임에서 필요한 **인벤토리 플러그인**, **GAS기반 아이템 컨텐츠**를 제작하였습니다.  
> 아이템은 **드랍**하고 **상호작용**하여 획득할 수 있습니다.  
> 아이템을 담을 수 있고 플레이어가 상호작용하여 착용하는 **가방**도 제작하였습니다.  
> 아이템과 관련된 **UI**또한 제작하였습니다.  
> 인벤토리, 아이템을 제작하며 필요한 **테스트 환경**도 같이 제작하였습니다.  
>  <br>
>![사진]({{ '/assets/ItemGIF/SprayItem.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 


>## **세션 기반 매칭 시스템 제작** [자세히 보기][Link_UnrealMatching]{: .markdown-Link }  <br>  
>
> 멀티게임의 매칭을 위한 **온라인 서브시스템**의 **세션 인터페이스**를 이용하여 세션기반으로 방을 **생성, 검색, 조인**하는 매칭 시스템을 제작하였습니다.  
> `ExtraSettings`를 이용하여 유저친화적인 비밀번호(비밀방) 기능도 제작하였습니다.  
>  <br>
>![사진]({{ '/assets/ItemGIF/Title1.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/Title2.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 


>## **게임 진행도 저장 시스템 제작** [자세히 보기][Link_SaveGame]{: .markdown-Link }  <br>  
> 
> 멀티게임에서 호스트의 생성된 게임 진행도를 저장하고 불러오는 시스템을 제작하였습니다.  
> 게임정보(종료된 레벨, 돈, 플레이상태)와 드랍된 아이템등의 액터정보를 저장하고 불러올수있습니다.
> 
>  <br>
>![사진]({{ '/assets/MyLittleStorage/SaveSystem.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 

<br><br><br>  

---

# AI 

>## **AI 시스템, 크리처 컨텐츠 제작** [자세히 보기][Link_AI]{: .markdown-Link }  <br>  
>
> 멀티생존 게임에서 필요한 **AI 시스템**과 **크리처 컨텐츠**를 제작하였습니다.  
> 플레이어와의 상호작용을 위해 **GAS 어트리뷰트 세트**를 사용하여 제작하였습니다.  
> AI의 행동은 다양한 의사결정을 위해 **행동트리**와 **블랙보드**를 사용하여 제작하였습니다.  
> 크리처는 **문을 열고**, 플레이어를 **추적하고**, **공격**하는 행동을 구현하였습니다.  
> 크리처의 **플레이어 감지**를 위해 언리얼의 **퍼셉션 컴포넌트**를 사용했습니다.  
>
>  <br>
>![사진]({{ '/assets/MyLittleStorage/CreatureOpenDoor.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 



{% comment %}

인벤토리
>![사진]({{ '/assets/ItemGIF/CoolTime.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/CoolTime2.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/ManaStone.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/ChaosDestruction.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/ManaStoneUse1.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/ManaStoneUse2.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/ManaStoneBoom.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/Physics.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/BackpackClient.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/BackpackServer.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/SprayItem.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/SprayItem2.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 

AI
>![사진]({{ '/assets/MyLittleStorage/CreatureOpenDoor.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/MyLittleStorage/CreatureHorse.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/MyLittleStorage/CreatureRat.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/MyLittleStorage/CreatureGhost.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/MyLittleStorage/CreatureSlime.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/MyLittleStorage/CreatureCryAngle.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 

게임 진행도 저장 
>![사진]({{ '/assets/MyLittleStorage/SaveSystem.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 

세션 기반 매칭
>![사진]({{ '/assets/ItemGIF/Title1.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/ItemGIF/Title2.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 


{% endcomment %}



{% endcapture %}
{% include paragraph2.html content=paragraph %}

{% capture paragraph %}

<div style="display: flex;" align="center">
  <div style="flex: 1;">

# 팀프로젝트
# **Potion Atelier**
자체엔진 탑다운뷰 3D 타이쿤 게임입니다.  

[![사진]({{ '/assets/GitHub.png' | relative_url }}){: style="width: 50px; height: 50px;" loading="lazy" }](https://github.com/antae714/PotionAtelier){:target="_blank"}
[![사진]({{ '/assets/Youtube.png' | relative_url }}){: style="width: 50px; height: 50px;" loading="lazy" }](https://youtu.be/Symfl9evOls?si=fJgnMrNLEOgD-BaK){:target="_blank"}


  </div>
  <div style="flex: 1;">

<p align="center">
 <img src = "images/PorionAtelier.png" style="width: 100%;" loading="lazy">
</p>

  </div>
</div>


{{ include.Link }}

>## **에디터/툴 제작** [자세히 보기][Link_MaterialEditor]{: .markdown-Link }  <br>  
>![사진]({{ '/assets/MyLittleStorage/NodeEditor.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 
>
>![사진]({{ '/assets/MyLittleStorage/NodeEditor.png' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 

<br>


>## **그래픽스 렌더링 엔진 제작**  [자세히 보기][Link_GrapicsRendering]{: .markdown-Link }  <br>  
>![사진]({{ '/assets/MyLittleStorage/Grapic.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 
>
>![사진]({{ '/assets/MyLittleStorage/Grapic2.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 
>
>![사진]({{ '/assets/MyLittleStorage/Grapic.png' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 

{% endcapture %}
{% include paragraph2.html content=paragraph %}

{% capture paragraph %}

<div style="display: flex;" align="center">
  <div style="flex: 1;">

# 팀프로젝트
# **Rail Way To Hell**
D2D 퍼즐 게임입니다.  

[![사진]({{ '/assets/GitHub.png' | relative_url }}){: style="width: 50px; height: 50px;" loading="lazy" }](https://github.com/antae714/RailwayToHell){:target="_blank"}
[![사진]({{ '/assets/Youtube.png' | relative_url }}){: style="width: 50px; height: 50px;" loading="lazy" }](https://youtu.be/TYsThMBrRks){:target="_blank"}

  </div>
  <div style="flex: 1;">

<p align="center">
 <img src = "images/2.png" style="width: 100%;" loading="lazy">
</p>

  </div>
</div>


{{ include.Link }}

>## **턴제관리 시스템 제작** [자세히 보기][Link_TurnManager]{: .markdown-Link }  <br>  
>![사진]({{ '/assets/MyLittleStorage/RailWaytoHell.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 
>
>![사진]({{ '/assets/MyLittleStorage/RailWaytoHell2.gif' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 
<br>

{% endcapture %}
{% include paragraph2.html content=paragraph %}

{% comment %}

{% capture paragraph %}

<div style="display: flex;" align="center">
  <div style="flex: 1;">

# 팀프로젝트
# **Project Reload**
언리얼 TPS 게임입니다.

  </div>
  <div style="flex: 1;">

<p align="center">
 <img src = "images/image6.png" style="width: 100%;" loading="lazy">
</p>

  </div>
</div>


{{ include.Link }}


>## **애니메이션**  [자세히 보기][Link_Animation]{: .markdown-Link }  <br>  
>![사진]({{ '/assets/MyLittleStorage/FSM.png' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" } 
>
>![사진]({{ '/assets/MyLittleStorage/HandIK.png' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
>![사진]({{ '/assets/MyLittleStorage/FootIK.png' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" } 
<br>


>## **엄폐 시스템**  [자세히 보기][Link_CoverSystem]{: .markdown-Link }  <br>  
>![사진]({{ '/assets/MyLittleStorage/Cover.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" }
>![사진]({{ '/assets/MyLittleStorage/Cover2.gif' | relative_url }}){: style="width: 49%; height: auto;" loading="lazy" }
>![사진]({{ '/assets/MyLittleStorage/CoverNavigation.png' | relative_url }}){: style="width: 100%; height: auto;" loading="lazy" }

{% endcapture %}
{% include paragraph2.html content=paragraph %}

{% endcomment %}