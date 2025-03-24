# git-usage

자주 사용하는 깃 명령어 모음

## 구조

코드는 아래 세 단계에 걸쳐 저장된다.

스테이징 -> 커밋 -> 원격저장소

1. git add {파일명} 으로 파일을 스테이징 상태에 넣는다.
2. git commit 으로 스테이징 상태에 있는 모든 변경사항을 커밋한다. 여기까지가 로컬에서의 작업
3. git push 로 커밋된 저장소를 원격 저장소로 밀어넣는다.

## 기본 명령어

저장소 생성

```
git init
```

원격 저장소로부터 복제

```
git clone {url}
```

변경 사항 체크

```
git status //
```

특정 파일 스테이징

```
git add {파일명}
```

변경된 모든 파일 스테이징

```
git add *
```

커밋

```
git commit -m “{변경 내용}”
```

원격으로 보내기

```
git push origin master
```

원격저장소 추가

```
git remote add origin {원격서버주소}
```

참고 페이지

- download(osx): http://code.google.com/p/git-osx-installer/downloads/list
- download(windows): http://git-scm.com/download/win
- 설치 메뉴얼: http://blog.outsider.ne.kr/389
- 사용 메뉴얼:http://dogfeet.github.io/articles/2012/how-to-github.html
- git 간편 안내서: http://rogerdudler.github.com/git-guide/index.ko.html
- 한장으로 핵심 기능만: http://rogerdudler.github.com/git-guide/files/git_cheat_sheet.pdf

## Add, Commit, Push 취소

# add 취소

```

// 모든 파일이 Staged 상태로 바뀐다.
$ git add *

// 파일들의 상태를 확인한다.
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
  renamed:    README.md -> README
  modified:   CONTRIBUTING.md

// CONTRIBUTING.md 파일을 Unstage로 변경한다.
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md

// 파일들의 상태를 확인한다.
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
  renamed:    README.md -> README
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
  modified:   CONTRIBUTING.md

```

# commit 취소

```
// commit 목록 확인
$ git log

// [방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
$ git reset --soft HEAD^

// [방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
$ git reset --mixed HEAD^ // 기본 옵션
$ git reset HEAD^ // 위와 동일
$ git reset HEAD~2 // 마지막 2개의 commit을 취소

// [방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
$ git reset --hard HEAD^

```

# commit message 변경하기

```

commit message를 잘못 적은 경우, git commit –amend 명령어를 통해 git commit message를 변경할 수 있다.

$ git commit --amend

```

# TIP git reset 명령은 아래의 옵션과 관련해서 주의하여 사용해야 한다.

```
-- reset 옵션

–soft : index 보존(add한 상태, staged 상태), 워킹 디렉터리의 파일 보존. 즉 모두 보존.
–mixed : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 보존 (기본 옵션)
–hard : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 삭제. 즉 모두 취소.


# TIP - 만약 워킹 디렉터리를 원격 저장소의 마지막 commit 상태로 되돌리고 싶으면, 아래의 명령어를 사용한다.
단, 이 명령을 사용하면 원격 저장소에 있는 마지막 commit 이후의 워킹 디렉터리와 add했던 파일들이 모두 사라지므로 주의해야 한다.

// 워킹 디렉터리를 원격 저장소의 마지막 commit 상태로 되돌린다.
$ git reset --hard HEAD

```

# push 취소

```
1. 워킹 디렉토리에서 commit 되돌린다.

// 가장 최근의 commit을 취소 (기본 옵션: --mixed)
$ git reset HEAD^

// Reflog(브랜치와 HEAD가 지난 몇 달 동안에 가리켰었던 커밋) 목록 확인
$ git reflog 또는 $ git log -g

// 원하는 시점으로 워킹 디렉터리를 되돌린다.
$ git reset HEAD@{number} 또는 $ git reset [commit id]


2. 되돌린 상태에서 다시 commit 한다.

$ git commit -m "Write commit messages"


3. 원격 저장소에 강제로 push 한다.

$ git push origin [branch name] -f

또는

$ git push origin +[branch name]

// Ex) master branch를 원격 저장소(origin)에 강제로 push
$ git push origin +master


# TIP 경고를 무시하고 강제로 push 하기

[방법 1] -f 옵션
–force 옵션과 동일하다.

[방법 2] +[branch name]
해당 branch를 강제로 push한다.

```

# untracked 파일 삭제하기

```
git clean 명령은 추적 중이지 않은 파일만 지우는 게 기본 동작이다.
즉, .gitignore 에 명시하여 무시되는 파일은 지우지 않는다.

$ git clean -f // 디렉터리를 제외한 파일들만 삭제
$ git clean -f -d // 디렉터리까지 삭제
$ git clean -f -d -x // 무시된 파일까지 삭제

# TIP option

-d 옵션
디렉터리까지 지우는 것

-x 옵션
무시된 파일(.DS_Store나 .gitignore에 등록한 확장자 파일들)까지 모두 지우는 것
Ex) .o 파일 같은 빌드 파일까지도 지울 수 있다.

-n 옵션
가상으로 실행해보고 어떤 파일들이 지워질지 알려주는 것

```

## Commit

커밋 합치기

```
git rebase -i HEAD~4 // 최신 4개의 커밋을 하나로 합치기
```

커밋 메세지 수정

```
$ git commit --amend // 마지막 커밋메세지 수정(ref)
```

간단한 commit 방법

```
$ git add {변경한 파일명}
$ git commit -m “{변경 내용}"
```

커밋 이력 확인

```
$ git log // 모든 커밋로그 확인
$ git log -3 // 최근 3개 커밋로그 확인
$ git log --pretty=oneline // 각 커밋을 한 줄로 표시
```

커밋 취소

```
$ git reset HEAD^ // 마지막 커밋 삭제
$ git reset --hard HEAD // 마지막 커밋 상태로 되돌림
$ git reset HEAD * // 스테이징을 언스테이징으로 변경, ref
```

## Branch

master 브랜치를 특정 커밋으로 옮기기

```
git checkout better_branch
git merge --strategy=ours master    # keep the content of this branch, but record a merge
git checkout master
git merge better_branch            # fast-forward master up to the merge
```

브랜치 목록

```
$ git branch // 로컬
$ git branch -r // 리모트
$ git branch -a // 로컬, 리모트 포함된 모든 브랜치 보기
```

브랜치 생성

```
git branch new master // master -> new 브랜치 생성
git push origin new // new 브랜치를 리모트로 보내기
```

브랜치 삭제

```
git branch -D {삭제할 브랜치 명} // local
git push origin :{the_remote_branch} // remote
```

빈 브랜치 생성

```
$ git checkout --orphan {새로운 브랜치 명}
$ git commit -a // 커밋해야 새로운 브랜치 생성됨
$ git checkout -b new-branch // 브랜치 생성과 동시에 체크아웃
```

리모트 브랜치 가져오기

```
$ git checkout -t origin/{가져올 브랜치명} // ref
```

브랜치 이름 변경

```
$ git branch -m {new name} // ref
```

## Tag

태그 생성

```
git tag -a {tag name} -m {tag message} {commit hash}
git tag {tag name} {tag name} -f -m "{new message}" // Edit tag message
```

태그 삭제

```
git tag -d {tag name}
git push origin :tags/{tag name} // remote
```

태그 푸시

```
git push origin --tags
git push origin {tag name}
git push --tags
```

## 기타

파일 삭제

```
git rm --cached --ignore-unmatch [삭제할 파일명]
```

히스토리 삭제

- 목적: 패스워드, 아이디 같은 비공개 정보가 담긴 파일을 실수로 올렸을 때 삭제하는 방법이다. (history에서도 해당 파일만 삭제)

```
$ git clone [url] # 소스 다운로드
$ cd [foler_name] # 해당 폴더 이동
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch [삭제할 파일명]' --prune-empty -- --all # 모든 히스토리에서 해당 파일 삭제
$ git push origin master --force # 서버로 전송
```

히스토리에서 폴더 삭제:

```
git filter-branch --tree-filter 'rm -rf vendor/gems' HEAD
```

리모트 주소 추가하여 로컬에 싱크하기

```
$ git remote add upstream {리모트 주소}
$ git pull upstream {브랜치명}
```

최적화

```
$ git gc
$ git gc --aggressive
```

## 서버 설정

강제 푸시 설정

```
git config receive.denynonfastforwards false
```

## Alias

~/.gitconfig 파일을 설정하여 깃 명령어의 앨리어스를 지정할 수 있다.

~/.gitconfig > alias 부분:

```

[alias]
  br = branch
  co = checkout
  rb = rebase
  st = status
  cm = commit
  pl = pull
  ps = push
  lg = log --graph --abbrev-commit --decorate --format=format:'%C(cyan)%h%C(reset) - %C(green)(%ar)%C(reset)  %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(yellow)%d%C(reset)' --all
  ad = add
  tg = tag
  df = diff
```
