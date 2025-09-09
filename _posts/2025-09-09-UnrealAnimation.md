---
title: "애니메이션 시스템 제작"
date: 2025-09-09 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "소개"
AutoContents: false
---

{% capture paragraph %}
## **FSM**
애니메이션 상태를 정의하기위해 **언리얼 상태머신**을 사용했습니다.

![사진]({{ '/assets/MyLittleStorage/FSM.png' | relative_url }}){: style="max-width: 100%; height: auto;" } 

<br>

### **모듈화**
컴포넌트(무브먼트, 총, 엄폐, 피격)별로 ABP를 나누어 플레이어 캐릭터 ABP에서 이를 조합하는 형태로 제작했습니다.

![사진]({{ '/assets/MyLittleStorage/FSMComponent.png' | relative_url }}){: style="max-width: 100%; height: auto;" } 

![사진]({{ '/assets/MyLittleStorage/FSMComponent2.png' | relative_url }}){: style="max-width: 100%; height: auto;" } 

### **HandIK**

**2bone IK**를 사용해 플레이어 손이 무기를 잡도록 제작했습니다.
장전같은 액션에는 IK 블렌드 값을 조절해 자연스럽게 손이 떼지게 했습니다.

![사진]({{ '/assets/MyLittleStorage/HandIK.png' | relative_url }}){: style="max-width: 49%; height: auto;" } 
![사진]({{ '/assets/MyLittleStorage/HandIK2.png' | relative_url }}){: style="max-width: 49%; height: auto;" } 


### **FootIK**
게임틱에서 허리에서 발끝까지 레이체크를해 발이 땅에 닿도록 제작했습니다.  
레이의 거리가 너무 짧게 나오면 오히려 이상해져서 무시하게 하였습니다.  
**2bone IK**를 사용하였으나 퀄리티를위해 DragoneIK 플러그인을 사용했습니다.


![사진]({{ '/assets/MyLittleStorage/FootIK.png' | relative_url }}){: style="max-width: 100%; height: auto;" } 

### **되돌아보기**
몽타주를 사용해도 됫을부분을 상태머신으로 제작한점이 아쉬웠습니다.

재활용을 너무 염두하여 제작하다보니 오히려 복잡해진 부분이 있었습니다.
재활용을 안하더라도 캐릭터 종류마다 다르게 애니메이션을 제작하는것이 복잡도나 유지보수에 더 좋을것 같습니다.

{% endcapture %}
{% include paragraph.html content=paragraph %}




