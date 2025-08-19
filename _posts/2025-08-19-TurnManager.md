---
title: "턴제관리 시스템 제작"
date: 2025-08-19 00:00:00
layout: post
subtitle: 
 - "부제목 1"
 - "부제목 2"
description: "소개"
AutoContents: false
mermaid: true
---

{% capture paragraph %}
## **커맨드 패턴**
몬스터들이 어떻게 행동할지 결정후 플레이어가 행동을 선택하고,
모든 행동이 처리되는 턴제 시스템을 제작하였습니다.


{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **상태 트리**
턴제 시스템상 상태가 흐른다는 개념이 상태머신보다는 **상태 트리**로 표현하는 것이 더 어울렸기에 상태트리로 턴을 관리 하였습니다.

초기에는 다음과 같았습니다.
```
//트리지만 Yaml구조로 표현하였습니다.
RootNode:
  ST::SequenceNode:
    - GameFlow_MonsterSpawnActionNode
    - GameFlow_DecideEnemyActionNode
    - GameFlow_PlayerActionNode
    - GameFlow_ProcessCommandNode

```

<div style="display: flex;" align="center">
  <div style="flex: 1;">
    
``` mermaid
flowchart 
A1[GameFlow_MonsterSpawnActionNode] --> A2[GameFlow_DecideEnemyActionNode]
A2 --> A3[GameFlow_PlayerActionNode]
A3 --> A4[GameFlow_ProcessCommandNode]
```

  </div>
  <div style="flex: 1;">
    
``` mermaid
flowchart 
A1[몬스터 소환] --> A2[적행동결정]
A2 --> A3[플레이어 행동 선택]
A3 --> A4[명령 처리]
```
  </div>
</div>

![몬스터 행동 움짤]()
<br>
<br>
<br>

클리어조건이 추가가 되면서 해당트리에
클리어 체크, 맵전환하는 서브트리를 추가하게 되었습니다. 
서브트리로 몬스터 스폰 전에 배치함으로써 모든 몬스터가 사망시 클리어, 스테이지 전환후 몬스터 스폰할수 있게 되었습니다.

``` 
//트리지만 Yaml구조로 표현하였습니다.
StageCheckSubTree: &StageCheckSubTree
  ST::ForceSuccess:
    - ST::SequenceNode:
        - GameFlow_IsStageClearNode
        - GameFlow_GameClearNode
        - GameFlow_StageStartNode

RootNode:
  ST::SequenceNode:
    - *StageCheckSubTree
    - GameFlow_MonsterSpawnActionNode
    - GameFlow_DecideEnemyActionNode
    - GameFlow_PlayerActionNode
    - GameFlow_ProcessCommandNode
```
![전환 움짤]()


<br><br><br>

실제 코드는 다음과 같이 사용하였고
정적 리플렉션을 사용하여 텍스트기반으로 C++데이터를 생성히였습니다.
``` cpp

void GameFlowBehaviorTree::Init()
{
	Archive archive;
	Archive archive2;

	{
		YAML::Emitter emitter;
		emitter << YAML::BeginSeq;

		emitter << LoadArchive("GameFlow_IsStageClearNode:");
		emitter << LoadArchive("GameFlow_GameClearNode:"); 
		emitter << LoadArchive("GameFlow_StageStartNode:");

		emitter << YAML::EndSeq;

		archive2["BT::ForceSuccess"]["nextNode"]["BT::SequenceNode"]["childNodes"] = LoadArchive(emitter.c_str());
	}
	{
		YAML::Emitter emitter;
		emitter << YAML::BeginSeq;

		emitter << archive2; 
		emitter << LoadArchive("GameFlow_MonsterSpawnActionNode:");
		emitter << LoadArchive("GameFlow_DecideEnemyActionNode:");
		emitter << LoadArchive("GameFlow_PlayerActionNode:");
		emitter << LoadArchive("GameFlow_ProcessCommandNode:");

		emitter << YAML::EndSeq;

		archive["rootNode"]["BT::SequenceNode"]["childNodes"] = LoadArchive(emitter.c_str());
	}
	
	std::stringstream tempsstream;
	tempsstream << archive;
	DeSerialize(LoadArchive(tempsstream));
}
```


{% endcapture %}
{% include paragraph.html content=paragraph %}

{% capture paragraph %}
## **csv -> C Struct**
스테이지정보 로드


{% endcapture %}
{% include paragraph.html content=paragraph %}

