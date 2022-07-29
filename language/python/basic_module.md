# 모듈

### 모듈과 패키지

- 모듈(module): 다양한 기능을 하나의 파일로 묶은 것
- 패키지(package): 다양한 파일(모듈)을 하나의 폴더로 묶은 것
- 라이브러리(library): 다양한 폴더(패키지)를 하나로 묶은 것
    - 참고. 라이브러리 vs 프레임워크: buzzword
- pip: 이것을 관리하는 관리자(파이썬)
- 가상 환경: 패키지의 활용 공간

- 모듈
    - 특정 기능을 하는 코드를 파이썬 파일(.py) 단위로 작성한 것
- 패키지
    - 특정 기능과 관련된 여러 모듈의 집합
    - 패키지 안에는 또 다른 서브 패키지를 포함

### 모듈과 패키지 불러오기

- import module
- from module import var, funciton, Class
- from module import * (전부 다)
- from package import module
- from package.module import var, funcion, Class

### 파이썬 표준 라이브러리

- 파이썬에 기본적으로 설치된 모듈과 내장 함수(파이썬 공식 문서 확인)

### 파이썬 패키지 관리자(pip)

- PyPI(Python Package Index)에 저장된 외부 패키지들을 설치하도록 도와주는 패키지 관리 시스템
- pip에서 설치하고 import로 가져옴
- 패키지 설치
    - 최신 버전/특정 버전/최소 버전을 명시하여 설치할 수 있음
        - $ pip install SomePackage
        - $ pip install SomePackage==1.0.5
        - $ pip install ‘SomePackage>=1.0.4’
    - 이미 설치되어 있는 경우 이미 설치되어 있음을 알리고 아무것도 하지 않음
- 패키지 삭제
    - $ pip uninstall SomePackage
    - pip는 패키지 업그레이드를 하는 경우 과거 버전을 자동으로 지워줌
- 패키지 목록
    - $ pip list
- 특정 패키지 정보
    - $ pip show SomePackage
- 패키지 관리하기
    - 아래의 명령어들을 통해 패키지 목록을 관리[1]하고 설치할 수 있음[2]
    - 일반적으로 패키지를 기록하는 파일의 이름은 requirements.txt로 정의함(컨벤션)
    - $ pip freeze > requirements.txt
    - $ pip install -r requirements.txt

# 사용자 모듈과 패키지

### 패키지

- 패키지는 여러 모듈/하위 패키지로 구조화
    - 활용 예시: package.module
- 모든 폴더에는 `__init__.py`를 만들어 패키지로 인식
    - Python 3.3부터는 파일이 없어도 되지만, 하위 버전 호환 및 프레임워크 등에서의 동작을 위해 파일을 생성하는 것을 권장

### 패키지 만들기

- 계산 기능이 들어간 calculator 패키지를 아래와 같이 구성
    - check.py에서 calculator의 tools.py의 기능을 사용
- 폴더 구조

```python
my_package/
  __init__.py
  check.py
  calculator/
    __init__.py
    tools.py
```

### 패키지 활용

- calculator/tools.py 에 add 함수와 minus 함수 구성
- 모듈을 활용하기 위해서는 import문을 통해 가져옴
    
    ```python
    from calculator import tools
    print(tools.add(1,2))
    print(dir(tools))
    ```
### if \_\_init__ == ‘\_\_main__’:

해당 모듈이 임포트된 경우가 아니라 인터프리터에서 직접 실행된 경우에만, if문 이하의 코드를 돌리라는 명령

- \_\_name__ 은 모듈의 이름을 담는 파이썬 내장변수이다.
- 인터프리터로 직접 실행된 모듈의 경우 \_\_name__ 은 \_\_main__이라는 값을 가지게 되며, 직접 실행되지 않은 import된 모듈의 경우 \_\_name__은 모듈의 이름(파일명)을 가지게 된다.
- 따라서 `if \_\_init__ == '\_\_main__':` 은 \_\_init__ 이 \_\_main__일 때 즉 import하지 않고 직접 interpreter로 실행한 모듈(현재 파일)의 코드에 대해서만 해당 if문의 코드를 실행하라는 명령이다.
- 왜 이렇게 하냐면, 어떤 모듈을 import할 경우 그 모듈 내부의 코드가 원하지 않는 상황에서 함께 실행될 수도 있기 때문에 이를 방지하기 위해서이다.
- 참고로 어떤 모듈을 import하면 기본적으로 한 번 실행된다.

<br>

# 가상 환경

- 파이썬 표준 라이브러리가 아닌 외부 패키지와 모듈을 사용하는 경우 모두 pip를 통해 설치를 해야 함
- 복수의 프로젝트를 하는 경우 버전이 상이할 수 있음
    - 과거 외주 프로젝트: django 버전 2.x
    - 신규 회사 프로젝트: django 버전 3.x
- 이러한 경우 가상환경을 만들어 프로젝트별로 독립적인 패키지를 관리할 수 있음
- 가상환경을 만들고 관리하는 데 사용되는 모듈(Python 버전 3.5부터)
- 특정 디렉토리에 가상 환경을 만들고, 고유한 파이썬 패키지 집합을 가질 수 있음
    - 특정 폴더에 가상 환경(패키지 집합 폴더 등)이 있고
    - 실행 환경(예. bash)에서 가상 환경을 활성화시켜
    - 해당 폴더에 있는 패키지를 관리/사용함
- 가상 환경을 생성하면, 해당 디렉토리에 별도의 파이썬 패키지가 설치됨
    - $ python -m venv <폴더명>
- 가상환경 활성화/비활성화
    - 아래의 명령어를 통해 가상환경을 활성화
        - <venv>는 가상환경을 포함하는 디렉토리의 경로
        - 플랫폼 - 셀 - 가상환경을 활성화하는 명령
        - POSIX - bash/zshs - $ source <venv>/bin/activate
        - POSIX - fish - $ source <venv>/bin/activate.fish
        - POSIX - sch/tcsh - $ source <venv>/bin/activate.csh
        - POSIX - PowerShell Core - $ <venv>/bin/Activate.ps1
        - 윈도우 - bash - $ source <venv>/Scripts/activate
        - 윈도우 - cmd.exe - C:\> <venv>\Scripts\activate.bat
        - 윈도우 - PowerShell - PS C:\> <venv>\Scripts\Activate.ps1
    - 가상환경 비활성화는 $ deactivate 명령어를 사용
- 동일 컴퓨터에서 별도의 가상환경을 만들어서 프로젝트별로 사용할 수 있음
    - venv 폴더별로 고유한 프로젝트가 설치됨