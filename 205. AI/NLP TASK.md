![](https://miro.medium.com/v2/resize:fit:700/1*pR9nbCcPHwCZnSX5VHrYZA.png)

[NLP 작업 분류. 저자가 직접 만든 이미지.](https://medium.com/nlplanet/two-minutes-nlp-33-important-nlp-tasks-explained-31e2caad2b1b)



> 안녕하세요, NLP 애호가 여러분! 오늘은 33가지 일반적인 NLP 작업에 대한 간략한 설명과 함께 NLP의 전반적인 모습을 살펴보겠습니다. 최대한 간단하고 단순하지 않게 설명하려고 노력할 테니, 이 글을 시작점으로 삼아 NLP 분야를 깊이 파고들어 보세요. 자, 시작해 볼까요! _😄_

**분류**

- 텍스트 분류: 문장이나 문서에 범주를 할당합니다(예: 스팸 필터링).
- 감정 분석: 텍스트의 양극성을 식별합니다.

**정보 검색 및 문서 순위**

- 문장/문서 유사성: 두 텍스트가 얼마나 유사한지 판단합니다.
- 질의응답: 자연어로 질문에 답하는 작업입니다.

**텍스트-텍스트 생성**

- 기계 번역: 한 언어에서 다른 언어로 번역하는 것.
- 텍스트 생성: 사람이 쓴 텍스트와 구별할 수 없는 텍스트를 만드는 것.
- 텍스트 요약: 대부분의 의미를 보존하면서 여러 문서의 축약 버전을 만드는 것입니다.
- 텍스트 단순화: 주요 아이디어와 대략적인 의미를 보존하면서 텍스트를 읽고 이해하기 쉽게 만드는 작업입니다.
- 어휘 정규화: 비표준 텍스트를 표준 레지스터로 번역/변환하는 작업입니다.
- 의역 생성: 입력 내용의 의미는 보존하되 단어 선택과 문법에 변형을 가한 출력 문장을 만드는 과정입니다.

**지식 기반, 엔터티 및 관계**

- 관계 추출: 텍스트에서 의미 관계를 추출합니다. 추출된 관계는 일반적으로 두 개 이상의 개체 간에 발생하며 특정 의미 범주(예: ~에 거주함, ~의 ​​자매 등)에 속합니다.
- 관계 예측: 두 개의 명명된 의미적 개체 간의 명명된 관계를 식별합니다.
- 명명된 엔터티 인식: 일반적으로 BIO 표기법을 사용하여 텍스트의 엔터티에 해당 유형을 태그 지정합니다.
- 엔터티 연결: 지식 기반(일반적으로 위키데이터)에 명명된 엔터티를 인식하고 모호성을 해소합니다.

**주제 및 키워드**

- 주제 모델링: 문서 컬렉션의 기초가 되는 추상적인 "주제"를 식별합니다.
- 키워드 추출: 문서의 주제를 설명하는 데 가장 관련성 있는 용어 식별

**챗봇**

- 의도 감지: 사용자의 메시지에 담긴 의미를 파악하고 이에 올바른 레이블을 지정합니다.
- 슬롯 채우기: 텍스트에서 주어진 엔터티의 특정 유형의 속성(도시나 날짜와 같은 슬롯) 값을 추출하는 것을 목표로 합니다.
- 대화 관리: 대화의 상태와 흐름을 관리합니다.

**텍스트 추론**

- 상식적 추론: "상식"이나 세상 지식을 사용하여 추론하는 것.
- 자연어 추론: "전제"가 주어졌을 때 "가설"이 참(함축), 거짓(모순), 또는 결정되지 않은(중립) 것인지를 판단하는 것입니다.

**가짜 뉴스 및 증오 표현 감지**

- 가짜 뉴스 감지: 거짓되고 오해의 소지가 있는 정보가 담긴 텍스트를 감지하고 걸러냅니다.
- 입장 탐지: 주요 행위자의 주장에 대한 개인의 반응을 파악하는 것으로, 가짜 뉴스 평가 접근법의 핵심 요소입니다.
- 혐오 발언 감지: 텍스트에 혐오 발언이 포함되어 있는지 감지합니다.

**텍스트-데이터 변환 및 그 반대**

- 텍스트-음성 변환: 디지털 텍스트를 소리내어 읽어주는 기술.
- 음성-텍스트 변환: 음성을 텍스트로 변환합니다.
- 텍스트-이미지 변환: 텍스트 설명과 의미적으로 일관성이 있는 사진처럼 사실적인 이미지를 생성합니다.
- 데이터-텍스트 변환: 레코드 데이터베이스, 스프레드시트, 전문가 시스템 지식 기반 등 비언어적 입력으로부터 텍스트를 생성합니다.

**텍스트 전처리**

- 공동참조 해결: 동일한 기본 현실 세계 엔터티를 참조하는 텍스트 내 언급을 클러스터링합니다.
- ==품사(POS) 태깅: 텍스트에서 단어에 품사를 표시하는 것입니다. 품사는 명사, 동사, 형용사, 부사, 대명사, 전치사, 접속사 등 유사한 문법적 속성을 가진 단어들의 집합을 말합니다.==
- 단어 의미 모호성 해소: 사전 정의된 의미 목록(일반적으로 WordNet)에서 가장 적합한 항목과 맥락에 맞는 단어를 연결합니다.
- 문법 오류 수정: 철자, 구두점, 문법 및 단어 선택 오류 등 텍스트의 다양한 오류를 수정합니다.
- 특징 추출: 텍스트에서 일반적인 숫자 특징(일반적으로 임베딩)을 추출합니다.

읽어주셔서 감사합니다! NLP에 대해 더 자세히 알고 싶으시다면 [Medium](https://medium.com/nlplanet) , [LinkedIn](https://www.linkedin.com/company/nlplanet) , [Twitter](https://twitter.com/nlplanet_) 에서 NLPlanet을 팔로우하세요 !