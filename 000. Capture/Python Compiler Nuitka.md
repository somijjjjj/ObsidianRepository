
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

## Module 모드 (`--module`)

**장점**

- Docker 레이어(베이스 이미지 + `pip install`)를 그대로 활용 → **작고 예측 가능한 이미지**.
    
- 기존 코드/명령을 거의 그대로 유지:  
    `celery -A celery_app worker` / `uvicorn main:rest ...` 그대로 OK.
    
- `eventlet`, `multiprocessing` 같은 runtime 패턴과의 **호환성 좋음**.
    
- MeCab 같이 **시스템 라이브러리**(libmecab, 사전 경로)도 기존 방식 그대로 사용.
    

**주의**

- `.so`(확장 모듈)로 컴파일된 파일만 교체하므로, 엔트리포인트(`main.py`, `celery_app.py`)를 `.py`로 남기거나, 엔트리도 `.so`로 돌리려면 import 경로 유지가 필요.
    

**추천 상황**

- “이미 **Docker로만 배포**하고, 컨테이너 밖 유출 방지가 최우선은 아님”
    
- “Celery/uvicorn 프로세스 실행 플로우를 건드리고 싶지 않음”
    

## Standalone 모드 (`--standalone` [+ `--onefile` 옵션 가능])

**장점**

- 타겟 머신에 파이썬이 없어도 실행(이점은 Docker 내부에선 작음).
    
- 코드 노출 방지 강도 면에서 선호할 수 있음(바이너리/번들 형태).
    

**단점/주의**

- **이미지 커짐 + 빌드 시간 증가**.
    
- Celery는 **여러 프로세스/스레드/그린스레드**를 스폰합니다. Standalone일 때는  
    `--enable-plugin=multiprocessing`(필수에 가깝고), 경우에 따라 `eventlet` 플러그인도 고려해야 합니다.
    
- 런타임에 **동적 import/리소스 경로**(예: MeCab 사전)가 꼬일 수 있어 **data-files 포함**이나 환경변수 세팅 필요.
    
- `celery multi` 같이 CLI 의존 흐름은 “독립 바이너리로 바꿨을 때” 호출 방식을 조정해야 할 때가 있음.
    

**추천 상황**

- “컨테이너 밖/타사 환경에도 배포해야 해서 **파이썬 런타임 없이 돌아가야 함**”