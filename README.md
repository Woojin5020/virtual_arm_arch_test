# 🛠️ STM32 개발 환경 구축 및 Renode 개요
## 1. Renode란 무엇인가?
### Renode는 실제 물리적인 하드웨어 없이 임베디드 시스템을 개발하고 테스트할 수 있는 오픈소스 에뮬레이터 프레임워크입니다.

- 주요 기능: 
프로세서(CPU), 주변장치(UART, GPIO, 타이머 등), 센서, 유선 네트워크 등을 소프트웨어적으로 모방합니다.

- 사용 목적:
  - STM32와 같은 ARM Cortex-M 계열 칩을 위한 펌웨어를 PC 상에서 바로 실행하고 디버깅할 수 있습니다.
  - 하드웨어가 준비되지 않은 상태에서도 소프트웨어 개발이 가능합니다.

  - CI/CD(지속적 통합) 파이프라인에서 자동화된 테스트를 수행할 수 있습니다.

## 2. 개발 환경 구축 (Windows + MSYS)
libopencm3 라이브러리를 빌드하고 Renode에서 실행할 펌웨어를 만들기 위해 다음과 같은 툴체인과 환경을 구성했습니다.

### ✅ 설치한 도구 (Prerequisites)
1. MSYS (1.0): Windows에서 GNU 빌드 환경(make, bash 등)을 사용하기 위한 셸.

2. ARM GNU Toolchain: ARM Cortex-M(Bare-metal)용 크로스 컴파일러 (arm-none-eabi-gcc).

3. Python: 빌드 스크립트 실행용.

4. Git: 소스 코드 버전 관리 및 다운로드용.

### 3. MSYS 셸 환경 변수(PATH) 설정
MSYS 셸이 Windows에 설치된 도구들(Python, GCC, Git)을 인식할 수 있도록 PATH를 설정했습니다.

#### 🚩 PATH 설정 명령어
Windows 경로를 MSYS 포맷(POSIX 스타일)으로 변환하여 export 합니다.
```
# Git(cmd), Python, ARM Toolchain 경로 등록
export PATH="/c/Program Files/Git/cmd:/c/Users/woojin/AppData/Local/Programs/Python/Python314:/c/Program Files (x86)/Arm GNU Toolchain arm-none-eabi/14.3 rel1/bin:/usr/local/bin:/usr/bin:/bin"
```
#### 🔍 설정 확인
설정이 제대로 되었는지 각 도구의 버전을 확인합니다.

```
echo $PATH                 # 경로 확인
arm-none-eabi-gcc --version # 컴파일러 확인
git --version              # Git 확인
```

### 4. 라이브러리(libopencm3) 빌드 과정
STM32 개발을 위한 오픈소스 라이브러리인 libopencm3를 다운로드하고 빌드했습니다.

#### 1) 소스 코드 다운로드 (Git Clone)
```
# 홈 디렉터리로 이동 후 클론
cd ~
git clone https://github.com/libopencm3/libopencm3.git
```

#### 2) Python 명령어 호환성 해결
libopencm3의 Makefile은 python3라는 명령어를 찾는데, Windows는 python.exe로 되어 있어 이를 연결해주는 작업이 필요했습니다.

```
# Python 설치 폴더로 이동 (경로는 사용자 환경에 맞게)
cd "/c/Users/woojin/AppData/Local/Programs/Python/Python314"

# 심볼릭 링크 생성 (python.exe -> python3.exe)
ln -s python.exe python3.exe
```

#### 3) 최종 빌드 (Make)
다시 라이브러리 폴더로 돌아와 빌드를 실행합니다.

```
cd ~/libopencm3
make
결과: 빌드가 완료되면 라이브러리 파일(.a)이 생성되며, 이제 펌웨어 예제 코드를 컴파일할 준비가 완료되었습니다.
```

# 참고 문헌
https://github.com/libopencm3/libopencm3
