## file unstage & unstrack

- `git restore {file name}`
  - 파일을 마지막 커밋 상태로 되돌림
  - 파일 이름에는 `.` 이 들어올 수 있음
  - 특정 커밋(a라고 하자) 이후 작업을 수정한 이후, 직전 커밋 상태로 되돌릴 수 있다. 작업내용이 날아간다.
- `git restore --staged {file name}`
  - 파일을 staging area에서 unstaging함
  - 파일 이름에는 `.` 이 들어올 수 있음
  - `git add {file name}` 이후에 사용
  - a라는 파일과 b라는 파일을 `git add .` 명령을 통해 모두 staging area에 올렸을 경우 `git restore --staged b` 명령을 통해 b파일만 unstaging할 수 있다.
  - restore 명령어는 직전 커밋을 기준으로 하기 때문에 커밋이 최소 1개 필요하다.
- `git rm --cached {file name}`
  - 파일을 git이 관리하지 않도록 한다.(untrack)
  - .gitignore 에 추가하는 걸 빼먹었을 때 유용하게 사용할 수 있다.
    - 참고로 .gitignore에 들어간 내용은 git이 트래킹을 아예 하지 않기 때문에 `git status` 명령을 통해 확인했을 때 untracked files에도 뜨지 않는다.
  - 그냥 `git rm --cached {file name}` 명령을 하고 .gitignore에 추가하지 않으면 다시 tracking할 수 있다. (인식은 하는 상태로 untracked files에 뜸)
  - —cached 플래그 없이 사용하면 파일도 삭제되니 주의

## editing commit

- `git commit --amend`
  - 마지막 커밋을 수정할 수 있다.
  - 커밋한 이후, 작업을 추가했으나 커밋을 새로 만드는 게 아니라 직전 커밋에 묶어 하나의 커밋으로 남기고 싶을 때 `git add .` 이후에 `git commit -m {commit message}` 대신 `git commit --amend` 를 사용하여 직전 커밋의 수정페이지(VIM)에서 :wq 로 저장 종료하면 직전 커밋에 현재까지 작업 내용이 함께 저장된다.
  - 커밋 메시지를 수정하려면 i 키를 눌러 insert mode로 전환한 후 메시지를 수정하고 esc키를 누르고 :wq 로 종료한다.
    - 추가 작업 내용을 기존 메시지에 (some work added)처럼 붙여주면 좋을듯!
    - :q 를 통해 저장하지 않고 나갈 수 있고, 수정한 이후 저장하지 않고 나가려고 하면 에러가 뜨는데 이때 :q! 를 통해 강제종료할 수 있다.

## canceling commit

- `git reset`

  - 커밋을 (HEAD를) 과거로 이동
  - 협업 환경 또는 remote 환경에서는 가급적 `reset` 을 사용하지 않는 것을 추천 (대신 `revert` 사용)
  - 파일의 상태를 옵션으로 선택할 수 있다.
  - `git reset --hard {commit id}`
    - id에 해당하는 해당 커밋으로 이동
    - working directory도 과거로 이동(즉 파일도 없어짐)
    - staging area는 비어있는 상태
    - 즉 되돌아간 커밋 이후의 파일들을 모두 워킹 디렉토리에서 삭제 (기존의 언트랙드 파일은 사라지지 않고 언트랙드로 남아있음)
  - `git reset --soft {commit id}`
    - id에 해당하는 해당 커밋으로 이동
    - working directory는 최신 상태(즉 파일이 유지됨)
    - staging area에는 작업 내용이 모두 올라가 있다.
    - 즉 되돌아간 커밋 이후의 파일들을 스테이징 에리어로 돌려놓음
  - `git reset {commit id}`
    - 또는 `git reset --mixed {commit id}` (디폴트값)
    - id에 해당하는 해당 커밋으로 이동
    - working directory는 최신 상태(즉 파일이 유지됨)
    - staging area는 비어있는 상태
    - 즉 되돌아간 커밋 이후의 파일들을 워킹 디렉토리로 돌려놓음

- `git reflog`

  - 커밋 변경 내용을 포함한 기록을 조회 가능
  - remote에 푸시해둔 상태라면 github 등에서 조회해도 된다

- `git revert {commit id}`
  - id에 해당하는 커밋 작업 내용을 삭제하고 (커밋 취소) 해당 내역에 대한 커밋을 새로 남긴다.
  - 협업 또는 remote 환경에서 사용할 수 있다.
  - revert는 conflict 등만 문제가 없다면 가장 마지막 커밋뿐 아니라 그 이전 커밋에 대해서도 revert(즉 취소)할 수 있다.
  - 참고로 revert를 revert하면 작업 내용이 원복된다.
  - `git revert {commit id}..{commit id}` 를 이용해서 특정 커밋부터 특정 커밋까지를 한번에 revert할 수 있다. (포함여부 주의)

## git branch

### git branch strategy

- git-flow
  - 5개의 브랜치로 나누어 소스코드를 관리
    - master: 제품으로 출시될 수 있는 브랜치
    - develop: 다음 출시 버전을 개발하는 브랜치
    - feature: 기능을 개발하는 브랜치
    - release: 이번 출시 버전을 준비하는 브랜치
    - hotfix: 출시 버전에서 발생한 버그를 수정하는 브랜치
- github-flow

### git branch

- `git branch`
  - 브랜치 목록 조회
  - `git branch -r`
    - 원격 저장소의 브랜치 목록 조회
- `git branch {branch name}`
  - 새로운 브랜치 생성
  - `git branch {branch name} {commit id}`
    - 특정 커밋 기준으로 브랜치 생성
- `git switch {branch name}`
  - 해당 브랜치로 이동
  - `git switch -c {branch name}`
    - 브랜치를 새로 생성 및 이동
- `git merge {branch name}`
  - 현재 있는 브랜치에서 {branch name} 브랜치를 병합
  - 수명을 다한 브랜치는 지워주는 게 좋다. (feature 등)
- `git branch -d {branch name}`
  - 해당 브랜치를 삭제(병합된 브랜치만 삭제 가능)
  - `git branch -D {branch name}`
    - 강제 삭제

## git workflow

- 원격저장소 소유권이 있는 경우 → shared repository model
  - 원격저장소가 자신의 소유이거나 collaborator로 등록되어 있는 경우
  - main 브랜치에 직접 개발하는 것이 아니라 기능별로 브랜치를 따로 만들어 개발
  - pull request를 사용하여 팀원간 변경 내용에 대한 소통 진행
- 원격저장소 소유권이 없는 경우 → fork & pull model

참고

- `git stash`
  - 임시 저장
  - `git stach pop`
