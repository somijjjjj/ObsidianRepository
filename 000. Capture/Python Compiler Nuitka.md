
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
