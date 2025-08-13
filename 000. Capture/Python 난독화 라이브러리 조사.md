


# 1. 개요

- **목적**
  
  ![[Pasted image 20250813095738.png|300]]
    
    파이썬은 인터프리터 언어로, 배포 시 `.py` 소스코드가 그대로 노출되는 경우가 많음.
    
    → 서비스 배포 시 **역공학·무단 복제·코드 변조 방지**를 위해 난독화 또는 암호화가 필요.
    
- **필요성**
    
    - 무단 복제 및 배포 방지
    - 역공학(Reverse Engineering) 난이도 상승
    - 코드 변조 방지 및 보안 강화

---

# 2. 라이브러리

## 2-1. **PyArmor - 무료 버전 한계**

- **방식**: `.py` → 바이트코드(`.pyc`) → 암호화 → 실행 시 런타임 복호화
- **보호 강도**: 높음 (복호화된 바이트코드만 메모리에 존재)
- **특징**
    - 라이선스 락(기간, 기기 바인딩) 가능
    - 7.x 버전: 오픈소스에 가까움, 무료 기능 많음
    - 8.x 버전: 명령어 구조 변경, 상용 기능 강화
- **단점**
    - 무료판은 32,768 bytes 초과 코드 객체 난독화 불가
    - 상용 라이선스 필요

### 설치 및 사용방법

1. 설치

```bash
pip install pyarmor==7.6.0
```

- 오픈소스에 가까운 7.6 버전으로 설치
- 버전 8부터 구조와 명령어가 바뀌고, 라이선스 정책도 변경되어서 상용 쪽으로 비중이 커짐

2. 사용방법

3. 난독화 할 Python 스크립트 준비
    
    ```bash
    # hello.py
    print("Hello, PyArmor!")
    ```
    
4. 스크립트 난독화
    
    - 단일 파일 난독화
    
    ```bash
    pyarmor obfuscate [hello.py](<http://hello.py/>)
    ```
    
    터미널 또는 명령 프롬프트에서 다음 명령어를 실행하여 [hello.py](http://hello.py/) 파일을 난독화
    
    이 명령은 현재 디렉토리에 `dist` 폴더를 생성하고, 그 안에 난독화된 스크립트와 PyArmor 런타임 파일을 저장
    
    - 특정 경로의 하위 폴더까지 난독화
    
    ```bash
    # 1) dist 비우기(선택)
    rm -rf dist
    ```
    
    ```bash
    # 2) 전체 재귀 난독화
    pyarmor obfuscate -r -O dist \\
      --exclude venv --exclude .venv --exclude __pycache__ --exclude tests \\
      main.py
    ```
    
5. 난독화 된 스크립트 실행
    
    ```bash
    cd dist 
    python hello.py
    ```
    
    ![[Pasted image 20250813145342.png]]
    `dist` 디렉터리 내 Python 파일을 실행
    
    원본 스크립트와 동일한 출력을 생성하지만, 소스 코드는 난독화 되어 있어서 원본 코드를 직접 보거나 수정하기 어려움
    
6. 그 외 기능
    
    프로젝트 관리, 라이선스 관리, 슈퍼 모드 난독화 등 - PyArmor 문서를 참조
    

7. 주의사항

- 🗒️ Pyarmor 버전 차이로 인한 실행 오류
    
    ```bash
    C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api>pyarmor obfuscate main.py
    Pyarmor 8.0+ has only 3 commands: gen, reg, cfg
    Please replace `pyarmor` with `pyarmor-7` to run old commands
    ```
    
    - **Pyarmor 7.x**
        - 예전 버전은 기본 기능(난독화, 실행파일 생성 등)이 **GPL 라이선스**로 제공되어서 사실상 오픈소스에 가깝게 무료 사용 가능했어요.
        - 일부 고급 기능만 상용 라이선스 필요.
    - **Pyarmor 8.x**
        - 구조와 명령어가 바뀌었고, 라이선스 정책도 변경되어 상용 쪽 비중이 커졌습니다.
        - 무료 버전은 기능 제약이 많고, 배포용 난독화에 제약이 있을 수 있음.

소스파일이 32,768바이트를 넘기면 무료버전으로 난독화 불가능

- 🚨 Too big code object, the limitation is 32768 bytes in trial version
    
    ```bash
    INFO     Start obfuscating the scripts...
    INFO            celery\\celery_app.py -> dist\\celery\\celery_app.py
    INFO            function\\apiReqProcess.py -> dist\\function\\apiReqProcess.py
    INFO            function\\textAnalysis.py -> dist\\function\\textAnalysis.py
    ERROR    Too big code object, the limitation is 32768 bytes in trial version
    ```
    
    - 체험판에서는 _하나의 코드 객체_가 32,768바이트를 넘으면 난독화를 중단
    
    
    - textAnalysis.py가 64KB

### 로컬 테스트

1. 프로젝트 전체를 난독화 실행
    
2. 에러 발생
    
3. 에러 발생 지점 확인 ([textAnalysis.py](http://textAnalysis.py))
    
4. 해당 파일만 난독화
    
    ```bash
    pyarmor obfuscate --exact -O dist_textAnalysis function/textAnalysis.py
    ```
    
    ```bash
    Start obfuscating the scripts...
    INFO            C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\textAnalysis.py -> dist_textAnalysis\\textAnalysis.py
    ERROR    Too big code object, the limitation is 32768 bytes in trial version
    ```
    
5. 파일 크기가 큰 경우 해결 방법
    
    1. 라이센스 구매
    2. 코드 분할

---

## 2-2. **pyminifier**

[Installation — Python Minifier 2.11.3 documentation](https://dflook.github.io/python-minifier/installation.html)

**Python 소스코드를 난독화(obfuscation)**, 압축(minification), 그리고 경량화(compaction)할 수 있는 파이썬 전용 도구

- **방식**: 텍스트 레벨 난독화(식별자 변경, 주석·공백 제거)
- **보호 강도**: 낮음~중간 (코드 복원 가능성 높음)
- **특징**
    - 완전 무료 (MIT)
    - 속도 저하 없음
    - 코드 압축 효과
- **장점**
    - 간단, 가벼움
    - 도커 빌드 과정에 쉽게 통합 가능
- **단점**
    - 실행 시 평문 코드 그대로 존재 → 보안 목적으론 한계

### 설치 및 사용 방법

설치

```bash
pip install python-minifier

```

사용방법

```bash
python -m python_minifier --in-place ./function_op/textAnalysis.py 
```

- 난독화 대상 파일을 난독화된 코드로 교체하는 명령어

난독화 된 스크립트 실행

```bash
python your_script_min.py
```

![[Pasted image 20250813145454.png]]
---

## 2-3. **pyconcrete**

[https://github.com/Falldog/pyconcrete](https://github.com/Falldog/pyconcrete)

- **방식**: `.pyc` 바이트코드 AES 암호화 → `.pye`로 저장 → 실행 시 C 모듈로 복호화 후 로드
- **보호 강도**: 높음 (소스 파일 배포 X)
- **특징**
    - 오픈소스 (MIT)
    - 런타임 복호화, 메모리에서만 평문 존재
    - 실행 시 약간의 지연
- **주의**
    - 빌드 환경에 C 컴파일러 필요 (Windows: MSVC Build Tools)
    - pyconcrete를 사용할 때는 암호화 키를 안전하게 관리하는 것이 중요하다. 이 키는 **.pye** 파일을 해독하는 데 필요하며, 키가 유출될 경우 난독화된 코드의 보안이 위협받을 수 있다.
- **장점**
    - 무료, 소스 완전 은닉
    - 도커 이미지 배포에 적합
- **단점**
    - PyArmor처럼 라이선스 락 기능 없음

### 설치 및 사용

pip 업그레이드

```bash
python -m pip install --upgrade pip
```

설치

```bash
$ pip install pyconcrete --no-cache-dir\\
 --config-settings=setup-args="-Dpassphrase=#misodev#123" \\
 --config-settings=setup-args="-Dmode=lib" \\
 --config-settings=setup-args="-Dinstall-cli=true"
```

- 설치하려면 암호를 설정해야 함
- pip 23.1 이상 버전에서만 `-C` 또는 `--config-settings` 를 통한 인수 전달을 지원함
- `--no-cache-dir` 는 이미 오래된 암호문구로 빌드된 pip의 캐시된 패키지를 사용하지 않도록 할당
- 사용 가능한 인수
    - 설정 방법`-config-settings=setup-args="-D<argurment_name>=<value>"`
    - `passphrase` : (필수) 암호화를 위한 비밀 키를 생성합니다.
    - `ext.pye` : 사용자 지정 암호화된 파일 확장자를 지정할 수 있습니다. 기본값은 `.pye`
    - `modeexelibexe` : 기본값은 `exe`또는 `lib` 입니다
    - `install-clipyeclitrue`: 설치할지 여부를 결정합니다. 기본값은 무엇입니까? `ture`

설치 성공

```bash
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\dist>python -m pip install --no-cache-dir pyconcrete --config-settings=setup-args="-Dpassphrase=misodev#123"
Collecting pyconcrete
  Downloading pyconcrete-1.1.0.tar.gz (53 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: pyconcrete
  Building wheel for pyconcrete (pyproject.toml) ... done
  Created wheel for pyconcrete: filename=pyconcrete-1.1.0-py3-none-win_amd64.whl size=42758 sha256=950ead8d67a6b48521820744049aa25ce3ea75e526f32d194976d3e74e8abc1f
  Stored in directory: C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-ephem-wheel-cache-aka3umhl\\wheels\\57\\00\\21\\7c9f1a6630ebb1c7a5fbe431b736ff7b970940af208f6dbfea
Successfully built pyconcrete
Installing collected packages: pyconcrete
Successfully installed pyconcrete-1.1.0
```

- 해결 - Windows에 **MSVC**(VS Build Tools) 설치
    
    1. PowerShell(관리자)에서 빌드툴 설치:
    
    ```powershell
    winget install --id Microsoft.VisualStudio.2022.BuildTools -e
    ```
    
    - 설치 화면에서 **C++ Build Tools**, **Windows 10/11 SDK** 체크
	    ![[Pasted image 20250813145524.png]]
    
    - ✅ C++ 빌드툴 제대로 설치됐는지 확인
        
        1. “**Visual Studio Installer**” 실행 → **Build Tools 2022** “수정(Modify)”.
        2. **Workloads** 탭에서 **C++ build tools** 체크.
        3. *Individual components(개별 구성요소)**에서 최소한 이 4가지가 체크되어 있어야 해요:
            - **MSVC v143** C++ x64/x86 build tools
            - **Windows 10 SDK**(10.0.19041 이상) 또는 Windows 11 SDK
            - **C++ CMake tools for Windows**
            - (선택) **Windows Universal CRT SDK**
        4. 설치 완료.
        
        > winget만으로 설치하면 워크로드 선택이 비어있을 수 있으므로 꼭 인스톨러에서 위 항목을 확인/체크!
        
    
    ![[Pasted image 20250813145534.png]]
    ```bash
    Collecting pyconcrete
      Downloading pyconcrete-1.1.0.tar.gz (53 kB)
      Installing build dependencies ... done
      Getting requirements to build wheel ... done
      Installing backend dependencies ... done
      Preparing metadata (pyproject.toml) ... error
      error: subprocess-exited-with-error
    
      × Preparing metadata (pyproject.toml) did not run successfully.
      │ exit code: 1
      ╰─> [21 lines of output]
          + meson setup C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-3zhy1eon\\pyconcrete_357bdd231ac940feb8f522123db8627b C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-3zhy1eon\\pyconcrete_357bdd231ac940feb8f522123db8627b\\.mesonpy-igw0og9r -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-3zhy1eon\\pyconcrete_357bdd231ac940feb8f522123db8627b\\.mesonpy-igw0og9r\\meson-python-native-file.ini
          The Meson build system
          Version: 1.8.3
          Source dir: C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-3zhy1eon\\pyconcrete_357bdd231ac940feb8f522123db8627b
          Build dir: C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-3zhy1eon\\pyconcrete_357bdd231ac940feb8f522123db8627b\\.mesonpy-igw0og9r
          Build type: native build
          WARNING: Failed to activate VS environment: Could not find C:\\Program Files (x86)\\Microsoft Visual Studio\\Installer\\vswhere.exe
          Project name: pyconcrete
          Project version: 1.1.0
    
          ..\\meson.build:1:0: ERROR: Unknown compiler(s): [['icl'], ['cl'], ['cc'], ['gcc'], ['clang'], ['clang-cl'], ['pgcc']]
          The following exception(s) were encountered:
          Running `icl ""` gave "[WinError 2] 지정된 파일을 찾을 수 없습니다"
          Running `cl /?` gave "[WinError 2] 지정된 파일을 찾을 수 없습니다"
          Running `cc --version` gave "[WinError 2] 지정된 파일을 찾을 수 없습니다"
          Running `gcc --version` gave "[WinError 2] 지정된 파일을 찾을 수 없습니다"
          Running `clang --version` gave "[WinError 2] 지정된 파일을 찾을 수 없습니다"
          Running `clang-cl /?` gave "[WinError 2] 지정된 파일을 찾을 수 없습니다"
          Running `pgcc --version` gave "[WinError 2] 지정된 파일을 찾을 수 없습니다"
    
          A full log can be found at C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-3zhy1eon\\pyconcrete_357bdd231ac940feb8f522123db8627b\\.mesonpy-igw0og9r\\meson-logs\\meson-log.txt
          [end of output]
    
      note: This error originates from a subprocess, and is likely not a problem with pip.
    error: metadata-generation-failed
    
    × Encountered error while generating package metadata.
    ╰─> See above for output.
    
    note: This is an issue with the package mentioned above, not pip.
    ```
    
    설치 후 **“x64 Native Tools Command Prompt for VS 2022”** 실행
    
    (꼭 이 프롬프트에서 하세요. Git Bash/MINGW64 말고)
    
    ```bash
    where cl
    where link
    where nmake
    ```
    
    - 경로가 출력되어야 하고, cl 실행 시 버전 배너가 떠야 정상입니다.
    
    1. 빌드 툴/도구 업그레이드:
    
    ```bash
    python -m pip install --upgrade pip setuptools wheel meson-python ninja
    ```
    
    2. pyconcrete 설치:
    
    ```bash
    python -m pip install pyconcrete
    ```
    
    - 에러 발생
        
        ```bash
        
        C:\\Windows\\System32>python -m pip install pyconcrete
        Collecting pyconcrete
          Using cached pyconcrete-1.1.0.tar.gz (53 kB)
          Installing build dependencies ... done
          Getting requirements to build wheel ... done
          Preparing metadata (pyproject.toml) ... error
          error: subprocess-exited-with-error
        
          × Preparing metadata (pyproject.toml) did not run successfully.
          │ exit code: 1
          ╰─> [18 lines of output]
              + meson setup C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-xx93fe1m\\pyconcrete_90a2ea9329744eaea94b96ed83629bec C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-xx93fe1m\\pyconcrete_90a2ea9329744eaea94b96ed83629bec\\.mesonpy-kwdrxvax -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md --native-file=C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-xx93fe1m\\pyconcrete_90a2ea9329744eaea94b96ed83629bec\\.mesonpy-kwdrxvax\\meson-python-native-file.ini
              The Meson build system
              Version: 1.8.3
              Source dir: C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-xx93fe1m\\pyconcrete_90a2ea9329744eaea94b96ed83629bec
              Build dir: C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-xx93fe1m\\pyconcrete_90a2ea9329744eaea94b96ed83629bec\\.mesonpy-kwdrxvax
              Build type: native build
              Project name: pyconcrete
              Project version: 1.1.0
              C compiler for the host machine: cl (msvc 19.44.35214 "Microsoft (R) C/C++ 최적화 컴파일러 버전 19.44.35214(x64)")
              C linker for the host machine: link link 14.44.35214.0
              Host machine cpu family: x86_64
              Host machine cpu: x86_64
              Program python found: YES (C:\\Users\\misouser\\AppData\\Local\\Programs\\Python\\Python38\\python.exe)
              Run-time dependency python found: YES 3.8
        
              ..\\meson.build:41:4: ERROR: Problem encountered: Must assign a passphrase to build pyconcrete
        
              A full log can be found at C:\\Users\\misouser\\AppData\\Local\\Temp\\pip-install-xx93fe1m\\pyconcrete_90a2ea9329744eaea94b96ed83629bec\\.mesonpy-kwdrxvax\\meson-logs\\meson-log.txt
              [end of output]
        
          note: This error originates from a subprocess, and is likely not a problem with pip.
        error: metadata-generation-failed
        
        × Encountered error while generating package metadata.
        ╰─> See above for output.
        
        note: This is an issue with the package mentioned above, not pip.
        hint: See above for details.
        ```
        
    
    1. 확인:
    
    ```bash
    where cl
    where vswhere
    ```
    
    둘 다 경로가 잡혀야 정상입니다.
    
- 예시 환경: Windows 11, Visual Studio 2022
    
    - 🚨 오류:`..\\meson.build:1:0: ERROR: Unknown compiler(s): [['icl'], ['cl'], ['cc'], ['gcc'], ['clang'], ['clang-cl'], ['pgcc']]`
        - Visual Studio를 설치해야 합니다
            - "C++를 사용한 데스크톱 개발"을 선택하세요
            - "Windows 11 SDK"를 포함해야 합니다.
    - 🚨오류:`..\\meson.build:23:31: ERROR: Python dependency not found`
        - Python 아키텍처가 플랫폼(예: Arm64, Amd64 또는 x86)과 동일한지 확인하세요.

Python 파일을 .pye로 변환

```bash
$ pyecli compile --pye -s=<your py script>
$ pyecli compile --pye -s=<your py module dir>
$ pyecli compile --pye -s=<your py module dir> -e=<your file extension>
```

- **[SOURCE_DIR] :** 암호화 대상 python 파일 또는 디렉토리 경로
    
    이 명령은 지정된 소스 내 모든 Python 파일(.py)을 암호화하여, .pye파일로 컴파일한다.
    
- src 전체를 암호화하려면
    

```bash
pyecli compile --pye -s=.
```

생성 확인

```bash
dir /s /b *.pye
```

```bash
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api>dir /s /b *.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\hello.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\main.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\test.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\celery\\celery_app.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\apiReqProcess.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\chunking.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\dataImport.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\operators.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\postFix.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\textAnalysis.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\textPreprocess.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\function\\utils.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\mariaDB\\connectDB.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\mariaDB\\mySqlConnector.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\request\\requestApi.pye
C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api\\setting\\mongoClient.pye

```

난독화 된 스크립트 실행

```bash
pyconcrete main.pye
```

- main 프로세스 실행 안됨
    
    ```bash
    C:\\eGovFrameDev-4.3.0-64bit\\workspace-egov\\dataScan_api>pyconcrete main.pye
    INFO:     Will watch for changes in these directories: ['C:\\\\eGovFrameDev-4.3.0-64bit\\\\workspace-egov\\\\dataScan_api']
    INFO:     Uvicorn running on <http://0.0.0.0:8888> (Press CTRL+C to quit)
    INFO:     Started reloader process [17560] using StatReload
    ```
    
    - 정상적으로 uvicorn 실행되었으나 api 통신이 안되는 상태
    - 현재는 Uvicorn이 `reload=True`로 실행돼서 “리로더 프로세스만 뜬 상태
        - 리로더는 내부적으로 새 하위 프로세스를 python으로 다시 띄우는데, 그 아이는 pyconcrete 래퍼를 거치지 않아서 암호화(.pye) 모듈을 못 불러오고 죽습니다. 그래서 API가 안 먹는 것
    

![image.png](attachment:4752eea5-a7a5-4fc7-8e99-1d9d4139c499:image.png)

---

## **2-4. Cython**

- **방식**: `.py` → C 코드 → 확장 모듈(.so/.pyd) 컴파일
- **보호 강도**: 높음 (바이너리 형태)
- **특징**
    - 성능 최적화 가능
    - 일부 모듈만 선택적 컴파일 가능
- **장점**
    - 속도 향상 + 소스 은닉
- **단점**
    - 빌드 스크립트 필요, 일부 코드 수정 필요 가능

### 설치 및 사용방법

설치

```bash
pip install cython
```

사용방법

1. 원본 파일의 확장자를 .pyx 로 변경 `my_script.pyx`
    
2. [setup.py](http://setup.py/) 파일 생성
    
    ```bash
    from setuptools import setup
    from Cython.Build import cythonize
    
    setup(
        ext_modules=cythonize("my_script.pyx")
    )
    ```
    
3. Cpython 모듈 빌드
    
    ```bash
    python setup.py build_ext --inplace
    ```
    
4. Python 파일 암호화
    

```bash
pyconcrete-admin.py compile --source=[SOURCE_DIR/FILE] --pye
```

- **[SOURCE_DIR] :** 암호화 대상 python 파일 또는 디렉토리 경로
    
    이 명령은 지정된 소스 내 모든 Python 파일(.py)을 암호화하여, .pye파일로 컴파일한다.
    

1. **컴파일된 모듈 사용하기**
    
    ```bash
    import example
    ```
    
    Cython으로 컴파일된 모듈은 일반 Python 모듈처럼 임포트하여 사용할 수 있다.
    

---

## 2-6. Nuitka

[https://github.com/Nuitka/Nuitka](https://github.com/Nuitka/Nuitka)

- **방식**: `.py` → C++ → 기계어 실행 파일/모듈
- **보호 강도**: 매우 높음
- **특징**
    - 소스 없이 배포 가능
    - 독립 실행 파일 생성 가능
- **장점**
    - 역공학 난이도 매우 높음
    - 전체 프로젝트 변환 가능
- **단점**
    - 빌드 시간 길어짐, 이미지 크기 증가

### 설치 및 사용방법

설치

```bash
pip install nuitka
```

```bash
pip install nuitka ordered-set zstandard

```

사용방법

```bash
python -m nuitka \\
    --standalone \\
    --follow-imports \\
    --python-version=3.8 \\
    --output-dir=/app/build \\
    main.py
```

- --standalone: 실행에 필요한 파일 묶음 생성(서버에 적합)

```bash
python -m nuitka --module "$f" --output-dir=/api/build
```

```bash
python -m nuitka main.py --standalone --onefile --enable-plugin=multiprocessing --output-filename=/app/api_server

```

---

## 2-7. Opy

[https://github.com/QQuick/Opy](https://github.com/QQuick/Opy)

- **방식**: 소스코드 난독화(식별자 변경, 문자열 난독화)
- **보호 강도**: 중간
- **특징**
    - 설정 파일로 난독화 범위/대상 지정 가능
    - 멀티 모듈 지원
- **장점**
    - 무료, 세밀한 제어 가능
- **단점**
    - 실행 시 평문 코드 필요 → 보안 제한

### 설치 및 사용방법

설치

```bash
pip install Opy
```

설정 파일(opy_config.txt)

난독화 대상은 `./function`만, 결과물은 임시 디렉터리로 뽑은 뒤 교체합니다.

**포인트**: 패키지/모듈 이름은 유지(`obfuscate_module_names = no`), 외부에서 import하는 **공개 API 식별자**는 난독화 제외로 지정하세요.

```
# opy_config.txt
# 난독화 대상 소스 경로(핵심 모듈)
source_path = ./function

# 난독화 결과물 경로(임시)
output_path = ./_obf/function

# 처리할 확장자
ext = .py

# 제외할 파일(필요 시 추가)
exclude_files = __init__.py

# 공개 API(메인/셀러리에서 import하는 함수/클래스/상수 이름을 나열)
# 예: run, Service, VERSION 등
exclude_identifiers = run, Service, VERSION

# 문자열/모듈명 난독화 여부 (모듈명은 유지 권장)
obfuscate_strings = yes
obfuscate_module_names = no

```

> 공개 API 이름을 정확히 넣는 게 핵심입니다.
> 
> (예: `from function.service import run` 하면 `run`은 그대로 두어야 `main.py`와 `celery` 코드가 그대로 동작합니다.)

난독화 실행

```bash
python opy.py [SOURCE_DIR] [TARGET_DIR] [CONFIG_PATH]
```

```bash
python -m opy ./function ./opy ./opy_config.txt
```

```bash
python  opy -c opy_config.txt
```

---

## 라이브러리 비교

|라이브러리|방식|보호 강도|장점|단점|추천 상황|
|---|---|---|---|---|---|
|**PyConcrete**|바이트코드 AES 암호화(.pye) + C 런타임 복호화|중~높|적용 쉬움, 소스 미포함, 오픈소스|라이선스 락 없음, 약간의 런타임 오버헤드|**운영 편의성 + 소스 비공개** 필요|
|**Nuitka**|C++로 컴파일 → 바이너리(.so/.exe)|매우 높음|역공학 난이도 최고, 소스 완전 제거|빌드 느림, 이미지 커짐, 설정 복잡|**보안 최우선 상용 배포**|
|**pyminifier**|텍스트 난독화/압축|낮~중|매우 간단·빠름, 추가 런타임 無|복원 가능성 높음, 보안 약함|**가벼운 보호/코드 최소화**|
|**PyArmor**|바이트코드 암호화 + 전용 런타임|높음|라이선스 락(기간/기기) 지원|8.x 상용 비중↑, **트라이얼 32KB 코드 객체 제한**|**정품 라이선스로 상용 배포 + 라이선싱 제어**|
|**Cython**|C로 변환해 확장모듈(.pyd/.so)|높음|성능 최적화 가능, 부분 적용 용이|빌드 스크립트 필요, 코드 수정 가능성|**핵심 알고리즘 보호 + 성능 향상**|
|**Opy**|소스 난독화(식별자/문자열)|중|무료, 범위/규칙 제어 쉬움|평문 .py 필요(실행 시), 보안 한계|**코드 가독성 저하로 분석 지연**|

- ⚖️ PyArmor vs pyminifier vs **pyconcrete**차이점
    
    |항목|**PyArmor**|**pyminifier**|**pyconcrete**|
    |---|---|---|---|
    |**난독화 방식**|**바이트코드 암호화 + 런타임 복호화**|**소스 코드 변환(텍스트 레벨)**|**바이트코드 암호화 + C 런타임 복호화**|
    |**보호 강도**|매우 강함 (실행 시 복호화된 바이트코드만 메모리에 존재)|낮음~중간 (소스 구조를 변형하지만, 복원 가능성 높음)|높음 (PyArmor보다는 단순 구조)|
    |**복원 난이도**|역공학이 매우 어려움|일부 역공학 툴로 비교적 쉽게 복원 가능|역공학 난이도 높음, 하지만 PyArmor 대비 런타임 구조 단순|
    |**동작 원리**|`.py` → `.pyc` 바이트코드 생성 후 암호화 → 실행 시 PyArmor 런타임이 복호화 후 로드|식별자 이름 변경, 주석 제거, 코드 압축(화이트스페이스 제거)|`.py` → `.pyc` → AES 암호화 → `.pye` 생성 → 실행 시 C 기반 런타임이 복호화 후 메모리에서 로드|
    |**실행 파일 형태**|난독화된 `.py`(런타임 포함) → 그대로 실행 가능|변환된 `.py` 그대로 실행 가능|`.pye` 파일과 런타임 모듈([pyconcrete.so/.pyd](http://pyconcrete.so/.pyd))로 실행|
    |**성능 영향**|미미~약간 느려질 수 있음 (런타임 복호화 과정)|거의 없음|런타임 복호화로 약간의 지연 발생|
    |**배포 형태**|난독화된 소스 + PyArmor 런타임(`pytransform` 등)|난독화된 소스만 배포|`.pye` 암호화 파일 + pyconcrete 런타임 배포|
    |**추가 기능**|실행 기간 제한, 기기 바인딩(라이선스 락), 패키징 옵션 등|단순 난독화, 압축, Base64 인코딩|없음 (순수 암호화/실행 기능만 제공)|
    |**라이선스**|**부분 무료** (상용, trial 버전 제한)|**오픈소스** (MIT License), 완전 무료|**오픈소스** (MIT License), 완전 무료|
    |**적합한 상황**|상용 서비스, 폐쇄형 배포, 소스 유출 방지가 중요한 경우. 실행 기간 제한, 특정 기기에서만 실행 등 라이선스 제어 필요|오픈소스 프로젝트, 가벼운 난독화. 속도 저하 없이 파일 크기 줄이기(압축 효과). 보안보다는 “코드 읽기 불편하게 만들기” 목적|무료이면서 바이트코드 암호화 기반 보안을 원할 때. PyArmor만큼 복잡한 기능은 필요 없고, 단순히 소스 보호와 실행만 필요할 때|
    
- ⚖️ Nuitka vs Cpython
    
    |항목|Nuitka|Cython|
    |---|---|---|
    |변환 대상|**순수 Python** → C++ 코드 → 실행 파일/확장 모듈|**Python + Cython 확장 문법** → C 코드 → 확장 모듈|
    |목적|**완전한 파이썬 프로그램을 컴파일**해서 배포 가능 (원본 소스 숨김)|성능 최적화 + C 확장 개발 (보호 목적은 부가 효과)|
    |코드 수정 필요성|거의 없음 (기존 .py 그대로)|고성능·최적화를 하려면 `cdef`, `cpdef` 등 Cython 문법 써야 함|
    |빌드 결과|단일 실행 파일(`.exe`) 또는 공유 라이브러리(.so/.pyd)|주로 `.pyd`(Windows) / `.so`(Linux) 확장 모듈|
    |성능 향상|CPython 그대로 돌리는 경우는 성능 향상 크지 않음(보호 중심)|타입 지정 최적화 시 상당한 성능 향상 가능|
    |배포 난이도|비교적 쉬움 (`--onefile`, `--follow-imports`)|빌드 스크립트/설정 필요, C 컴파일러 환경 세팅 필요|
    |보안(소스 보호)|매우 강함 — C++ 바이너리로 빌드되어 역공학 난이도 높음|.pyd/.so로 빌드되어도 바이너리 디컴파일 가능성 있음|
    |적합한 상황|- 소스 코드 노출 방지가 최우선||
    
    - 기존 프로젝트를 거의 수정하지 않고 그대로 빌드하고 싶음
    - 단일 실행 파일(.exe)로 배포하고 싶음 | - 성능 최적화가 주 목표
    - C 확장과의 연동, 복잡한 수치 연산/알고리즘 최적화
    - 일부 모듈만 고성능으로 바꾸고 싶음 |
- ⚖️ Opy vs Pyconcrete
    
    |구분|**Opy**|**Pyconcrete**|
    |---|---|---|
    |**방식**|소스 코드 **난독화** (Obfuscation)|Python 바이트코드 **암호화 & 런타임 복호화**|
    |**결과물**|`.py` 파일 그대로 존재하지만, 가독성이 극도로 떨어짐|`.pyc` 대신 **암호화된 바이너리 파일** 생성|
    |**보안 수준**|중간 — 난독화지만, 여전히 역공학 가능|높음 — 암호화된 상태에서 실행, 역공학 난이도 높음|
    |**목표**|코드 읽기 어렵게 하여 분석 지연|코드 자체를 배포 시 **노출 차단**|
    |**장점**|- 오픈소스 & 무료||
    
    - 설정 파일로 난독화 범위 세밀 제어
    - 멀티 모듈 지원 | - 코드 자체를 암호화해 안전성 높음
    - 역공학 난이도 높음
    - 실행 시 복호화가 메모리에서만 이루어짐 | | **단점** | - 보안 한계 있음(시간 들이면 복원 가능)
    - 실행 시 평문 소스 필요 | - 배포 환경에 Pyconcrete 런타임 필요
    - 컴파일·빌드 과정이 필요
    - 일부 환경에서 호환성 문제 | | **사용 용도** | - 내부 로직 숨기기
    - 리버스 엔지니어링 지연 | - 상용 SW 코드 보호
    - 고객사 배포 시 소스 완전 은닉 |

---

# 3. 테스트

> 파이썬 프로젝트 난독화 조건

- Docker 이미지로 배포
- 핵심 소스 < 10개
- 외부에서 **소스(.py) 열람 불가**가 목표
- 용량/라이선스 이슈로 **PyArmor는 제외**

## 옵션 A) pyconcrete (오픈소스, 가볍고 세팅 쉬움)

- **장점**: 소스(.py) 배포 안 함, 이미지가 비교적 가벼움, 라이선스 걱정 X
    
- **단점**: PyArmor처럼 라이선스 락/만료일 같은 정책 기능은 없음
    
- ~~1) 프로젝트 내 모든 py 난독화하여 도커로 실행하도록 테스트~~
    
    빌드할 때 py코드는 모두 제거하고 난독화한 pye만 남긴 뒤
    
    pye로 난독화된 main과 celery를 pyconcrete 래퍼로 실행하도록 하는데
    
    작은 오류들 발생으로 실행 못함
    
    - Dockerfile (multi-stage)
        
        ```docker
        # syntax=docker/dockerfile:1
        
        ############################
        # 1) Build stage
        ############################
        FROM python:3.8-slim AS build
        WORKDIR /src
        
        # 빌드 도구
        RUN apt-get update && apt-get install -y --no-install-recommends build-essential \\
         && rm -rf /var/lib/apt/lists/*
        
        # (선택) 의존성 캐시 최적화를 위해 먼저 requirements만 복사
        # COPY requirements.txt /src/requirements.txt
        # RUN pip install --no-cache-dir -r requirements.txt
        
        # 앱 소스 복사
        COPY . /src
        
        # ---- pyconcrete 휠 빌드(+설치) & .pye 생성 ----
        # 빌드 인자: PYCONCRETE_PASS (CI/CD Secret로 주입 권장)
        ARG PYCONCRETE_PASS
        
        # 1) passphrase로 pyconcrete wheel 빌드
        RUN pip wheel --no-cache-dir pyconcrete \\
            --config-settings=setup-args="-Dpassphrase=${PYCONCRETE_PASS}" \\
            -w /wheels
        
        # 2) build 단계에 설치(pyecli 사용하려고)
        RUN pip install --no-cache-dir /wheels/pyconcrete-*.whl
        
        # 3) 프로젝트 전체 .py -> .pye
        RUN pyecli compile --pye -s=/src
        
        # 4) 배포용 번들 생성: .pye + 리소스만 (원본 .py 제거)
        #    필요한 데이터/설정 확장자만 추가하세요.
        RUN mkdir -p /bundle \\
         && cp -a /src/. /bundle/ \\
         && find /bundle -type f -name "*.py" -delete \\
         && find /bundle -type d -name "__pycache__" -prune -exec rm -rf {} +
        
        ############################
        # 2) Runtime stage
        ############################
        FROM python:3.8-slim AS runtime
        WORKDIR /app
        ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
        
        # 앱 의존성 설치 (uvicorn, fastapi 등)
        # 권장: requirements.txt를 루트에 두고 복사
        # COPY requirements.txt /app/requirements.txt
        # RUN pip install --no-cache-dir -r /app/requirements.txt
        
        # build에서 만든 pyconcrete 휠만 가져와 설치(런타임에서 passphrase 노출 방지)
        COPY --from=build /wheels /wheels
        RUN pip install --no-cache-dir /wheels/pyconcrete-*.whl \\
         && rm -rf /wheels
        
        # 암호화된 앱 파일 배포
        COPY --from=build /bundle /app
        
        EXPOSE 8888
        # 엔트리가 main.pye라면:
        CMD ["pyconcrete", "main.pye"]
        
        ```
        
        ```bash
        set PYCONCRETE_PASS="#misodev!@"
        docker build --build-arg PYCONCRETE_PASS=%PYCONCRETE_PASS% -t datascan-api:enc .
        
        ```
        
        ```bash
        docker run --rm -p 8080:80 --name datascan datascan-api:enc
        
        ```
        
        ```bash
        # 컨테이너 셸로 진입
        docker run --rm -it --entrypoint sh datascan-api:enc
        
        # 1) pyconcrete CLI 존재?
        which pyconcrete && pyconcrete --help
        
        # 2) pye 파일들 생성돼 있음?
        ls -l /api/main.pye
        ls -l /api/celery/celery_app.pye
        
        # 3) cmd.sh 내용 확인 (혹시 python pyconcrete ... 로 되어있는지)
        sed -n '1,120p' /api/cmd.sh
        
        ```
        
- 2. 핵심 모듈만 pye로 컴파일
    
    ```bash
    FROM python:3.8
    
    # 필요 패키지
    RUN apt-get update && apt-get install -y \\
        redis-server \\
        vim \\
        build-essential python3-dev pkg-config ninja-build \\        
        && rm -rf /var/lib/apt/lists/*
    
    # 앱 루트
    WORKDIR /api
    
    # pip 최신화 (중요)
    RUN python -m pip install --upgrade pip setuptools wheel
    
    # 의존성
    COPY ./requirements.txt /api/requirements.txt
    COPY ./mecab /mecab
    RUN pip install --no-cache-dir --upgrade -r /api/requirements.txt
    RUN pip install --no-cache-dir konlpy mecab-python3
    
    # ===== MeCab 설치 (요건 기존대로 유지) =====
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-0.996-ko-0.9.2.tar.gz
    WORKDIR /mecab/mecab-0.996-ko-0.9.2
    RUN ./configure && make && make check && make install && ldconfig
    
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-ko-dic-2.1.1-20180720.tar.gz
    WORKDIR /mecab/mecab-ko-dic-2.1.1-20180720
    RUN ./autogen.sh && ./configure && make && make install
    # =========================================
    
    # 앱 소스 복사 (난독화 전에 전체 복사)
    WORKDIR /api
    COPY ./ /api
    
    # ===== pyconcrete: 특정 디렉토리만 난독화 =====
    # OBFUSCATE_DIR: 난독화할 대상 디렉토리 (예: /api/app/secure)
    ARG OBFUSCATE_DIR
    ARG PYCONCRETE_PASSPHRASE
    # pyconcrete 설치 및 키 생성/내장 빌드
    RUN pip install --no-cache-dir pyconcrete==1.1.0 \\
        --config-settings=setup-args="-Dpassphrase=${PYCONCRETE_PASSPHRASE}" \\
    	# mode: lib  → import hook 방식(부분 난독화) 사용할 때 권장
        --config-settings=setup-args="-Dmode=lib" \\
        --config-settings=setup-args="-Dinstall-cli=true"
    
    # -s 소스경로, --pye: .pye 생성, --remove: 원본 .py 제거
    RUN pyecli compile --pye -s=${OBFUSCATE_DIR} --remove-py
    # sitecustomize로 pyconcrete import hook 자동 활성화
    # (컨테이너에서 어떤 파이썬 프로세스가 떠도 .pye를 해독할 수 있도록 보장)
    # 방법 A: sitecustomize로 전역 hook
    RUN python - <<'PY'
    import site, os
    p = site.getsitepackages()[0]
    open(os.path.join(p, "sitecustomize.py"), "w").write("import pyconcrete\\n")
    PY
    
    # 선택: 개발 산출물/캐시 정리 (있다면)
    # RUN rm -rf /api/tests /api/docs
    
    # 엔트리포인트 (기존 유지)
    WORKDIR /api
    CMD ["./cmd.sh"]
    
    ```
    
    ```bash
    
    docker build --build-arg PYCONCRETE_PASSPHRASE="#misodev!@" --build-arg OBFUSCATE_DIR=./function -t datascan-api:enc . 
    ```
    
    ```bash
    docker run -d -p 8888:80 --name datascan -v d:/fileupload:/share001 datascan-api:enc
    ```
    
    ```bash
    docker exec datascan sh -lc "echo [py count]; find /api/function -type f -name '*.py' | wc -l; echo [pye count]; find /api/function -type f -name '*.pye' | wc -l; echo [samples]; find /api/function -type f -name '*.pye' | head -n 10"
    
    ```
    
- py파일과 pye파일 확인
    

![image.png](attachment:5093de98-d9cf-4780-9e11-14e42e7399e1:image.png)

![image.png](attachment:5cc0a326-b097-437a-af29-63ae4496cdf0:image.png)

---

## 옵션 B) Nuitka (보호 강도↑, 소스 없이 바이너리 배포)

- **장점**: 결과가 바이너리라 소스 유출 난이도 높음, 오픈소스
- **단점**: 빌드 타임/이미지 크기가 늘 수 있음(플러그인/의존성에 따라 다름)

ini 파일은 컴파일 불가

- Dockerfile - 모듈 → 평균 2분이내 빌드
    
    - py파일 모두 컴파일
        
        ```bash
        FROM python:3.8
        
        # 빌드 도구 (Nuitka와 MeCab 빌드에 필요)
        RUN apt-get update && apt-get install -y \\
            build-essential \\
            autoconf automake libtool pkg-config \\
            redis-server \\
            vim
        
        WORKDIR /api
        
        COPY ./requirements.txt /api/requirements.txt
        COPY ./mecab /mecab
        
        RUN pip install --no-cache-dir --upgrade -r /api/requirements.txt
        RUN pip install --no-cache-dir konlpy mecab-python3
        
        # ===== MeCab 설치 =====
        WORKDIR /mecab
        RUN tar -zxvf /mecab/mecab-0.996-ko-0.9.2.tar.gz
        WORKDIR /mecab/mecab-0.996-ko-0.9.2
        RUN ./configure && make && make check && make install && ldconfig
        
        WORKDIR /mecab
        RUN tar -zxvf /mecab/mecab-ko-dic-2.1.1-20180720.tar.gz
        WORKDIR /mecab/mecab-ko-dic-2.1.1-20180720
        RUN ./autogen.sh && ./configure && make && make install
        
        # 앱 복사
        WORKDIR /api
        COPY ./ /api
        
        # ===== 🔒 Nuitka로 function 모듈 컴파일(.so) 후 .py 교체 =====
        # __init__.py는 유지, 나머지는 명시 파일만 .so로 바꿔서 원본 .py 삭제
        RUN pip install --no-cache-dir nuitka ordered-set zstandard && \\
            set -eux; mkdir -p /api/build; \\
            FILES="$(printf '%s\\n' \\
              /api/celery/celery_app.py \\
              /api/mariaDB/connectDB.py \\
              /api/mariaDB/mySqlConnector.py \\
              /api/request/requestApi.py \\
              /api/setting/mongoClient.py \\
              /api/function/apiReqProcess.py \\
              /api/function/chunking.py \\
              /api/function/dataImport.py \\
              /api/function/operators.py \\
              /api/function/postFix.py \\
              /api/function/textAnalysis.py \\
              /api/function/textPreprocess.py \\
              /api/function/utils.py )"; \\
            printf '%s\\n' $FILES | grep -E '\\.py$' | xargs -n1 -P"$(nproc)" -I{} sh -c '\\
              f="$1"; bn=$(basename "$f" .py); dn=$(dirname "$f");\\
              python -m nuitka --module "$f" --output-dir=/api/build;\\
              so=$(ls -1 /api/build/${bn}.*.so | head -n1) || exit 1;\\
              cp "$so" "$dn/${bn}.so"; rm -f "$f";\\
            ' _ {}; \\
            find /api -type d -name "__pycache__" -prune -exec rm -rf {} +; \\
            find /api -type f -name "*.pyc" -delete
        
        # ============================================
        
        CMD ["./cmd.sh"]
        
        ```
        
    
    ```bash
    
    docker build -t datascan-api:nuitka . 
    ```
    
    ```bash
    docker run -d -p 8888:80 --name datascan -v d:/fileupload:/share001 datascan-api:nuitka
    ```
    
    ![image.png](attachment:17d37d2b-83c3-469b-9544-fd80c7e22163:image.png)
    
    ![image.png](attachment:b09d7f91-c696-4248-acf4-e81786941115:image.png)
    
- Dockerfile - 스탠다론 → 빌드 2,940s = 49분
    
    ```docker
    FROM python:3.8
    
    RUN apt-get update && apt-get install -y \\
    	redis-server \\
    	vim \\
        gcc g++ make python3-dev \\
        autoconf automake libtool pkg-config \\
        patchelf 
    
    WORKDIR /api
    
    COPY ./requirements.txt /api/requirements.txt
    COPY ./mecab /mecab
    
    RUN pip install --no-cache-dir --upgrade -r /api/requirements.txt
    
    RUN pip install konlpy mecab-python3
    
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-0.996-ko-0.9.2.tar.gz
    WORKDIR /mecab/mecab-0.996-ko-0.9.2
    RUN ./configure
    RUN make
    RUN make check
    RUN make install
    RUN ldconfig
    
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-ko-dic-2.1.1-20180720.tar.gz
    WORKDIR /mecab/mecab-ko-dic-2.1.1-20180720
    RUN ./autogen.sh
    RUN ./configure
    RUN make 
    RUN make install
    
    COPY ./ /api
    
    WORKDIR /api
    
    # ===== Nuitka 빌드 =====
    RUN pip install --no-cache-dir nuitka ordered-set zstandard
    # ---- Nuitka Standalone 빌드 ----
    RUN python -m nuitka main.py \\
        --standalone \\
        --enable-plugin=multiprocessing --enable-plugin=eventlet \\
        --output-dir=/api/main.dist
    
    # 빌드 산출물을 /api 루트로 이동
    RUN cp -r /api/main.dist/* /api/ && rm -rf /api/main.dist
    
    CMD ["./cmd.sh"]
    ```
    
    ```bash
    
    docker build -t datascan-api:nuitka . 
    ```
    
    ```bash
    docker run -d -p 8888:80 --name datascan -v d:/fileupload:/share001 datascan-api:nuitka
    ```
    
    ![image.png](attachment:17d37d2b-83c3-469b-9544-fd80c7e22163:image.png)
    
    ![image.png](attachment:b09d7f91-c696-4248-acf4-e81786941115:image.png)
    
- Dockerfile - 원파일→ 빌드 2,597s = 43분
    
    - entry_allinone.py
        
        ```bash
        # /api/entry_allinone.py
        import os, sys, subprocess, signal
        
        def chdir_to_runtime_root():
            """Nuitka --onefile 실행 시 추출 디렉터리로 CWD 변경."""
            root = os.environ.get("NUITKA_ONEFILE_TEMP")
            if root and os.path.isdir(root):
                os.chdir(root)
            else:
                os.chdir(os.path.dirname(os.path.abspath(__file__)))
        
        def run_uvicorn():
            # FastAPI 앱: main.py의 FastAPI 인스턴스 이름이 'rest' 라고 가정
            from main import rest
            import uvicorn
            host = os.getenv("UVICORN_HOST", "0.0.0.0")
            port = int(os.getenv("UVICORN_PORT", "80"))
            workers = int(os.getenv("UVICORN_WORKERS", "1"))
            log_level = os.getenv("UVICORN_LOG_LEVEL", "info")
            uvicorn.run(rest, host=host, port=port, workers=workers, log_level=log_level)
        
        def import_celery_app():
            # 충돌을 피하기 위해 루트에 둔 래퍼 모듈을 통해 가져옴
            from celery_app_import import app
            return app
        
        def run_celery_worker():
            app = import_celery_app()
            argv = [
                "worker",
                "-P", "eventlet",                           # 기존과 동일
                "--loglevel", os.getenv("CELERY_LOGLEVEL", "info"),
                "-n", os.getenv("CELERY_NODENAME", "datascan@%h"),
            ]
            conc = os.getenv("CELERY_CONCURRENCY")
            if conc:
                argv += ["--concurrency", conc]
            app.worker_main(argv)
        
        def maybe_start_redis():
            if os.getenv("START_REDIS", "1") != "1":
                return
            try:
                subprocess.run(["redis-server", "--daemonize", "yes"], check=True)
                print("[INFO] redis-server started (daemonize=yes).", flush=True)
            except Exception as e:
                print(f"[WARN] redis-server start failed: {e}", flush=True)
        
        def main():
            chdir_to_runtime_root()
        
            # celery 서브모드
            if "--mode=celery" in sys.argv:
                run_celery_worker()
                return
        
            # 감독 프로세스: redis → (자기 자신을 celery 모드로) → uvicorn
            maybe_start_redis()
        
            binary = sys.argv[0]
            env = os.environ.copy()
            celery_proc = subprocess.Popen([binary, "--mode=celery"], env=env)
        
            def shutdown(_signum, _frame):
                try:
                    celery_proc.terminate()
                except Exception:
                    pass
                try:
                    celery_proc.wait(timeout=10)
                except Exception:
                    celery_proc.kill()
                sys.exit(0)
        
            signal.signal(signal.SIGTERM, shutdown)
            signal.signal(signal.SIGINT, shutdown)
        
            try:
                run_uvicorn()  # 포그라운드 유지
            finally:
                shutdown(signal.SIGTERM, None)
        
        if __name__ == "__main__":
            main()
        
        ```
        
    - celery_app_import.py
        
        ```bash
        # /api/celery_app_import.py
        import os, sys
        
        # 프로젝트 내 /api/celery 디렉터리를 임시 경로에 추가
        _here = os.path.dirname(os.path.abspath(__file__))
        _celery_dir = os.path.join(_here, "celery")
        if _celery_dir not in sys.path:
            sys.path.insert(0, _celery_dir)
        
        # 이제 'celery_app.py'를 모듈명 'celery_app'으로 임포트 가능
        from celery_app import app  # celery/celery_app.py 내부의 Celery 인스턴스
        
        ```
        
    
    ```docker
    FROM python:3.8
    
    RUN apt-get update && apt-get install -y \\
    	redis-server \\
    	vim \\
        gcc g++ make python3-dev \\
        autoconf automake libtool pkg-config \\
        patchelf 
    
    WORKDIR /api
    
    COPY ./requirements.txt /api/requirements.txt
    COPY ./mecab /mecab
    
    RUN pip install --no-cache-dir --upgrade -r /api/requirements.txt
    
    RUN pip install konlpy mecab-python3
    
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-0.996-ko-0.9.2.tar.gz
    WORKDIR /mecab/mecab-0.996-ko-0.9.2
    RUN ./configure
    RUN make
    RUN make check
    RUN make install
    RUN ldconfig
    
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-ko-dic-2.1.1-20180720.tar.gz
    WORKDIR /mecab/mecab-ko-dic-2.1.1-20180720
    RUN ./autogen.sh
    RUN ./configure
    RUN make 
    RUN make install
    
    COPY ./ /api
    
    WORKDIR /api
    
    # ---- 앱 소스 복사 이후 ----
    WORKDIR /api
    # 필요: eventlet (celery -P eventlet), nuitka
    RUN pip install --no-cache-dir nuitka ordered-set zstandard eventlet
    # (중요) 패키지로 인식되게 __init__.py 주입
    # 폴더가 없을 수도 있으니 존재할 때만 생성
    RUN python - <<'PY'
    import os
    for d in ["function","mariaDB","request","setting","celery"]:
        if os.path.isdir(d):
            open(os.path.join(d, "__init__.py"), "a").close()
    print("init files ensured")
    PY
    
    # Nuitka onefile 빌드
    # - function/mariaDB/request/setting 패키지 포함
    # - config.ini* 리소스 포함
    # - 래퍼 모듈 통해 celery_app 인식 (따라서 'celery' 패키지 자체는 include하지 않음: pip celery와 충돌 방지)
    ENV PYTHONPATH="/api:/api/celery"
    
    # Nuitka onefile 빌드 (상대경로 사용)
    RUN python -m nuitka entry_allinone.py \\
        --onefile \\
        --enable-plugin=multiprocessing \\
        --enable-plugin=eventlet \\
        --include-package=function \\
        --include-package=mariaDB \\
        --include-package=request \\
        --include-package=setting \\
        --include-module=celery_app_import \\
        --include-module=celery_app \\
        --include-data-files=config.ini=config.ini \\
    	--output-dir=/api/bin \\
        --output-filename=datascan_app
    
    # 소스 제거 (바이너리만 남김)
    RUN find /api -type d -name "__pycache__" -prune -exec rm -rf {} + && \\
        find /api -type f -name "*.py" -delete && \\
        find /api -type f -name "*.pyc" -delete
    
    # 단일 실행파일로 기동
     CMD ["./bin/datascan_app"]
    CMD ["./cmd.sh"]
    ```
    
    ```bash
    
    docker build -t datascan-api:nuitka . 
    ```
    
    ```bash
    docker run -d -p 8888:80 --name datascan -v d:/fileupload:/share001 datascan-api:nuitka
    ```
    
    ```bash
    docker run -d -p 8888:80 --name datascan -e TMPDIR=/app/.onefile_tmp -v d:/fileupload:/share001 datascan-api:nuitka_one
    ```
    
    ![image.png](attachment:17d37d2b-83c3-469b-9544-fd80c7e22163:image.png)
    
    ![image.png](attachment:b09d7f91-c696-4248-acf4-e81786941115:image.png)
    
    - 실행 파일이 임시폴더에 쓸 수 없어서 에러 발생
        
        ![image.png](attachment:84ed235f-be4b-4431-8f5a-67c09a8a6cc5:image.png)
        

### 운영 배포

![image.png](attachment:fb12115e-1f9b-4543-a458-74a328f85995:image.png)

![image.png](attachment:24ecac1e-79f9-4342-b784-9a16dc3eb4e3:image.png)

||기존 운영 배포 파일|Nuitka 적용 배포 파일|
|---|---|---|
|이미지 크기|2.81GB|2.88GB|
|빌드 시간|평균 5초 이내|평균 2분 이내|

## 옵션 C) **pyminifier (빠르고 무료, 코드 최소화)**

- **장점**: 설치·사용 간단, 빠른 변환 속도, 추가 실행 환경 불필요, 주석/공백 제거·식별자 난독화·압축 지원, 무료
    
- **단점**: 보안 수준 낮음(복원 가능), 문자열 기반 접근 로직 오류 가능, Python3 최신 호환성 떨어짐, 역공학 방지 효과 제한
    
- 도커 파일
    
    ```bash
    FROM python:3.8
    
    RUN apt-get update && apt-get install -y \\
        redis-server \\
        vim
    
    WORKDIR /api
    
    COPY ./requirements.txt /api/requirements.txt
    COPY ./mecab /mecab
    
    RUN pip install --no-cache-dir --upgrade -r /api/requirements.txt
    
    RUN pip install konlpy mecab-python3
    
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-0.996-ko-0.9.2.tar.gz
    WORKDIR /mecab/mecab-0.996-ko-0.9.2
    RUN ./configure
    RUN make
    RUN make check
    RUN make install
    RUN ldconfig
    
    WORKDIR /mecab
    RUN tar -zxvf /mecab/mecab-ko-dic-2.1.1-20180720.tar.gz
    WORKDIR /mecab/mecab-ko-dic-2.1.1-20180720
    RUN ./autogen.sh
    RUN ./configure
    RUN make
    RUN make install
    
    # 앱 복사
    WORKDIR /api
    COPY ./ /api
    
    # ===== 🔒 function 폴더 난독화(원본 교체) =====
    # __init__.py 제외, 명시적 파일 목록만 in-place 난독화
    RUN pip install --no-cache-dir python-minifier && \\
        set -eux; \\
        FILES="\\
          ./function/apiReqProcess.py \\
          ./function/chunking.py \\
    	  ./function/dataImport.py \\
    	  ./function/operators.py \\
    	  ./function/postFix.py \\
    	  ./function/textAnalysis.py \\
    	  ./function/textPreprocess.py \\
    	  ./function/utils.py \\
        "; \\
        for f in $FILES; do \\
          [ -f "$f" ] || { echo "[ERR] not found: $f"; exit 1; }; \\
          before=$(wc -c < "$f"); \\
          python -m python_minifier --in-place "$f"; \\
          after=$(wc -c < "$f"); \\
          echo "[MINIFY] $f  ${before}B -> ${after}B"; \\
        done;
    
    # ============================================
    
    CMD ["./cmd.sh"]
    
    ```
    
    ```bash
    
    docker build  -t datascan-api:enc . 
    ```
    
    ```bash
    docker run -d -p 8888:80 --name datascan -v d:/fileupload:/share001 datascan-api:enc
    ```
    

![image.png](attachment:61188bac-b745-4df1-a4f2-984ad71a43bd:image.png)

![image.png](attachment:a02d662b-622a-453d-9c92-6bbcabf0ab57:image.png)

![image.png](attachment:f985a7eb-8f96-46eb-8bc2-ad255c704000:image.png)

---

---

|옵션|보호 강도|실행 성능|이미지 크기|구현 난이도|주요 특징|추천 상황|
|---|---|---|---|---|---|---|
|**Pyconcrete**|**중간~높음**|약간 저하|가벼움|중간|- `.py` 대신 `.pye` 배포- 소스 완전 은닉- 무료/오픈소스|**“소스 노출 방지” + “배포 용량 최소화”**가 필요할 때|
|**Nuitka**|**매우 높음**|약간 향상|커짐|높음|- `.py`를 C++ 바이너리로 컴파일- 역공학 난이도 ↑- 소스 완전 제거 가능|**상용 서비스, 보안 최우선, 역공학 난이도 극대화** 필요할 때|
|**pyminifier**|낮음~중간|영향 없음|매우 가벼움|매우 쉬움|- 텍스트 난독화 + 압축- 무료/빠름- 복원 가능성 있음|**성능 영향 없이 핵심 로직만 가볍게 숨길 때**|

### 선택 기준

1. **보안 우선** → **Nuitka**
    - 완전 바이너리 변환, 소스 유출 가능성 최소화
    - 단점: 빌드 시간 길고, 이미지 크기 증가
2. **속도·운영 편의성 우선** → **Pyconcrete**
    - 소스 없이 배포 가능, 무료, 용량 부담 적음
    - 라이선스 락 등 부가기능 없음
3. **간단·빠른 적용** → **pyminifier**
    - 설치·적용 쉬움, 배포 구조 변화 거의 없음
    - 보안은 약하지만, 읽기 난이도 상승 + 코드 압축 효과

### 이미지 빌드, 크기 비교

|옵션|빌드시간|이미지 크기|코멘트|
|---|---|---|---|
|**Nuitka**|**120s**|2.86GB|보안성 가장 높음|
|**Pyconcrete**|18.6s|2.85GB|실행 구조 단순, 적용 쉬움|
|**pyminifier**|13.8s|**2.81GB**|크기 최소, 하지만 보안 약함|

- **내부 서버·클라우드 서비스** → **PyConcrete**가 많음
    - 빌드 간단하고, 런타임 환경만 맞추면 실행 가능
    - 관리 용이 + 무료
- **외부에 배포하는 상용 프로그램** → **Nuitka**가 선호
    - 특히 **윈도우 배포형**, **리눅스 배포형 CLI** 프로그램
    - 보안 수준을 최우선으로 하는 경우