

AI 기술은 단순한 '대화형'을 넘어 실제 업무를 수행하는 실행형 AI로 진화하고 있습니다.
이 흐름의 중심에는 AI Agent, 그리고 이를 확장한 MCP(Multi-agent Control Plane), A2A(Agent-to-Agent) 구조가 있습니다.


기존의 LLM은 질문에 답하는 데는 뛰어나지만, 실제 행동을 수행하는 데에는 한계가 있었습니다.
예를 들어, “내일 오후 2시에 치과 예약해줘”라는 요청에 대해 단순히 정보를 알려줄 수는 있지만, 직접 예약을 처리하지는 못합니다.

> **AI Agent**는 "계획 수립 + 도구 사용 + 기억 + 실행" 능력을 갖춘 실행형 AI입니다.

- 복잡한 요청을 여러 단계로 분해    
- 외부 도구/API를 활용해 실제 업무 수행  
- 사용자 맥락 및 상태 기억


---

## ✅ AI Agent란?


```
[사용자 요청]
     ↓
[AI Agent]
 ├─ 계획 수립 (Planning)
 ├─ 도구 호출 (Tool Call)
 ├─ 상태 기억 (Memory)
 └─ 응답 생성 (LLM 기반)
```

AI Agent는 **LLM을 기반으로 하되**, 다음 기능들을 추가해 실제 업무를 수행합니다.

- **Planning**: 사용자의 요청 의도 분석 및 작업 계획 수립    
- **Tool 사용 (Function Calling)**: 외부 API, DB, 계산기, 캘린더 등 사용    
- **Memory**: 대화 히스토리 및 상태 저장 (단기/장기)    
- **Reasoning & Execution**: 멀티스텝 논리 추론 및 실행
    

### 예시 시나리오

"내일 오후 2시~6시 사이 치과 예약해줘"

AI Agent의 흐름
1. 사용자의 캘린더 확인    
2. 치과 예약 가능한 시간 탐색    
3. 예약 시스템 API 호출 
4. 예약 완료 및 사용자에게 안내
    

---

### AI Agent 기술 스택

- **LangChain**, **LlamaIndex**, **Semantic Kernel** 등
- **CrewAI**, **AutoGen**, **LangGraph** 등 멀티에이전트 프레임워크
- 프롬프트 템플릿, 툴 래퍼, 메모리 모듈, 체인 구성 등 제공
    

---

## ✅ MCP (Multi-agent Control Plane)

하나의 Agent로 처리하기 어려운 복합 작업은 여러 Agent의 협력이 필요합니다.
MCP는 이들 Agent를 통합적으로 조율하고, 전체 워크플로우를 관리합니다.

> **MCP는 멀티 Agent의 프로젝트 매니저 역할**을 합니다.


### **주요 기능**
- Agent 간 역할 분배 및 순서 조정    
- 실패 감지 및 Retry, Fallback 처리    
- Shared Memory 기반의 상태 관리

### **구성 예시**
```
[MCP]
  ├── Planner Agent : 전체 워크 플로우 설계
  ├── Research Agent : 필요한 정보 수집
  ├── Summarizer Agent : 수집 결과를 정리
  └── Evaluator Agent : 결과 검토
```



---

## ✅ 5. A2A (Agent to Agent)

**A2A는 에이전트 간의 직접적인 메시지 교환과 협업 구조**를 의미합니다.  

MCP가 중앙에서 제어하는 구조였다면,  
A2A는 **Agent끼리 서로 메시지를 주고받으며 수평적으로 협업**하는 방식입니다.

이 구조에서는 하나의 Agent가 다른 Agent에게 결과를 넘겨주거나,  
필요한 정보를 요청할 수 있고,  병렬적으로 작업을 수행할 수도 있습니다.

A2A는 단일 Agent보다 더 유연하고 확장성 높은 시스템을 만들 수 있으며, 특히 대규모 태스크나 병렬 처리가 필요한 시나리오에 적합합니다.

### 통신 방식
- 자연어 또는 JSON 기반의 구조화된 메시지 교환    
- REST API 호출, 메시지 큐(Kafka, Redis 등), 내부 RPC 등 다양한 방식    
- 메시지를 통한 상태 전달, 중간 결과 공유, 역할 위임 등
    

### 특징
- 병렬 작업 최적화 (동시에 다수 작업 가능)
- Agent 간 협상 및 작업 분배 (ex. 계획 → 조사 → 작성)
- 모듈화/확장성 뛰어남 (새 Agent 쉽게 추가/교체 가능)
- LangGraph, CrewAI, AutoGen 등으로 구현 가능


---

## 정리

| 항목    | AI Agent                | MCP (Control Plane)   | A2A (Agent-to-Agent) |
| ----- | ----------------------- | --------------------- | -------------------- |
| 역할    | 단일 작업 실행                | 여러 Agent 조율/워크플로우 관리  | Agent 간 수평 협업        |
| 구성 방식 | 1개의 LLM + Tool + Memory | 여러 Agent + Controller | Agent 다수 간 직접 연결     |
| 장점    | 실행 능력                   | 전체 흐름 설계와 실패 복구       | 병렬 처리, 유연한 확장 구조     |
| 활용 예시 | 일정 예약, 이메일 답장           | 문서 작성 자동화, 업무 보고서 생성  | 마케팅 전략 병렬 설계, QA 협업  |



📺 **[유튜브 : LangChain Agents 구조 설명](https://www.youtube.com/watch?v=OdwuHsXPqn4)**



---
