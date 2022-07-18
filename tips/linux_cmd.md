# 리눅스 커맨드(터미널)
### Tips
- 터미널에서 깔끔하게 위로 올려 보기 단축키: `ctrl + l`
- ls 커맨드로 확인시 이름 뒤에 /가 붙어있다면 디렉토리라는 뜻(Windows)

### `-`옵션과 `--`옵션의 차이
1. “-”: UNIX
    - UNIX options, which may be grouped and must be preceded by a dash.
2. “--”: GNU
    - BSD options, which may be grouped and must not be used with a dash.
3. no dash: BSD
    - GNU long options, which are preceded by two dashes.

- 여기서 UNIX, GNU, BSD는 모두 LINUX 기반 OS 이름임<br>
- -옵션(축약형)은 --옵션(서술형)의 줄임말인 경우가 많다

### Commands in Alphabetical order
- `cd`: change directory, 이동
  - `cd`만 명령하면 홈 디렉토리로 이동한다. 
  - `cd ..`: 상위 경로로 이동
- `ls`: 현재 디렉토리에 속한 리스트 표시
  - `ls -a`: 숨김한 것까지 모두 표시(all)
- `mkdir`: make directory, 디렉토리 생성 
- `mv`: move, 이름 변경 또는 이동
- `pwd`: 내 현재 위치 경로표시
- `rm`: remove, 삭제
  - `rm -r`: 디렉토리를 삭제하려면 -r (recursive) 플래그를 붙여준다.
  - `rm -rf`: 강제 (force)
