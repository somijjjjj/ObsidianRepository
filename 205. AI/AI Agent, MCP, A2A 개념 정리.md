

AI 기술은 단순한 '대화형'을 넘어 실제 업무를 수행하는 실행형 AI로 진화하고 있습니다. 이 흐름의 중심에는 AI Agent, 그리고 이를 확장한 MCP(Multi-agent Control Plane), A2A(Agent-to-Agent) 구조가 있습니다.

기존의 LLM은 질문에 답하는 데는 뛰어나지만, 실제 행동을 수행하는 데에는 한계가 있었습니다. 예를 들어, “내일 오후 2시에 치과 예약해줘”라는 요청에 대해 단순히 정보를 알려줄 수는 있지만, 직접 예약을 처리하지는 못합니다.

> AI Agent는 "계획 수립 + 도구 사용 + 기억 + 실행" 능력을 갖춘 실행형 AI입니다.

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

### ▫️ 예시 시나리오

"내일 오후 2시~6시 사이 치과 예약해줘"

AI Agent의 흐름

1. 사용자의 캘린더 확인
2. 치과 예약 가능한 시간 탐색
3. 예약 시스템 API 호출
4. 예약 완료 및 사용자에게 안내

### ▫️AI Agent 기술 스택

- **LangChain**, **LlamaIndex**, **Semantic Kernel** 등
- **CrewAI**, **AutoGen**, **LangGraph** 등 멀티에이전트 프레임워크
- 프롬프트 템플릿, 툴 래퍼, 메모리 모듈, 체인 구성 등 제공

---

## ✅ MCP (Model Context Protocol)

2024년 11월 Anthropic 에서 도입한 개방형 표준 , 오픈 소스 프레임워크로 ,

대규모 언어 모델 (LLM)과 같은 인공 지능 (AI) 시스템이 외부 도구, 시스템 및 데이터 소스와 데이터를 통합하고 공유하는 방식을 표준화합니다.

"AI용 USB‑C 포트"라는 비유처럼, 다양한 시스템과 연결할 수 있는 일종의 공통 인터페이스 역할을 한다.

기존 Agent & 도구 연동의 LangChain 등 프레임워크에서는 개별 메서드 수준 통합 방식이라면, MCP는 “프로토콜 기반 융합”을 목표로 하며, 모든 에이전트·도구와 표준 연결을 지향

### MCP 작동방식


![[Pasted image 20250709145019.png|500]]

구조: Host–Client–Server 모델

|역할|설명|
|---|---|
|**Host**|LLM 또는 AI 앱(예: Claude Desktop, Copilot Studio 등). 버튼 클릭 한 번으로 MCP Client 호출.|
|**Client**|호스트 내에서 MCP 서버와 통신을 담당하는 연결자.|
|**Server**|외부 데이터 소스나 도구(API, DB, 파일 등)을 MCP 메시지로 전달.|
|([fr.wikipedia.org](https://fr.wikipedia.org/wiki/Model_Context_Protocol?utm_source=chatgpt.com), [zh.wikipedia.org](https://zh.wikipedia.org/wiki/%E6%A8%A1%E5%9E%8B%E4%B8%8A%E4%B8%8B%E6%96%87%E5%8D%8F%E8%AE%AE?utm_source=chatgpt.com), [evergreen.insightglobal.com](https://evergreen.insightglobal.com/the-new-model-context-protocol-for-ai-agents/?utm_source=chatgpt.com))||

### ▫️ **주요 기능**

- **보안·권한 제어**: 서버는 액세스 범위만 제한적으로 노출, 민감 정보 보호 .
- **기능 탐색**: 연결되면 사용 가능한 도구나 액션 목록을 LLM이 확인 가능 .
- **상태 유지**: 연결(session) 기반으로 **상태 유지 및 메모리 공유** 가능 .
- **양방향 통신**: LLM ↔ 도구 간 데이터 흐름이 모두 가능 .

### ▫️ **생태계와 채택 현황**

- **2024년 11월** Anthropic 주도로 오픈소스로 공개 .
- **OpenAI, Google DeepMind** 등 주요 AI 업체들도 공식 도입 (2025년 초부터 적용 시작)
- **SDK 및 서버 구현**: Python, TypeScript, C#, Java, Kotlin, Go 등 다양한 언어에서 SDK 및 레퍼런스 서버 제공
- 오픈소스 프로젝트도 활발(예: lastmile-ai/mcp-agent)

---

## ✅ 5. A2A (Agent to Agent)

**A2A는 에이전트 간의 직접적인 메시지 교환과 협업 구조**를 의미합니다.

MCP가 중앙에서 제어하는 구조였다면,

A2A는 **Agent끼리 서로 메시지를 주고받으며 수평적으로 협업**하는 방식입니다.

이 구조에서는 하나의 Agent가 다른 Agent에게 결과를 넘겨주거나,

필요한 정보를 요청할 수 있고, 병렬적으로 작업을 수행할 수도 있습니다.

A2A는 단일 Agent보다 더 유연하고 확장성 높은 시스템을 만들 수 있으며, 특히 대규모 태스크나 병렬 처리가 필요한 시나리오에 적합합니다.

### ▫️ 통신 방식

- 자연어 또는 JSON 기반의 구조화된 메시지 교환
- REST API 호출, 메시지 큐(Kafka, Redis 등), 내부 RPC 등 다양한 방식
- 메시지를 통한 상태 전달, 중간 결과 공유, 역할 위임 등

### ▫️ 특징

- 병렬 작업 최적화 (동시에 다수 작업 가능)
- Agent 간 협상 및 작업 분배 (ex. 계획 → 조사 → 작성)
- 모듈화/확장성 뛰어남 (새 Agent 쉽게 추가/교체 가능)
- LangGraph, CrewAI, AutoGen 등으로 구현 가능

---

## 정리

|항목|AI Agent|MCP (Control Plane)|A2A (Agent-to-Agent)|
|---|---|---|---|
|역할|단일 작업 실행|여러 Agent 조율/워크플로우 관리|Agent 간 수평 협업|
|구성 방식|1개의 LLM + Tool + Memory|여러 Agent + Controller|Agent 다수 간 직접 연결|
|장점|실행 능력|전체 흐름 설계와 실패 복구|병렬 처리, 유연한 확장 구조|
|활용 예시|일정 예약, 이메일 답장|문서 작성 자동화, 업무 보고서 생성|마케팅 전략 병렬 설계, QA 협업|

📺 [유튜브 : AI 경쟁구도를 바꾸는 MCP 알아보기](https://www.youtube.com/watch?v=OdwuHsXPqn4)

---