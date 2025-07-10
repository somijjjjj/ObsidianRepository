

## 📝 폴더 관리 규칙

- 새로운 노트는 일단 `000. Capture`에 생성 → 정리 후 정식 폴더로 이동
- 각 폴더 내에 `README.md`를 두고 그 안에서 세부 분류 작성 가능
- 사용하지 않는 폴더는 2주마다 리뷰하여 삭제 or 통합

## 📁 000~099: 임시 / 개인 메모

- `000. Capture`: 새로 생성한 노트가 자동 저장되는 임시 저장소
  - 주기적으로 정리하여 아래 정식 폴더로 이동
- `001. Note 2025`: 연간 계획이나 목표 노트

## 📁 200~299: CS 기초 및 기술 분류

- `200. CS`: 컴퓨터공학 전반 (자료구조, OS, 네트워크 등)
- `201. 서버`: 서버 구축, 관리, 설정
- `203. 클라우드`: AWS, Oracle Cloud, 기타 클라우드 관련
- `204. API`: REST, GraphQL 등 API 설계 및 사용
- `205. AI`: LLM, AI Agent, Prompt 설계 등
- `206~208`: 데이터베이스 (MongoDB, MySQL 등)
- `209. 보안`: 보안 사고, 제로트러스트, EDR, 암호화 등
- `210. Crawling:` 크롤링 대상 사이트 분석, 크롤링 사용 도구 정리 등

## 📁 300~399: 언어 & 프레임워크

- `301. HTML&CSS`, `302. JavaScript`, `303. Spring`, `305. Java` 등
- `306. FrontEnd`: React, Next.js, Tailwind 등

## 📁 400~499: 개발 도구

- `401. IDE`: VSCode, IntelliJ, 단축키, 플러그인 설정 등

## 📁 900~999: 시스템용 / 임시 저장소

- `999. Attached file`: 외부 첨부파일 모음
- `Attached file`: (중복 확인 후 정리 필요)


---


## ⚡ TAG 관리 규칙



- 태그는 `#영역/하위항목` 형태로 사용
- 검색 및 필터링이 용이하도록 되도록 일관된 네이밍 사용
- 예시:
  - `#topic/ai`
  - `#status/ongoing`
  - `#publish/draft`


### 🗒️ publish 태그 규칙

> 내부 노트의 외부 발행 여부를 관리할 때 사용

- `#publish/todo` : 초기 상태
- `#publish/draft` : 초안 작성 중
- `#publish/ready` : 발행 직전 최종 점검
- `#publish/done` : 발행 완료
- `#publish/update_needed` : 발행했으나 수정 필요
- `#publish/hold` : 발행 보류 상태 (추후 검토 예정)

> 해당 태그는 노트 상단 또는 메타데이터(YAML frontmatter)에서 관리