---
tistory: https://jn-silveryoung.tistory.com/52
tags:
  - publish/done
---


이제 AI는 단순한 '대화형'을 넘어, 직접 행동하는 실행형 AI 시대로 나아가고 있습니다.

그 중심에는 AI Agent, MCP(Model Context Protocol), A2A(Agent-to-Agent) 기술이 있습니다.

---

## ✅ 왜 실행형 AI가 필요한가?

기존의 LLM(Large Language Model)은 “대화”에는 강하지만, 실제 업무 실행에는 한계가 있었어요.

예를 들어:

> "내일 오후 2시에 치과 예약해줘."

이 요청에 대해 LLM은 관련 정보는 알려줄 수 있지만, 직접 예약을 **수행**하지는 못했죠.  
여기서 등장한 것이 **AI Agent**입니다.

---

## 🧠 AI Agent란?

AI Agent는 단순한 응답을 넘어, 사용자의 요청을 **계획하고**, **도구를 사용**해, **기억을 기반으로 실행**까지 하는 '실행형 인공지능'입니다.

### ✨ 핵심 구성 요소

```
[사용자 요청]
     ↓
[AI Agent]
 ├─ 계획 수립 (Planning)
 ├─ 도구 호출 (Tool Use)
 ├─ 상태 기억 (Memory)
 └─ 응답 생성 (LLM 기반)
```

### 🛠 주요 기능

-   **Planning**: 복잡한 요청을 다단계로 분석하고 처리 계획 수립
-   **Tool Use**: API, DB, 캘린더 등 외부 도구 호출
-   **Memory**: 대화 내역 및 작업 상태 기억
-   **Execution**: 실제 업무를 단계별로 실행

### 📌 예시 시나리오

> “내일 오후 2시~6시 사이 치과 예약해줘” 

1.  사용자의 일정 확인
2.  치과 예약 가능 시간 탐색
3.  예약 시스템 API 호출
4.  결과 요약 후 사용자에게 안내

---

## 🧩 MCP (Model Context Protocol)

AI를 위한 USB-C 포트”라는 별명이 붙은 MCP는 다양한 도구, 시스템, 데이터를 연결해주는 AI 통합 프로토콜입니다.

2024년 11월, **Anthropic**이 주도하여 발표한 개방형 표준으로, OpenAI·Google 등도 참여하고 있습니다.

### 📦 구조 및 작동 방식


![[MCP 구조.png|500]]

-   **Host**: LLM 또는 AI 앱 (예: Claude, Copilot 등)
-   **Client**: Host와 Server를 연결하는 중간 계층
-   **Server**: 실제 API, DB, 시스템 등 외부 리소스를 MCP 메시지로 처리

### 🔐 주요 특징

| 기능 | 설명 |
| --- | --- |
| 보안·권한 제어 | 외부 시스템에 필요한 범위만 노출하여 민감 정보 보호 |
| 기능 탐색 | Agent가 연결 시 사용 가능한 기능 목록 자동 파악 |
| 상태 유지 | 세션 기반 기억 및 공유 |
| 양방향 통신 | LLM ↔ 외부 도구 간 데이터 상호교환 가능 |

---

## 🤝 A2A (Agent-to-Agent)

MCP가 중앙 통제형이라면, A2A는 **에이전트 간 수평 협업 구조**입니다.

에이전트가 서로 메시지를 주고받으며 작업을 분담하고 병렬로 실행하는 것이 특징이에요.

### 📡 통신 방식

-   자연어 또는 JSON 형식의 메시지 교환
-   REST API, Kafka, RPC 등 다양한 방식 지원
-   협업, 역할 분담, 병렬 처리에 최적화

### 🧱 장점

-   병렬 처리 효율 ↑
-   각 Agent의 역할 모듈화 및 유연한 확장 가능
-   대규모 프로젝트, 다단계 업무, 협업 중심 환경에 적합

---

## 📊 비교 요약

| 항목 | AI Agent | MCP (Control Plane) | A2A (Agent-to-Agent) |
| --- | --- | --- | --- |
| 역할 | 단일 작업 실행 | 여러 Agent 조율 및 연결 | 에이전트 간 협업 |
| 구조 | LLM + Tool + Memory | Host–Client–Server | 수평형 에이전트 네트워크 |
| 장점 | 실제 업무 실행 가능 | 시스템 통합 및 보안 | 확장성, 병렬성 뛰어남 |
| 활용 | 일정 예약, 이메일 작성 | 보고서 생성, 데이터 처리 | 조사 → 분석 → 보고 협업 |

---

## 🧠 함께 보면 좋은 기술 스택

-   **LangChain, LlamaIndex, Semantic Kernel**: 에이전트 설계용 핵심 프레임워크
-   **AutoGen, CrewAI, LangGraph**: 멀티에이전트 시스템 구현 도구
-   **lastmile-ai/mcp-agent**: MCP 오픈소스 참고 프로젝트

---

실행형 AI는 이제 미래가 아닌 **현재**입니다.  
AI Agent → MCP → A2A로 이어지는 기술의 흐름은 “단순한 챗봇”을 넘어 “일을 하는 AI”로 나아가는 핵심 축이 됩니다.

개발자, 기획자, 서비스 운영자라면 지금 이 구조를 이해하고 **실제 업무에 어떻게 적용할지 고민할 시점**입니다.

#### 📺 **참고 영상**

[AI 경쟁 구도를 바꾸는 MCP란? (유튜브)](https://www.youtube.com/watch?v=OdwuHsXPqn4)

#### 📚 **관련 문서**

-   [Anthropic 공식 블로그 - MCP](https://www.anthropic.com/index/model-context-protocol)
-   [LangChain Docs](https://docs.langchain.com/)
-   [AutoGen GitHub](https://github.com/microsoft/autogen)


