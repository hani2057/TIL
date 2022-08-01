# GIT

<aside>
💡 git 안에 git이 있으면 안 된다. 

git으로 관리되고 있는 디렉토리 안에서 git init 하지 말 것

home directory에서 git init 하지 말 것 (용량제한, 너무 용량 크게 사용하지 말 것)
</aside>

### git 사용하기

- repository로 사용할 디렉토리로 이동하여 `git init` 명령어 입력시 master branch로 등록됨

### 로컬 저장소(git)에서의 상태

- working directory
- staging area
    - staging area는 안전장치의 역할도 하지만, 버전관리시스템의 기능에 충실할 수 있도록 도와주는 중간 처리구간 역할도 한다. (커밋 하나를 얼마의 변경 사이즈로 해서 커밋할건지, 버전 업데이트를 어떻게 할건지 관리할 수 있음)
- commits

### git commands

- git 명령어는 git 홈페이지 documents 활용 또는 구글링 *문서검색능력이 중요하다!!
- `git config`
    - 최초 1회 초기설정
    - git config —global user.name {user_name}
    - git config —global user.email {user_email}
    - —global 플래그 없이 git config를 작성 또는 --local 플래그를 사용하면 해당 레포에만 적용됨
    - `git config --global -l` 또는 `git config -l` 명령시 리스트 보여줌(설정된 내용들)
- `git init`: git으로 관리하도록 초기화(initialize)
- `git status`: 현상태 확인
- `git add {file_name}`: 파일을 staging area에 추가
    - 이때 해당 파일은 깃에 추가되어 working directory로 들어간다.
    - `git add .` : 현재 디렉토리의 모든 변경사항이 한꺼번에 staging area에 올라감
- `git commit -m ‘commit massage’`
    - 커밋(버전기록) 남기기
    - commit message는 필수
        - 어떤 변경사항이 있었는지를 기록
    - commit이 만들어지고 staging area가 비워진다
    - *커밋메세지 빼먹고 git commit 엔터 쳤을 경우
        - VIM이 뜬다(초기의 텍스트에디터 - 마우스 사용 불가)
        - i 입력시 insert 모드가 되어(왼쪽아래에 표시됨) 입력 가능
        - 커밋메세지 입력 후 esc키를 눌러 인서트 모드 나갈 수 있음
        - :wq (write and quit, 저장하고 나감) 입력 후 엔터치면 완료됨
    - *커밋메시지 수정하는 방법
        - 커밋메시지는 수정할 일이 많이 발생하면 안 된다. 특히 push하고 난 이후에는 하지 말 것!!
        - `git commit --ammend`
        - i 눌러서 insert모드로 바꾸고 바꿀 커밋메시지 입력한 후 esc-:wq 입력하여 완료
- `git log` : commit 기록을 확인
    - git log 명령시 표시되는 내용들
        
        ```jsx
        $ git log
        commit c4aeff83e8a88a9e4415ad0f484cfc884d31269e (HEAD -> master)
        Author: hani2057 <hanikim2057@gmail.com>
        Date:   Fri Jul 15 12:57:48 2022 +0900
        
            change a.txt
        
        commit a4151f5c60287113c090ddd0e48e8fc90430e991
        Author: hani2057 <hanikim2057@gmail.com>
        Date:   Fri Jul 15 12:43:20 2022 +0900
        
            add textfile
        ```
        
        - commit 뒤에 붙는 내용: commit id → 구분을 위한 것. 주로 앞의 4문자 정도를 따서 소통한다.
        - HEAD → : 어떤 브랜치를 현재 바라보고(heading) 있는지 명시해줌. 해당 위치에서의 해당 브랜치의 로그를 보여준다.
        - commit한 작성자, 날짜
        - commit message
    - `git log --oneline` : 주요정보만 한 줄로 표시해줌
    - `git log -1`: 최근 한 개만 보임
    - git log 나가는 방법: q 누르기(VIM)
- `git restore` : 변경사항을 변경 전 상태로 복원 (to discard changes in working directory)
    - `git restore --staged` : git add 이후에 staging area에서 working directory로 복원

# GITHUB(원격저장소 연결)

<aside>
💡 순서: 로컬에서 git init(깃 레포 만들기) → github에서 create repository → remote add 로 연결 → push → 변경사항 만들고 commit → push

</aside>

### 로컬에서 Github 연결하기

- `git remote` : 원격저장소 관련 명령
    - git remote add origin {url}
        - github에서 생성한 repository의 url 을 현재(터미널의 현재위치) 디렉토리의 git과 연결함. 이때 github의 해당 레포의 이름을 origin으로 설정함.
            - origin이 굳이 아니어도 되지만 origin으로 함(일종의 컨벤션)
    - git remote -v
- `git push` : 로컬의 커밋을 원격으로 업로드
    - git push {remote이름} {branch이름}
    - git push origin master : origin이라는 원격저장소에 master라는 로컬브랜치를 push함
    - git log 명령으로 해당시점에서 로컬과 원격의 위치를 알 수 있다. 아래는 ‘change a.txt’ 커밋을 로컬에서 하고 푸시하지 않은 상황
        
        ```jsx
        $ git log --oneline
        8226321 (HEAD -> main) change a.txt
        5d544a5 (origin/main) add b.txt
        ce0864d delete b.txt
        51e7e0c change a.txt again, add b.txt
        c4aeff8 change a.txt
        a4151f5 add textfile
        ```
        
    - 푸시하면 아래와 같이 된다.
        
        ```jsx
        $ git log --oneline
        8226321 (HEAD -> main, origin/main) change a.txt
        5d544a5 add b.txt
        ce0864d delete b.txt
        51e7e0c change a.txt again, add b.txt
        c4aeff8 change a.txt
        a4151f5 add textfile
        ```
        
    - `git push -u origin master`
        - `-u` 는 `--set-upstream` 의 축약
        - 해당 명령 이후로는 push할 때마다 origin master를 적지 않고 git push 만 명령해도 자동으로 {remote이름} {로컬이름}이 origin master 로 따라간다.
- `.gitignore`
    - git으로 관리되는 파일은 이후 .gitignore에 추가하여도 적용되지 않으므로 커밋하기 전 프로젝트 시작시 곧바로 .gitignore 먼저 만들어주는 게 좋다.
    - 🔗[gitignore.io](https://www.toptal.com/developers/gitignore/) 접속해서 Python, VisualStudioCode, Windows, macOS 등 입력 후 생성 클릭시 .gitignore 파일에 들어갈 내용을 작성해준다.
- master 브랜치 말고 main 브랜치를 쓰는 이유와 방법은 아래 메모 참고

### Github에서 로컬로 연결하기

- `git clone`: 원격저장소를 로컬로 복제한다. (로컬에서 최초 git init 과 같은 느낌)
    - git clone {url}
        - github repository에서 code → https url 주소 복사
        - `git clone {url} .` 또는 `git clone {url} {디렉토리이름}` 으로 사용가능
- `git pull`
    - 원격을 로컬로 가져옴(다운로드 개념)
    - 충돌을 방지하기 위해 pull 한 이후 push 하자

# CONFLICT

### 서로 다른 파일을 변경한 경우(3-way merge)

<aside>
💡 A(a, b, d) — Github(a, b) — B(a, b, c) 
A(a, b, d) — Github(a, b, c) ←push← B(a, b, c)
A(a, b, d) → ***conflict!!*** → Github(a, b, c) — B(a, b, c)

</aside>

- A가 push하기 전 git log는 아래와 같다.

```jsx
$ git log --oneline
522bdd4 (HEAD -> main) add d.txt
d4fc605 (origin/main, origin/HEAD) add a, b
```

- A가 push하려고 하면(d 추가) 아래와 같이 remote가 더 최신이라는 에러가 뜬다.

```jsx
$ git push -u origin main
To [https://github.com/hani2057/git-conflict.git](https://github.com/hani2057/git-conflict.git)
! [rejected]        main -> main (fetch first)
error: failed to push some refs to '[https://github.com/hani2057/git-conflict.git](https://github.com/hani2057/git-conflict.git)'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

- conflict를 해결하기 위해 git pull 한다.

```jsx
$ git pull
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 2 (delta 0), reused 2 (delta 0), pack-reused 0
Unpacking objects: 100% (2/2), 214 bytes | 14.00 KiB/s, done.
From [https://github.com/hani2057/git-conflict](https://github.com/hani2057/git-conflict)
d4fc605..53b4c39  main       -> origin/main
Merge made by the 'ort' strategy.
c.txt | 0
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 c.txt
```

- 그러면 파일 내용으로 겹치는 내용이 없으므로 merge 하라고 뜬다. VIM 이므로 :wq 입력하여 완료한다.
- 그러면 자동으로 merge commit 이 하나 생긴다. 그 상태에서 git log 찍어보면 아래와 같다.

```jsx
$ git log --oneline
0927de9 (HEAD -> main) Merge branch 'main' of [https://github.com/hani2057/git-conflict](https://github.com/hani2057/git-conflict)
522bdd4 add d.txt
53b4c39 (origin/main, origin/HEAD) add c.txt
d4fc605 add a, b
```

- 그래프로 보면 아래와 같다. `git log --oneline --graph`

```jsx
$ git log --oneline --graph
*   0927de9 (HEAD -> main, origin/main, origin/HEAD) Merge branch 'main' of https://github.com/hani2057/git-conflict
|\
| * 53b4c39 add c.txt
* | 522bdd4 add d.txt
|/
* d4fc605 add a, b
```

- conflict 해결 후 git push 해서 마무리한다.

### 동일한 파일을 변경한 경우

<aside>
💡 A(a*, b) — Github(a, b) — B(a-, b) 
A(a*, b) — Github(a-, b) ←push← B(a-, b)
A(a*, b) → ***conflict!!*** → Github(a-, b) — B(a, b)

</aside>

- A가 push하기 전 git log는 아래와 같다.

```jsx
$ git log --oneline
1e62828 (HEAD -> main) change a.txt from left
0927de9 (origin/main, origin/HEAD) Merge branch 'main' of https://github.com/hani2057/git-conflict
522bdd4 add d.txt
53b4c39 add c.txt
d4fc605 add a, b
```

- A가 push하려고 하면(a* 추가) 아래와 같이 remote가 더 최신이라는 에러가 뜬다.

```jsx
$ git push
To https://github.com/hani2057/git-conflict.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/hani2057/git-conflict.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

- conflict를 해결하기 위해 git pull 한다. 그러면 아래와 같이 a.txt에서 자동 머지가 실패했다는 내용이 뜬다. 직접 해결해야 한다.

```jsx
$ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 276 bytes | 15.00 KiB/s, done.
From https://github.com/hani2057/git-conflict
   0927de9..1106cbd  main       -> origin/main
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
Automatic merge failed; fix conflicts and then commit the result.
```

- 문제가 발생한 파일을 `code` 명령어를 통해 vscode에서 열면, 컨플릭트 해결이 필요한 파일에 ! 표시가 뜨고, 내용에서는 문제가 된 내용이 표시되고 그 위에 Accept Current Change | Accept Incoming Change | Accept Both Changes | Compare Changes 라는 옵션을 선택할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e950cc3-764e-4741-a8f6-6add83d77fd9/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33f0a276-8530-4e0a-b2f8-2a282c82597f/Untitled.png)

- 완료되면 저장하고, add, commit, push 한다.
    - 이때 commit message를 ‘Solve conflict in a.txt’ 등으로 적어주면 좋음
- 이후 git log 찍어보면 아래와 같다.

```jsx
$ git log --oneline
6411e73 (HEAD -> main) solve conflict
1e62828 change a.txt from left
1106cbd (origin/main, origin/HEAD) change a.txt from right
0927de9 Merge branch 'main' of https://github.com/hani2057/git-conflict
522bdd4 add d.txt
53b4c39 add c.txt
d4fc605 add a, b
```

- git log —oneline —graph 로 보면 아래와 같다.

```jsx
$ git log --oneline --graph
*   6411e73 (HEAD -> main) solve conflict
|\
| * 1106cbd (origin/main, origin/HEAD) change a.txt from right
* | 1e62828 change a.txt from left
|/
*   0927de9 Merge branch 'main' of https://github.com/hani2057/git-conflict
|\
| * 53b4c39 add c.txt
* | 522bdd4 add d.txt
|/
* d4fc605 add a, b
```

---

# 메모

- VIM
- url은 띄어쓰기를 `_`가 아닌 `-`로 주로 사용한다. (컨벤션)
    - url로 사용시 기본 하이퍼텍스트에 밑줄이 들어감 헷갈릴 수 있음
- git master 브랜치명 main으로 변경하는 방법 [https://kotlinworld.com/285](https://kotlinworld.com/285)
    - Black Lives Matter - master가 slave를 연상시킨다는 것과 관련하여 master 대신 main 사용하기 운동이 있었음
    - **default branch name을 master가 아닌 main으로 설정하기**
        - `git config —global init.defaultBranch main`
    - `git branch -M main` : master 브랜치에서 해당 명령 사용시 이름 바뀜
    - 하나 이상의 커밋 만들기
    - 변경사항을 github main 브랜치에 푸시하고 따라가도록 만들기 위해 git push -u 옵션을 사용한다. `git push -u origin main`
- 구글 이미지검색 ‘git flow’ ← 작업방식 이해
- bash 복사하기(ctrl + Insert) 붙여넣기(shift + Insert)

<br>

# Git - Branch & 공유 repository

- git branch {branch name} : 브랜치 생성
- git branch : 브랜치 목록 확인
- git switch {branch name} : {branch name} 브랜치로 이동
    - git checkout : switch + restore
    - 맥의 경우 구버전 git이 기본적으로 깔려 있기 때문에 switch를 쓰려면 git 업데이트해야 함
- git merge {branch name} : {branch name} 브랜치를 현재 위치한 브랜치로 머지함
    
    ```python
    $ git log --oneline --graph
    *   a12357d (HEAD -> main) Merge branch 'test'
    |\
    | * 3c9615d (test) 3rd commit
    * | 07d2d2f 2nd commit
    |/
    * ce5b3ca 1st commit
    ```
    
- git log —oneline -a —graph
- git push origin {branch name}
    - 해당하는 branch로 push함
    - PR(pull request)가 생성됨
    - confirm하면 master로 merge됨