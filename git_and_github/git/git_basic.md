# GIT

<aside>
๐ก git ์์ git์ด ์์ผ๋ฉด ์ ๋๋ค. 

git์ผ๋ก ๊ด๋ฆฌ๋๊ณ  ์๋ ๋๋ ํ ๋ฆฌ ์์์ git init ํ์ง ๋ง ๊ฒ

home directory์์ git init ํ์ง ๋ง ๊ฒ (์ฉ๋์ ํ, ๋๋ฌด ์ฉ๋ ํฌ๊ฒ ์ฌ์ฉํ์ง ๋ง ๊ฒ)
</aside>

### git ์ฌ์ฉํ๊ธฐ

- repository๋ก ์ฌ์ฉํ  ๋๋ ํ ๋ฆฌ๋ก ์ด๋ํ์ฌ `git init` ๋ช๋ น์ด ์๋ ฅ์ master branch๋ก ๋ฑ๋ก๋จ

### ๋ก์ปฌ ์ ์ฅ์(git)์์์ ์ํ

- working directory
- staging area
    - staging area๋ ์์ ์ฅ์น์ ์ญํ ๋ ํ์ง๋ง, ๋ฒ์ ๊ด๋ฆฌ์์คํ์ ๊ธฐ๋ฅ์ ์ถฉ์คํ  ์ ์๋๋ก ๋์์ฃผ๋ ์ค๊ฐ ์ฒ๋ฆฌ๊ตฌ๊ฐ ์ญํ ๋ ํ๋ค. (์ปค๋ฐ ํ๋๋ฅผ ์ผ๋ง์ ๋ณ๊ฒฝ ์ฌ์ด์ฆ๋ก ํด์ ์ปค๋ฐํ ๊ฑด์ง, ๋ฒ์  ์๋ฐ์ดํธ๋ฅผ ์ด๋ป๊ฒ ํ ๊ฑด์ง ๊ด๋ฆฌํ  ์ ์์)
- commits

### git commands

- git ๋ช๋ น์ด๋ git ํํ์ด์ง documents ํ์ฉ ๋๋ ๊ตฌ๊ธ๋ง *๋ฌธ์๊ฒ์๋ฅ๋ ฅ์ด ์ค์ํ๋ค!!
- `git config`
    - ์ต์ด 1ํ ์ด๊ธฐ์ค์ 
    - git config โglobal user.name {user_name}
    - git config โglobal user.email {user_email}
    - โglobal ํ๋๊ทธ ์์ด git config๋ฅผ ์์ฑ ๋๋ --local ํ๋๊ทธ๋ฅผ ์ฌ์ฉํ๋ฉด ํด๋น ๋ ํฌ์๋ง ์ ์ฉ๋จ
    - `git config --global -l` ๋๋ `git config -l` ๋ช๋ น์ ๋ฆฌ์คํธ ๋ณด์ฌ์ค(์ค์ ๋ ๋ด์ฉ๋ค)
- `git init`: git์ผ๋ก ๊ด๋ฆฌํ๋๋ก ์ด๊ธฐํ(initialize)
- `git status`: ํ์ํ ํ์ธ
- `git add {file_name}`: ํ์ผ์ staging area์ ์ถ๊ฐ
    - ์ด๋ ํด๋น ํ์ผ์ ๊น์ ์ถ๊ฐ๋์ด working directory๋ก ๋ค์ด๊ฐ๋ค.
    - `git add .` : ํ์ฌ ๋๋ ํ ๋ฆฌ์ ๋ชจ๋  ๋ณ๊ฒฝ์ฌํญ์ด ํ๊บผ๋ฒ์ staging area์ ์ฌ๋ผ๊ฐ
- `git commit -m โcommit massageโ`
    - ์ปค๋ฐ(๋ฒ์ ๊ธฐ๋ก) ๋จ๊ธฐ๊ธฐ
    - commit message๋ ํ์
        - ์ด๋ค ๋ณ๊ฒฝ์ฌํญ์ด ์์๋์ง๋ฅผ ๊ธฐ๋ก
    - commit์ด ๋ง๋ค์ด์ง๊ณ  staging area๊ฐ ๋น์์ง๋ค
    - *์ปค๋ฐ๋ฉ์ธ์ง ๋นผ๋จน๊ณ  git commit ์ํฐ ์ณค์ ๊ฒฝ์ฐ
        - VIM์ด ๋ฌ๋ค(์ด๊ธฐ์ ํ์คํธ์๋ํฐ - ๋ง์ฐ์ค ์ฌ์ฉ ๋ถ๊ฐ)
        - i ์๋ ฅ์ insert ๋ชจ๋๊ฐ ๋์ด(์ผ์ชฝ์๋์ ํ์๋จ) ์๋ ฅ ๊ฐ๋ฅ
        - ์ปค๋ฐ๋ฉ์ธ์ง ์๋ ฅ ํ escํค๋ฅผ ๋๋ฌ ์ธ์ํธ ๋ชจ๋ ๋๊ฐ ์ ์์
        - :wq (write and quit, ์ ์ฅํ๊ณ  ๋๊ฐ) ์๋ ฅ ํ ์ํฐ์น๋ฉด ์๋ฃ๋จ
    - *์ปค๋ฐ๋ฉ์์ง ์์ ํ๋ ๋ฐฉ๋ฒ
        - ์ปค๋ฐ๋ฉ์์ง๋ ์์ ํ  ์ผ์ด ๋ง์ด ๋ฐ์ํ๋ฉด ์ ๋๋ค. ํนํ pushํ๊ณ  ๋ ์ดํ์๋ ํ์ง ๋ง ๊ฒ!!
        - `git commit --ammend`
        - i ๋๋ฌ์ insert๋ชจ๋๋ก ๋ฐ๊พธ๊ณ  ๋ฐ๊ฟ ์ปค๋ฐ๋ฉ์์ง ์๋ ฅํ ํ esc-:wq ์๋ ฅํ์ฌ ์๋ฃ
- `git log` : commit ๊ธฐ๋ก์ ํ์ธ
    - git log ๋ช๋ น์ ํ์๋๋ ๋ด์ฉ๋ค
        
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
        
        - commit ๋ค์ ๋ถ๋ ๋ด์ฉ: commit id โ ๊ตฌ๋ถ์ ์ํ ๊ฒ. ์ฃผ๋ก ์์ 4๋ฌธ์ ์ ๋๋ฅผ ๋ฐ์ ์ํตํ๋ค.
        - HEAD โ : ์ด๋ค ๋ธ๋์น๋ฅผ ํ์ฌ ๋ฐ๋ผ๋ณด๊ณ (heading) ์๋์ง ๋ช์ํด์ค. ํด๋น ์์น์์์ ํด๋น ๋ธ๋์น์ ๋ก๊ทธ๋ฅผ ๋ณด์ฌ์ค๋ค.
        - commitํ ์์ฑ์, ๋ ์ง
        - commit message
    - `git log --oneline` : ์ฃผ์์ ๋ณด๋ง ํ ์ค๋ก ํ์ํด์ค
    - `git log -1`: ์ต๊ทผ ํ ๊ฐ๋ง ๋ณด์
    - git log ๋๊ฐ๋ ๋ฐฉ๋ฒ: q ๋๋ฅด๊ธฐ(VIM)
- `git restore` : ๋ณ๊ฒฝ์ฌํญ์ ๋ณ๊ฒฝ ์  ์ํ๋ก ๋ณต์ (to discard changes in working directory)
    - `git restore --staged` : git add ์ดํ์ staging area์์ working directory๋ก ๋ณต์

# GITHUB(์๊ฒฉ์ ์ฅ์ ์ฐ๊ฒฐ)

<aside>
๐ก ์์: ๋ก์ปฌ์์ git init(๊น ๋ ํฌ ๋ง๋ค๊ธฐ) โ github์์ create repository โ remote add ๋ก ์ฐ๊ฒฐ โ push โ ๋ณ๊ฒฝ์ฌํญ ๋ง๋ค๊ณ  commit โ push

</aside>

### ๋ก์ปฌ์์ Github ์ฐ๊ฒฐํ๊ธฐ

- `git remote` : ์๊ฒฉ์ ์ฅ์ ๊ด๋ จ ๋ช๋ น
    - git remote add origin {url}
        - github์์ ์์ฑํ repository์ url ์ ํ์ฌ(ํฐ๋ฏธ๋์ ํ์ฌ์์น) ๋๋ ํ ๋ฆฌ์ git๊ณผ ์ฐ๊ฒฐํจ. ์ด๋ github์ ํด๋น ๋ ํฌ์ ์ด๋ฆ์ origin์ผ๋ก ์ค์ ํจ.
            - origin์ด ๊ตณ์ด ์๋์ด๋ ๋์ง๋ง origin์ผ๋ก ํจ(์ผ์ข์ ์ปจ๋ฒค์)
    - git remote -v
- `git push` : ๋ก์ปฌ์ ์ปค๋ฐ์ ์๊ฒฉ์ผ๋ก ์๋ก๋
    - git push {remote์ด๋ฆ} {branch์ด๋ฆ}
    - git push origin master : origin์ด๋ผ๋ ์๊ฒฉ์ ์ฅ์์ master๋ผ๋ ๋ก์ปฌ๋ธ๋์น๋ฅผ pushํจ
    - git log ๋ช๋ น์ผ๋ก ํด๋น์์ ์์ ๋ก์ปฌ๊ณผ ์๊ฒฉ์ ์์น๋ฅผ ์ ์ ์๋ค. ์๋๋ โchange a.txtโ ์ปค๋ฐ์ ๋ก์ปฌ์์ ํ๊ณ  ํธ์ํ์ง ์์ ์ํฉ
        
        ```jsx
        $ git log --oneline
        8226321 (HEAD -> main) change a.txt
        5d544a5 (origin/main) add b.txt
        ce0864d delete b.txt
        51e7e0c change a.txt again, add b.txt
        c4aeff8 change a.txt
        a4151f5 add textfile
        ```
        
    - ํธ์ํ๋ฉด ์๋์ ๊ฐ์ด ๋๋ค.
        
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
        - `-u` ๋ `--set-upstream` ์ ์ถ์ฝ
        - ํด๋น ๋ช๋ น ์ดํ๋ก๋ pushํ  ๋๋ง๋ค origin master๋ฅผ ์ ์ง ์๊ณ  git push ๋ง ๋ช๋ นํด๋ ์๋์ผ๋ก {remote์ด๋ฆ} {๋ก์ปฌ์ด๋ฆ}์ด origin master ๋ก ๋ฐ๋ผ๊ฐ๋ค.
- `.gitignore`
    - git์ผ๋ก ๊ด๋ฆฌ๋๋ ํ์ผ์ ์ดํ .gitignore์ ์ถ๊ฐํ์ฌ๋ ์ ์ฉ๋์ง ์์ผ๋ฏ๋ก ์ปค๋ฐํ๊ธฐ ์  ํ๋ก์ ํธ ์์์ ๊ณง๋ฐ๋ก .gitignore ๋จผ์  ๋ง๋ค์ด์ฃผ๋ ๊ฒ ์ข๋ค.
    - ๐[gitignore.io](https://www.toptal.com/developers/gitignore/) ์ ์ํด์ Python, VisualStudioCode, Windows, macOS ๋ฑ ์๋ ฅ ํ ์์ฑ ํด๋ฆญ์ .gitignore ํ์ผ์ ๋ค์ด๊ฐ ๋ด์ฉ์ ์์ฑํด์ค๋ค.
- master ๋ธ๋์น ๋ง๊ณ  main ๋ธ๋์น๋ฅผ ์ฐ๋ ์ด์ ์ ๋ฐฉ๋ฒ์ ์๋ ๋ฉ๋ชจ ์ฐธ๊ณ 

### Github์์ ๋ก์ปฌ๋ก ์ฐ๊ฒฐํ๊ธฐ

- `git clone`: ์๊ฒฉ์ ์ฅ์๋ฅผ ๋ก์ปฌ๋ก ๋ณต์ ํ๋ค. (๋ก์ปฌ์์ ์ต์ด git init ๊ณผ ๊ฐ์ ๋๋)
    - git clone {url}
        - github repository์์ code โ https url ์ฃผ์ ๋ณต์ฌ
        - `git clone {url} .` ๋๋ `git clone {url} {๋๋ ํ ๋ฆฌ์ด๋ฆ}` ์ผ๋ก ์ฌ์ฉ๊ฐ๋ฅ
- `git pull`
    - ์๊ฒฉ์ ๋ก์ปฌ๋ก ๊ฐ์ ธ์ด(๋ค์ด๋ก๋ ๊ฐ๋)
    - ์ถฉ๋์ ๋ฐฉ์งํ๊ธฐ ์ํด pull ํ ์ดํ push ํ์

# CONFLICT

### ์๋ก ๋ค๋ฅธ ํ์ผ์ ๋ณ๊ฒฝํ ๊ฒฝ์ฐ(3-way merge)

<aside>
๐ก A(a, b, d) โ Github(a, b) โ B(a, b, c) 
A(a, b, d) โ Github(a, b, c) โpushโ B(a, b, c)
A(a, b, d) โ ***conflict!!*** โ Github(a, b, c) โ B(a, b, c)

</aside>

- A๊ฐ pushํ๊ธฐ ์  git log๋ ์๋์ ๊ฐ๋ค.

```jsx
$ git log --oneline
522bdd4 (HEAD -> main) add d.txt
d4fc605 (origin/main, origin/HEAD) add a, b
```

- A๊ฐ pushํ๋ ค๊ณ  ํ๋ฉด(d ์ถ๊ฐ) ์๋์ ๊ฐ์ด remote๊ฐ ๋ ์ต์ ์ด๋ผ๋ ์๋ฌ๊ฐ ๋ฌ๋ค.

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

- conflict๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด git pull ํ๋ค.

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

- ๊ทธ๋ฌ๋ฉด ํ์ผ ๋ด์ฉ์ผ๋ก ๊ฒน์น๋ ๋ด์ฉ์ด ์์ผ๋ฏ๋ก merge ํ๋ผ๊ณ  ๋ฌ๋ค. VIM ์ด๋ฏ๋ก :wq ์๋ ฅํ์ฌ ์๋ฃํ๋ค.
- ๊ทธ๋ฌ๋ฉด ์๋์ผ๋ก merge commit ์ด ํ๋ ์๊ธด๋ค. ๊ทธ ์ํ์์ git log ์ฐ์ด๋ณด๋ฉด ์๋์ ๊ฐ๋ค.

```jsx
$ git log --oneline
0927de9 (HEAD -> main) Merge branch 'main' of [https://github.com/hani2057/git-conflict](https://github.com/hani2057/git-conflict)
522bdd4 add d.txt
53b4c39 (origin/main, origin/HEAD) add c.txt
d4fc605 add a, b
```

- ๊ทธ๋ํ๋ก ๋ณด๋ฉด ์๋์ ๊ฐ๋ค. `git log --oneline --graph`

```jsx
$ git log --oneline --graph
*   0927de9 (HEAD -> main, origin/main, origin/HEAD) Merge branch 'main' of https://github.com/hani2057/git-conflict
|\
| * 53b4c39 add c.txt
* | 522bdd4 add d.txt
|/
* d4fc605 add a, b
```

- conflict ํด๊ฒฐ ํ git push ํด์ ๋ง๋ฌด๋ฆฌํ๋ค.

### ๋์ผํ ํ์ผ์ ๋ณ๊ฒฝํ ๊ฒฝ์ฐ

<aside>
๐ก A(a*, b) โ Github(a, b) โ B(a-, b) 
A(a*, b) โ Github(a-, b) โpushโ B(a-, b)
A(a*, b) โ ***conflict!!*** โ Github(a-, b) โ B(a, b)

</aside>

- A๊ฐ pushํ๊ธฐ ์  git log๋ ์๋์ ๊ฐ๋ค.

```jsx
$ git log --oneline
1e62828 (HEAD -> main) change a.txt from left
0927de9 (origin/main, origin/HEAD) Merge branch 'main' of https://github.com/hani2057/git-conflict
522bdd4 add d.txt
53b4c39 add c.txt
d4fc605 add a, b
```

- A๊ฐ pushํ๋ ค๊ณ  ํ๋ฉด(a* ์ถ๊ฐ) ์๋์ ๊ฐ์ด remote๊ฐ ๋ ์ต์ ์ด๋ผ๋ ์๋ฌ๊ฐ ๋ฌ๋ค.

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

- conflict๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด git pull ํ๋ค. ๊ทธ๋ฌ๋ฉด ์๋์ ๊ฐ์ด a.txt์์ ์๋ ๋จธ์ง๊ฐ ์คํจํ๋ค๋ ๋ด์ฉ์ด ๋ฌ๋ค. ์ง์  ํด๊ฒฐํด์ผ ํ๋ค.

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

- ๋ฌธ์ ๊ฐ ๋ฐ์ํ ํ์ผ์ `code` ๋ช๋ น์ด๋ฅผ ํตํด vscode์์ ์ด๋ฉด, ์ปจํ๋ฆญํธ ํด๊ฒฐ์ด ํ์ํ ํ์ผ์ ! ํ์๊ฐ ๋จ๊ณ , ๋ด์ฉ์์๋ ๋ฌธ์ ๊ฐ ๋ ๋ด์ฉ์ด ํ์๋๊ณ  ๊ทธ ์์ Accept Current Change | Accept Incoming Change | Accept Both Changes | Compare Changes ๋ผ๋ ์ต์์ ์ ํํ  ์ ์๋ค.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e950cc3-764e-4741-a8f6-6add83d77fd9/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33f0a276-8530-4e0a-b2f8-2a282c82597f/Untitled.png)

- ์๋ฃ๋๋ฉด ์ ์ฅํ๊ณ , add, commit, push ํ๋ค.
    - ์ด๋ commit message๋ฅผ โSolve conflict in a.txtโ ๋ฑ์ผ๋ก ์ ์ด์ฃผ๋ฉด ์ข์
- ์ดํ git log ์ฐ์ด๋ณด๋ฉด ์๋์ ๊ฐ๋ค.

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

- git log โoneline โgraph ๋ก ๋ณด๋ฉด ์๋์ ๊ฐ๋ค.

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

# ๋ฉ๋ชจ

- VIM
- url์ ๋์ด์ฐ๊ธฐ๋ฅผ `_`๊ฐ ์๋ `-`๋ก ์ฃผ๋ก ์ฌ์ฉํ๋ค. (์ปจ๋ฒค์)
    - url๋ก ์ฌ์ฉ์ ๊ธฐ๋ณธ ํ์ดํผํ์คํธ์ ๋ฐ์ค์ด ๋ค์ด๊ฐ ํท๊ฐ๋ฆด ์ ์์
- git master ๋ธ๋์น๋ช main์ผ๋ก ๋ณ๊ฒฝํ๋ ๋ฐฉ๋ฒ [https://kotlinworld.com/285](https://kotlinworld.com/285)
    - Black Lives Matter - master๊ฐ slave๋ฅผ ์ฐ์์ํจ๋ค๋ ๊ฒ๊ณผ ๊ด๋ จํ์ฌ master ๋์  main ์ฌ์ฉํ๊ธฐ ์ด๋์ด ์์์
    - **default branch name์ master๊ฐ ์๋ main์ผ๋ก ์ค์ ํ๊ธฐ**
        - `git config โglobal init.defaultBranch main`
    - `git branch -M main` : master ๋ธ๋์น์์ ํด๋น ๋ช๋ น ์ฌ์ฉ์ ์ด๋ฆ ๋ฐ๋
    - ํ๋ ์ด์์ ์ปค๋ฐ ๋ง๋ค๊ธฐ
    - ๋ณ๊ฒฝ์ฌํญ์ github main ๋ธ๋์น์ ํธ์ํ๊ณ  ๋ฐ๋ผ๊ฐ๋๋ก ๋ง๋ค๊ธฐ ์ํด git push -u ์ต์์ ์ฌ์ฉํ๋ค. `git push -u origin main`
- ๊ตฌ๊ธ ์ด๋ฏธ์ง๊ฒ์ โgit flowโ โ ์์๋ฐฉ์ ์ดํด
- bash ๋ณต์ฌํ๊ธฐ(ctrl + Insert) ๋ถ์ฌ๋ฃ๊ธฐ(shift + Insert)