
Nuitka는 Python으로 작성된 최적화 Python 컴파일러로, 별도의 설치 프로그램 없이도 실행 가능한 실행 파일을 생성합니다. 데이터 파일은 포함하거나 함께 사용할 수 있습니다.


ip 보호
	보안을 위해 소스 코드를 컴파일하세요. 상업적 사용자인 경우 모든 데이터와 파일의 보안을 유지하고 읽을 수 있는 문자열이 없는지 확인하세요.
성능
	프로그램 런타임과 출시 성능을 향상시키세요.
배포
	독립 실행형 배포판, OneFile, PyPI 휠 등을 통해 번거로움 없는 Python 배포를 즐겨보세요.

실행환경
	**Windows** , **macOS** , **Linux**

호환 버전
	**Python 3** (3.4~3.13) 및 **Python 2** (2.6, 2.7)


## 설치

### CentOS

```
# CentOS 6:
yum-config-manager --add-repo http://download.opensuse.org/repositories/home:/kayhayen/CentOS_CentOS-6/home:kayhayen.repo
# CentOS 7
yum-config-manager --add-repo http://download.opensuse.org/repositories/home:/kayhayen/CentOS_7/home:kayhayen.repo
# CentOS 8
yum-config-manager --add-repo http://download.opensuse.org/repositories/home:/kayhayen/CentOS_8/home:kayhayen.repo

# Install only one of these, not both.
yum install nuitka
yum install nuitka-unstable
```

### Pip 설치

```
python -m pip install -U Nuitka
```



# 모드 별 사용 요약


## 1. **모듈 모드** (`--module`)

- **출력 형태**: `.so`(Linux) / `.pyd`(Windows) 확장 모듈
    
- **특징**
    
    - Python에서 `import`해서 쓰는 **모듈 형태**로 컴파일
        
    - 특정 파일이나 폴더만 선택적으로 변환 가능
        
    - 기존 프로젝트 구조 유지하면서 `.py` 대신 `.so`를 배포
        
- **장점**
    
    - 필요한 부분만 변환하니 **빌드 속도 빠름**
        
    - 이미지 크기 증가 최소화
        
    - 다른 `.py` 코드와 혼합 가능
        
- **단점**
    
    - `.py`가 남아 있으면 해당 부분은 여전히 노출
        
- **사용 예시**
    
    ```bash
    python -m nuitka --module myscript.py --output-dir=build
    ```
    
    → `build/myscript.cpython-38-x86_64-linux-gnu.so` 생성
    
- **추천 상황**

	- “이미 **Docker로만 배포**하고, 컨테이너 밖 유출 방지가 최우선은 아님”
	    
	- “Celery/uvicorn 프로세스 실행 플로우를 건드리고 싶지 않음”
    

---

## 2. **스탠드얼론 모드** (`--standalone`)

- **출력 형태**: 독립 실행 가능한 디렉터리 구조
    
- **특징**
    
    - Python 인터프리터와 필요한 모든 라이브러리를 함께 묶음
        
    - `.py` 소스 없이 **바이너리와 리소스만 배포 가능**
        
    - 디렉터리 안에 `.so` / `.pyd` / 데이터 파일 등이 포함됨
        
- **장점**
    
    - 배포 시 **Python 설치 불필요**
        
    - 소스 완전 제거 가능
        
- **단점**
    
    - 디렉터리 구조가 커짐
        
    - 빌드 시간 길어짐
        
- **사용 예시**
    
    ```bash
    python -m nuitka main.py --standalone --output-dir=main.dist
    ```
    

---

## 3. **원파일 모드** (`--onefile`)

- **출력 형태**: 단일 실행 파일(`.exe` / ELF)
    
- **특징**
    
    - 스탠드얼론 모드에서 생성된 모든 파일을 하나로 압축
        
    - 실행 시 임시 폴더에 풀어서 실행
        
- **장점**
    
    - **파일 하나만 배포**하면 됨
        
- **단점**
    
    - 실행 시작 속도가 느려짐(압축 해제 시간)
        
    - 임시폴더 쓰기 권한 없으면 실행 오류 발생
        
- **사용 예시**
    
    ```bash
    python -m nuitka main.py --onefile --enable-plugin=multiprocessing
    ```
    
- **추천 상황**

	- “컨테이너 밖/타사 환경에도 배포해야 해서 **파이썬 런타임 없이 돌아가야 함**”



---

## 📌 정리 표

| 모드        | 출력물             | 장점                      | 단점                | 적합한 상황     |
| --------- | --------------- | ----------------------- | ----------------- | ---------- |
| **모듈**    | `.so` / `.pyd`  | 빠른 빌드, 부분 변환 가능         | 남은 `.py` 노출       | 핵심 모듈만 보호  |
| **스탠드얼론** | 실행 디렉터리         | Python 설치 불필요, 소스 완전 제거 | 크기 큼, 빌드 느림       | 서버·패키지형 배포 |
| **원파일**   | 단일 `.exe` / ELF | 배포 간단, 파일 하나로 전달        | 실행 느림, 임시폴더 권한 필요 | 배포 편의성 최우선 |




# 사용 예시

```bash
python -m nuitka main.py --standalone --enable-plugin=multiprocessing --enable-plugin=eventlet --output-dir=/api/main.dist
```

**1. python -m nuitka**

- `nuitka` 패키지를 **Python 모듈 실행 방식**(`m`)으로 호출
- Nuitka는 `.py` 파일을 C++ 코드로 변환 후 컴파일해서 실행 속도를 높이거나, 소스 코드 보호 목적으로 사용

---

**2. [main.py](http://main.py)**

- 컴파일할 진입점(Entry Point) 파일
- 여기서 `main.py`가 프로그램 시작점 역할을 함

---

**3. -standalone**

- **독립 실행 모드**
    
- Python 런타임과 모든 필요한 의존성(라이브러리, `.py` 파일 등)을 **하나의 실행 폴더**에 넣어서,
    
    Python이 설치되지 않은 환경에서도 실행 가능하게 함
    
- PyInstaller의 `-onefile`과 비슷하지만, Nuitka는 속도·호환성 측면에서 좀 더 안정적
    

---

**4. -enable-plugin=multiprocessing**

- Python `multiprocessing` 모듈 사용 시 필요한 Nuitka 플러그인 활성화
- `multiprocessing`이 프로세스를 새로 띄울 때 발생하는 경로 문제를 해결해 줌

---

**5. -enable-plugin=eventlet**

- **Eventlet** 라이브러리 사용 시 필요한 Nuitka 플러그인
- Eventlet은 비동기 네트워킹 라이브러리로, Celery의 `-pool=eventlet` 같은 구조에서 필요
- 이 플러그인을 활성화해야 Eventlet 관련 패치·호환 설정이 자동 적용됨

---

**6. -output-dir=/api/main.dist**

- 빌드 결과물을 저장할 경로 지정
- `/api/main.dist` 디렉토리에 실행 파일과 필요한 라이브러리들이 저장됨

---