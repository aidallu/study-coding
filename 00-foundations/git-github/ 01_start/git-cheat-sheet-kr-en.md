# Git Cheat Sheet (EN + KR)

## Set up
_설정과 도움말 기본기_

### Git Config
_깃 설정 확인/수정_

**See all the config**  
```bash
git config --list # show all settings (전체 설정 보기)
```

**Open config to edit**  
```bash
git config --global -e # open ~/.gitconfig (글로벌 설정 파일 편집)
```

**Set default editor**  
```bash
git config --global core.editor "code --wait" # set VS Code as default editor (기본 에디터로 VS Code 지정)
```

**User settings**  
```bash
git config --global user.name "name"   # set user.name (사용자 이름 설정)
git config --global user.email "email" # set user.email (사용자 이메일 설정)
git config user.name   # check user.name (사용자 이름 확인)
git config user.email  # check user.email (사용자 이메일 확인)
```

**Set Auto CRLF**  
```bash
git config --global core.autocrlf true   # for Windows (윈도우: 체크아웃 시 CRLF, 커밋 시 LF)
git config --global core.autocrlf input  # for macOS/Linux (맥/리눅스: 입력 시에만 LF로 변환)
```

**Git Aliases**  
```bash
git config --global alias.co checkout # 'co'를 'checkout' 별칭으로 (체크아웃 단축)
git config --global alias.br branch   # 'br' -> 'branch' (브랜치 단축)
git config --global alias.ci commit   # 'ci' -> 'commit' (커밋 단축)
git config --global alias.st status   # 'st' -> 'status' (상태 확인 단축)
```

### Help
_공식 문서와 도움말_

**Git official docs**  
https://git-scm.com/docs

**See help**  
```bash
git config --help # detailed help (자세한 도움말)
git config -h     # short help (짧은 도움말)
```


## Basic
_초기화, 상태, 무시 규칙, 스테이징, 변경 보기_

### Git init
```bash
git init      # initialise in current folder (현재 폴더에서 Git 시작)
rm -rf .git   # delete .git (Git 이력 폴더 완전 삭제 - 주의)
```

### Show the working tree status
```bash
git status    # full status (전체 상태 출력)
git status -s # short status (간단 상태 출력)
```

### Ignoring Files
_프로젝트 루트의 `.gitignore`에 무시 규칙 추가_
```bash
# ignore all .a files (모든 .a 파일 무시)
*.a

# but track lib.a even though '*.a' is ignored (lib.a는 추적)
!lib.a

# only ignore TODO in current dir (현재 폴더의 TODO만 무시)
/TODO

# ignore any directory named build (모든 build/ 폴더 무시)
build/

# ignore doc/notes.txt but not doc/server/arch.txt (doc/ 최상위만)
doc/*.txt

# ignore all .pdf under doc/ and subdirs (doc/ 이하 모든 .pdf 무시)
doc/**/*.pdf
```

### Staging files
```bash
git add a.txt           # stage a.txt (파일 스테이징)
git add a.txt b.txt     # stage multiple files (여러 파일 스테이징)
git add *.txt           # stage all *.txt (확장자 기준)
git add *               # stage all (숨김/삭제 제외 주의)
git add .               # stage everything in current dir (현재 폴더 전체 스테이징)
```

### Modifying files

**Removing files**
```bash
rm file.txt                # delete file locally (로컬 파일 삭제)
git add file.txt           # stage the deletion (삭제를 스테이징)
git rm file.txt            # remove from working dir + index (로컬+스테이징에서 삭제)
git rm --cached file.txt   # untrack only (스테이징/추적만 해제, 파일은 보존)
git clean -fd              # remove all untracked files/dirs (비추적 모두 삭제 - 주의)
```

**Moving files**
```bash
git mv from.txt to.txt           # rename/move (파일 이름 변경/이동)
git mv from.txt /logs/from.txt   # move to another folder (다른 폴더로 이동)
```

### Viewing the Staged/Unstaged changes
```bash
git status            # status (상태 확인)
git status -s         # short (간단 상태)
git diff              # working tree changes (워킹 디렉토리 변경)
git diff --staged     # staging area changes (스테이징 변경)
git diff --cached     # same as --staged (동일)
```

#### Visual Diff Tool
**Add to `~/.gitconfig`**
```ini
[diff]
    tool = vscode
[difftool "vscode"]
    cmd = code --wait --diff $LOCAL $REMOTE
```
**Run Git Diff tool**
```bash
git difftool # open external diff tool (외부 비교 툴 실행)
```

### Commit
```bash
git commit                      # commit staged (에디터로 메시지 작성)
git commit -m "message"         # commit with message (메시지 인라인 작성)
git commit -am "message"        # stage tracked + commit (신규 파일 제외)
```


## Log · History
_커밋 이력 보기/필터/서식_

### See history
```bash
git log                 # list commits (커밋 목록)
git log --patch         # show diff per commit (커밋별 변경 내용)
git log -p              # same as --patch (동일)
git log --stat          # file change stats (파일 변경 통계)
git log --oneline       # one-line view (한 줄 요약)
git log --oneline --reverse  # oldest → newest (오래된 것부터)
```

### Formatting
```bash
git log --pretty=oneline             # one line per commit (한 줄 포맷)
git log --pretty=format:"%h - %an %ar %s" # custom fields (해시/작성자/상대시간/메시지)
git log --pretty=format:"%h %s" --graph # with graph (그래프 포함)
git log --graph --all --pretty=format:'%C(yellow)[%ad]%C(reset) %C(green)[%h]%C(reset) | %C(white)%s %C(bold red){{%an}}%C(reset) %C(blue)%d%C(reset)' --date=short # graph + all refs (그래프/모든 참조/커스텀 포맷)
```

### Filtering
```bash
git log -2                    # last n commits (최근 n개)
git log --author="ellie"      # by author (작성자 필터)
git log --before="2020-09-29" # before date (특정 날짜 이전)
git log --after="one week ago"# after date (상대 시간 이후)
git log --grep="message"      # grep in messages (메시지 검색)
git log -S="code"             # pickaxe search in code (코드 추가/삭제 검색)
git log file.txt              # history for a file (특정 파일 이력)
```

### History of a file
```bash
git log file.txt        # commits for file (파일 이력)
git log --stat file.txt # stats for file (파일 통계)
git log --patch file.txt# diffs for file (파일 변경 내역)
```

### HEAD · Hash code
```bash
git log HEAD     # log from HEAD (현재 HEAD 기준 로그)
git log HEAD~1   # log from parent (이전 커밋 기준)
git log <hash>   # log starting at a commit (해시 기준)
```

### Viewing a commit
```bash
git show HEAD            # last commit details (마지막 커밋 상세)
git show <hash>          # specific commit (특정 커밋 상세)
git show <hash>:file.txt # file at that commit (해당 시점의 파일)
```

### Comparing
```bash
git diff <hash1> <hash2>           # diff between commits (커밋 간 비교)
git diff <hash1> <hash2> file.txt  # file only (특정 파일만 비교)
```


## Tagging
_버전 태그 붙이기/공유하기_

### Creating
```bash
git tag v1.0.0               # lightweight tag at HEAD (가벼운 태그)
git tag v1.0.0 <hash>        # lightweight tag at commit (특정 커밋에 태그)
git show v1.0.0              # show tag target (태그 대상 보기)
git tag -a v1.0.0 -m "msg"   # annotated tag (주석/작성자/서명 포함)
```

### Listing
```bash
git tag               # list tags (태그 목록)
git tag -l "v1.0.*"   # filter tags (패턴 검색)
```

### Deleting
```bash
git tag -d v1.0.0     # delete local tag (로컬 태그 삭제)
```

### Syncing with Remote
```bash
git push origin v1.0.0        # push one tag (특정 태그 푸시)
git push origin --tags        # push all tags (모든 태그 푸시)
git push origin --delete v1.0.0 # delete remote tag (원격 태그 삭제)
```

### Checking out Tags
```bash
git checkout v1.0.0                   # checkout tag (detached HEAD 상태)
git checkout -b branchName v1.0.0     # new branch from tag (태그 시점에서 새 브랜치)
```


## Branch
_브랜치 생성/조회/비교/정리_

### Creating branch
```bash
git branch testing        # create branch (브랜치 생성)
git checkout testing      # switch to it (전환)
git switch testing        # same (동일 기능)
git checkout -b testing   # create + switch (생성 후 전환)
git switch -C testing     # same (없으면 생성/있으면 전환)
```

### Managing Branch
```bash
git branch             # list local (로컬 브랜치 목록)
git branch -r          # list remote (원격 브랜치 목록)
git branch --all       # local + remote (전체 목록)
git branch -v          # last commit per branch (브랜치 최근 커밋)
git branch --merged    # merged into HEAD (HEAD에 병합된 브랜치)
git branch --no-merged # not merged into HEAD (아직 미병합)
git branch -d testing  # delete branch safely (안전 삭제)
git push origin --delete testing   # delete remote branch (원격 삭제)
git branch --move wrong correct    # rename branch (브랜치명 변경)
git push --set-upstream origin correct # set upstream (원격 추적 설정)
```

### Comparing
```bash
git log branch1..branch2  # commits in branch2 not in branch1 (차집합 커밋)
git diff branch1..branch2 # changes between branches (브랜치 간 변경점)
```

## Merge
```bash
git merge featureA            # merge into current (현재 브랜치에 병합)
git merge --squash featureA   # squash merge (하나의 커밋으로 병합)
git merge --no-ff featureA    # always create merge commit (머지 커밋 강제)
git merge --continue          # continue after conflicts resolved (충돌 해결 후 계속)
git merge --abort             # abort merge (머지 중단/원복)
git mergetool                 # open merge tool (머지 툴 열기)
```

**Merge tool Config**
```ini
[merge]
    tool = vscode
[mergetool]
    keepBackup = false
[mergetool "vscode"]
    cmd = code --wait $MERGED
[mergetool "p4merge"]
    path = "/Applications/p4merge.app/Contents/MacOS/p4merge"
```

## Rebasing
```bash
git rebase master  # rebase onto master (기준 브랜치에 재배치)
# note: default branch may be 'main' (기본 브랜치명이 'main'일 수 있음)

git rebase --onto master service ui
# take commits of 'ui' forked from 'service' and move onto 'master'
# ('service'에서 분기된 'ui'의 커밋을 'master' 위로 옮김)
```

## Cherry picking
```bash
git cherry-pick <hash>  # apply specific commit onto current (특정 커밋만 적용)
```


## Stashing
_작업 임시 저장_

### Saving
```bash
git stash push -m "message"  # new stash (새 스태시 생성)
git stash                    # same as push (기본 동작)
git stash --keep-index       # keep staged changes (스테이징은 유지)
git stash -u                 # include untracked (비추적 포함)
```

### Listing
```bash
git stash list               # list stashes (스태시 목록)
git stash show <stash>       # show summary (요약)
git stash show <stash> -p    # show patch (자세한 변경 내역)
```

### Applying
```bash
git stash apply <stash>      # apply specific (특정 스태시 적용)
git stash apply              # apply latest (최근 스태시 적용)
git stash apply --index      # re-stage changes (스테이징 복원)
git stash pop                # apply and drop (적용 후 삭제)
git stash branch branchName  # new branch from stash (스태시로 브랜치 생성)
```

### Deleting
```bash
git stash drop <stash>  # delete specific (특정 스태시 삭제)
git stash clear         # delete all (모두 삭제)
```


## Undo
_되돌리기(주의해서 사용)_

### Local Changes

**Unstaging a staged file**
```bash
git reset HEAD file.txt  # unstage file (스테이징 해제)
```

**Unmodifying a modified file**
```bash
git checkout -- file.txt  # old style; prefer restore (옛 방식, restore 권장)
```

**Discarding local changes**
```bash
git restore --staged file.txt  # unstage (스테이징 해제)
git restore file.txt           # discard changes (변경 취소)
git restore .                  # discard all in dir (디렉토리 전체 취소)
git clean -fd                  # remove untracked files/dirs (비추적 삭제 - 주의)
```

**Restoring file from certain commit**
```bash
git restore --source=<hash> file.txt   # restore from commit (특정 커밋에서 복원)
git restore --source=HEAD~2 file.txt   # restore from HEAD-2 (두 단계 이전에서 복원)
```

### Commit

**Amending the last commit**
```bash
git commit --amend  # edit last commit (마지막 커밋 수정/메시지 변경)
```

### Reset
_직전 커밋 취소 예시는 HEAD가 아니라 `HEAD~1`을 사용해야 효과가 있습니다._
```bash
git reset --soft HEAD~1  # keep changes staged (스테이징 유지)
git reset --mixed HEAD~1 # keep changes in working tree (워킹만 유지, 기본)
git reset --hard HEAD~1  # discard changes (모두 삭제 - 위험)
```

### Undo action - reflog
```bash
git reflog                 # reference log (HEAD 이동 이력 보기)
git reset --hard <hash>    # move to a previous point (이전 지점으로 강제 이동 - 위험)
```

### Revert
```bash
git revert <hash>                 # create a revert commit (되돌리는 새 커밋 생성)
git revert HEAD~1                 # previous commit revert (이전 커밋 되돌리기)
git revert --no-commit <hash>     # stage revert only (커밋 없이 변경만 스테이징)
```

### Interactive Rebasing
```bash
git rebase -i HEAD~2  # interactive (인터랙티브 리베이스)
git rebase --continue # continue after conflicts (충돌 해결 후 계속)
git rebase --abort    # abort rebase (리베이스 중단)
```


## Remote
_원격 저장소 연결/동기화_

```bash
git clone <URL>              # clone repo (원격 저장소 복제)
git remote -v                # list remotes (원격 주소 목록)
git remote add name <URL>    # add remote (원격 추가)
```

**Inspecting**
```bash
git remote             # list names (원격 이름 목록)
git remote show        # show help (도움말)
git remote show origin # show remote details (원격 상세)
```

**Syncing with remotes**
```bash
git fetch                  # download remote objects (원격 변경 내려받기)
git fetch origin           # from origin (origin에서 받기)
git fetch origin master    # only master (특정 브랜치만 - 또는 main)
git pull                   # fetch + merge (가져와서 병합)
git pull --rebase          # fetch + rebase (가져와서 리베이스)
git push                   # upload to remote (원격으로 푸시)
git push origin master     # push master (또는 main)
```

**Renaming or Removing**
```bash
git remote rename sec second  # rename remote (원격 이름 변경)
git remote remove second      # remove remote (원격 제거)
```


## Tools

### Basic Debugging
```bash
git blame file.txt  # who/when changed lines (라인별 변경자/시간 추적)
```

### Bisect
```bash
git bisect start         # start bisection (이진 탐색 시작)
git bisect good <hash>   # mark good commit (정상 커밋 표시)
git bisect good          # mark current as good (현재 정상)
git bisect bad           # mark current as bad (현재 문제)
git bisect reset         # stop and return (원상 복귀)
```

### Config (Aliases)
_유용한 별칭 모음(필요한 것만 선택)_
```ini
[alias]
    s = status
    l = log --graph --all --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %Cgreen(%cr) %C(magenta)<%an>%Creset'  # 그래프/모든 참조/요약 로그
    up = !git fetch origin master && git rebase origin/master   # 원격 master를 기준으로 리베이스 (기본 브랜치가 main이면 수정)
    co = checkout
    ca = "!git add -A && git commit -m"  # 사용법: git ca "msg" (모든 변경 추가 후 커밋)
    cad = "!git add -A && git commit -m '.'" # 모든 변경점을 '.' 메시지로 커밋
    c = commit
    b = branch
    list = stash list
    save = stash save 
    pop = stash pop
    apply = stash apply
    rc = rebase --continue   # 리베이스 계속 (en-dash 교정)
    get = "!f(){ git fetch && git checkout $1 && git reset --hard origin/$1; };f" # 원격 브랜치 강제 동기화
    new = "!f(){ git co -b ellie-$1 origin/master && git pull; };f"  # 새 브랜치 생성 후 최신화 (main이면 수정)
    st = status
    hist = log --graph --all --pretty=format:'%C(yellow)[%ad]%C(reset) %C(green)[%h]%C(reset) | %C(white)%s %C(bold red){{%an}}%C(reset) %C(blue)%d%C(reset)' --date=short # 그래프/날짜/작성자 포함 포맷
```

---

### Notes (KR)
- **detached HEAD**: 태그나 해시로 체크아웃하면 브랜치가 아닌 특정 시점으로 이동합니다. 수정하려면 새 브랜치를 만드세요.  
- **reset vs revert**: 협업 저장소에서는 `reset --hard`보다 `revert`가 안전합니다.  
- 기본 브랜치 이름은 저장소마다 다릅니다(최근 기본은 `main`). 필요 시 `master` → `main`으로 바꾸세요.
