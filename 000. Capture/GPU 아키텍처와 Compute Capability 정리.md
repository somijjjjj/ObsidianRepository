
https://www.reddit.com/r/CUDA/comments/w7i0vb/what_exactly_is_compute_capability_cc/?tl=ko
## Compute Capability란?
- NVIDIA GPU의 **아키텍처 버전 번호**로, 하드웨어 기능(지원 명령어, Tensor Core 세대, 메모리 특성 등)을 정의.  
- 예: Turing → CC 7.5, Ampere → CC 8.6, A100 → CC 8.0, Hopper(H100) → CC 9.0, Blackwell(GB200/B200) → CC 10.0.  
- 컴파일 시 이 값에 따라 실행 가능한 커널 코드가 결정됨.  
- **호환성**: 낮은 CC로 빌드된 코드는 상위 CC에서도 실행되지만, 최신 최적화는 활용 불가.  
- **실패 예시**: `"no kernel image is available for execution on the device"` → 해당 CC용 코드 누락 시 발생.

---

## 아키텍처별 정리

### 1. Pascal (2016)
- **CC**: 6.x  
- **특징**: 효율적 FP32 연산, GDDR5/X, GPU Boost 3.0, RT/Tensor 코어 없음.  
- **목적/용도**: 일반 게이밍, 기본 CUDA 연산.

### 2. Volta (2017)
- **CC**: 7.0  
- **특징**: 첫 세대 Tensor Core, NVLink 2.0, HBM2 메모리, INT/FP 동시 연산.  
- **목적/용도**: 데이터센터, HPC, AI 학습/추론 (Tesla V100 등).

### 3. Turing (2018)
- **CC**: 7.5  
- **특징**: RT 코어 1세대, Tensor 코어, GDDR6, 정수·부동 연산 병행.  
- **목적/용도**:  
  - RTX 20 시리즈 → 레이 트레이싱/AI 가속 게이밍.  
  - GTX 16 시리즈 → 보급형 게이밍 (RT 없음).  
  - 예시: GTX 1650 Ti (노트북용, RT 없음).

### 4. Ampere (2020)
- **CC**: 8.0 (A100) / 8.6 (RTX 30 시리즈, A40/A10 등)  
- **특징**: RT 코어 2세대, Tensor 코어 3세대, PCIe 4.0, MIG 지원(A100), GDDR6X/HBM2.  
- **목적/용도**: 고해상도 게이밍, AI 학습·추론, HPC, 데이터센터.

### 5. Ada Lovelace (2022)
- **CC**: 8.9  
- **특징**: RT 코어 3세대, Tensor 코어 4세대, SER(Shader Execution Reordering), AV1 인코딩, 초대형 L2 캐시.  
- **목적/용도**: 4K/8K 게이밍, DLSS 3, 크리에이티브 작업, AI 추론.

### 6. Hopper (2022, H100)
- **CC**: 9.0  
- **특징**: 대규모 AI 연산 최적화, 최신 Tensor 기능, HBM3 메모리, NVLink 4.0.  
- **목적/용도**: 데이터센터 AI 학습/추론, 슈퍼컴퓨팅.

### 7. Blackwell (2024~)
- **CC**: 10.0 (GB200/B200)  
- **특징**: 최신 세대, AI 워크로드 극대화, CUDA 코어·메모리 대폭 확장.  
- **목적/용도**: 차세대 데이터센터, 초고성능 AI/게이밍/크리에이션.

---

## GPU 아키텍처 목적 구분

- **데이터센터 GPU**
  - 목적: AI 학습, HPC, 빅데이터 분석 같은 **대규모 연산 최적화**  
  - 특징: Tensor Core, NVLink, HBM 계열 메모리 → 확장성과 안정성 중점  
  - 드라이버 스택: CUDA Toolkit, cuDNN, TensorRT 등 AI/HPC용 최적화

- **소비자용 GPU (GeForce/RTX 등)**
  - 목적: 게이밍, 그래픽 렌더링, 개인용 AI 연산  
  - 특징: GDDR 메모리, 가격 대비 성능, 그래픽 성능 중점  
  - 드라이버 스택: Game Ready / Studio Driver 등 그래픽·멀티미디어 중심

--- 

### GPU 구성 요소

- **SM (Streaming Multiprocessor)**  
    → GPU 코어를 이루는 기본 단위.  
    CPU로 치면 "코어"에 해당하는 개념이고, SM 안에 연산 유닛, 레지스터, 캐시 등이 들어있습니다.
    
- **Tensor Core**  
    → SM 안에 있는 특수 연산 유닛.  
    행렬 곱셈 같은 **딥러닝 연산(FP16, BF16, TF32, FP8 등)**을 빠르게 계산하도록 설계된 "전용 가속기"입니다.
    
- **RT Core (Ray Tracing Core)**  
    → 역시 GPU 안에 있는 전용 연산 유닛.  
    그래픽스에서 **광선 추적(ray tracing)** 계산을 가속합니다. (주로 RTX 시리즈에 탑재)
    
- **ECC 메모리 (Error Correcting Code Memory)**  
    → GPU 보드에 붙은 메모리(GDDR/HBM)에 적용된 기능.  
    메모리에 오류가 생겼을 때 자동으로 검출·수정하는 하드웨어 레벨 기능입니다. 데이터센터 GPU에서 주로 지원해요.