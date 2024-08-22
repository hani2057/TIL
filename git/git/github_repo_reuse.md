# 깃허브 레포지토리 재사용하기

<aside>
💡 리액트로 투두앱 만들기를 반복해서 연습하면서, 연결해둔 깃허브 레포지토리를 매번 지우고 새로 만들고 싶지 않고 하나로 계속 재사용하고 싶었다.
결론: `git clean` 명령어를 이용하자.

</aside>

기존 로컬 레포에서 `git clean` 명령어를 이용하면 레포에 저장된 내용을 삭제할 수 있다.

- `git clean -f` : cleans untracked files
- `git clean -f -d` : cleans untracked files and also directories
- `git clean -f -X` : cleans ignored files
- `git clean -f -x` : cleans ignored and non-ignored files

`git clean` 으로 레포 안의 내용물을 다 지워주고 나면, 굳이 연결을 유지할 필요가 없으니 옵셔널로 `git remote remove {remote branch name}` 으로 연결을 해제할 수 있다. 또는 그냥 `.git/` 디렉토리를 지워도 됨.

그리고 새로운 로컬 레포에 리모트 레포를 연결한다. 이때 기존 레포에서 작업중이어서 커밋이 쌓여있다면 풀이 안 된다. 머지는 공통된 기존 커밋이 필요하기 때문에 안 되고, 리베이스는 커밋되지 않은 내용이 있다면 안 된다는 것까지 따라가다가 그냥 새 디렉토리를 파서 그쪽으로 연결한 다음 내용물을 옮겨줬다. json 파일만 옮기고 npm install 하면 되니까 그냥 그게 편함.