
[프로젝트 관리 - 생활코딩 (opentutorials.org)](https://opentutorials.org/course/301)

----

## 로컬 저장소
#### git 내부
![[Pasted image 20230201164737.png]]

▶staging area
- commit을 하기 전에 commit할 파일들을 골라놓는 곳
- `git add 파일명` : 파일을 스테이징
- `git add 파일명1 파일명2` : 여러 파일을 동시에 스테이징
- `git add .` : 작업폴더의 모든 파일을 전부 스테이징
- `git status` : 뭐 하는지 까먹었을 때, 변경된 파일, 스테이징된 파일 이런걸 쭉 알려줌.
- `git restore --staged 파일명` : 스테이징된 파일을 취소하고 작업폴더로 넣고 싶을때
💥스테이징 전에 파일을 저장해야 변경파일이 저장됌.

▶repository
- commit된 파일의 버전들을 모아놓는 곳. 실체를 구경하고 싶으면 작업폴더안에 숨겨져 있는 .git 폴더 열어보기
- `git commit -m '아무메세지'` 

#### 로그
- `git log` : 로그 상세페이지 보여줌
![[Pasted image 20230201165735.png]]
- `git log --all --oneline` : commit 기록을 한 눈에 파악
- `git log --all --oneline --graph` : commit 기록을 그래프로 보여줌

#### 체크아웃
=> 체크아웃은 객실을 비우고 떠나는 것을 의미합니다. 즉, 현재 브랜치를 떠나 새로운 브랜치로 들어간다는 의미로, 깃에서 브랜치 간 이동할 때는 checkout 명령어를 사용합니다.
- `git checkout 브랜치이름`

####  임시저장소
- 코드를 잠깐 치워놓고 개발하고 싶으면 주석처리해도 되겠지만, git stash 명령어를 이용해도 잠깐 코드를 치울 수 있습니다.
- `git stash` :  방금 작성한 코드를 잠깐 다른 공간에 보관됩니다.
- `git stash save '메모'` : 메모가능
![[Pasted image 20230202025401.png]]

- `git stash list` : 현재 stash 되어있는 코드 목록을 전부 출력해주는 명령어
- `git stash pop` : 잠깐 보관했던 코드를 다시 불러옴. git stash 했던 코드가 여러개 있으면 가장 최근에 보관했던 코드부터 먼저 불러옴.
![[Pasted image 20230202025412.png]]
- `git stash drop 삭제할id` : 특정 stash 삭제
- `git stash clear` : 모든 stash 삭제하는 명령어

## 브랜치
#### 브랜치를 사용하는 이유는?
=> 계속 코드짜다보면 갑자기 새로운 기능을 추가하거나 그래야하는 경우가 있습니다. 원본파일에 코드를 추가하고 커밋해도 되겠지만 혹시나 잘못해서 지금까지 짰던 프로그램이 망가지거나 그러면 어떻게하죠? 그래서 **프로젝트의** **복사본을 만들어서 거기에 먼저 개발**하는 브랜치 사용함.
![[Pasted image 20230201173945.png]]


#### 브랜치 설정
- `git branch 브랜치명` : 브랜치 생성. 현재 브랜치 기준으로
- `git switch 브랜치이름` : 브랜치 이름으로 이동하고 싶을때
- `git branch -m 선택한브랜치명 바꿀브랜치명` : 브랜치 이름 변경
- `git branch -d 브랜치이름` : 병합이 완료된 브랜치 삭제시
- `git branch -D 브랜치이름` : 병합하지 않은 브랜치 삭제시


#### 브랜치 mege
- 브랜치를 합치는걸 전문용어로 merge
![[Pasted image 20230201183308.png]]
- `git merge 브랜치명` : 현재 브런치와 브랜치명과 합쳐짐

#### merge conflict
▶ 문제점 : A브랜치와 B브랜치에서 같은 파일, 같은 줄을 수정했을 경우 어느것도 선택 못해서 충돌일어남. (선택장애)
![[Pasted image 20230201183648.png]]

▶해결책 : <<<< / >>>> / ==== 이런 쓸데없는 것들은 다 지우고 원하는 코드만 남기기. 이후 add, commit 진행
![[Pasted image 20230201183525.png]]


## merge의 종류
#### 3-way merge
=> 브랜치에 각각 신규 commit이 1회 이상 있는 경우 merge 명령을 내리면 두 브랜치의 코드를 합쳐서 새로운 commit을 자동으로 생성하는 것.
![[Pasted image 20230201184112.png]]

#### fast-forward merge
=> 새로운 브랜치에만 commit 이 있고 기준이 되는 브랜치에는 신규 commit 이 없는 경우가 있습니다. 이 경우 merge 하게 되면 "fast-forward merge 되었습니다" 라고 알려줍니다.딱히 합칠게 없어서 그냥 신규브랜치 보고 "지금부터 니 이름은 main 브랜치여" 하는 것입니다.
![[Pasted image 20230201184118.png]]

#### rebase and merge
- rebase란? 브랜치의 시작점을 다른 commit으로 옮겨주는 행위
- 매커니즘
1) rebase를 이용해서 신규브랜치의 시작점을 main 브랜치 최근 commit으로 옮긴 다음(commit2 -> commit3)
2) fast-forward merge
![[Pasted image 20230201184155.png]]
- rebase and merge하는 이유는? 간단하고 짧은 브랜치들이 하면 깔끔해짐
- 단점 : 브랜치끼리 차이가 너무 많은 경우 rebase하면 충돌이 많이 발생할 수 있음.

#### squash and merge
- 3-way merge처럼 선으로 이어주지 않고 새 브랜치에 있던 코드변경사항들이 **main 브랜치로** **텔레포트**합니다.
- `git merge --squash 브랜치명`
![[Pasted image 20230201184219.png]]

## 파일 복구 명령어

#### restore
- 파일 하나를 되돌릴때 사용함
- `git restore 파일명` :  최근 commit 된 상태로 현재 파일의 수정내역 되돌리기
- `git restore --source 커밋아이디 파일명` :  입력한 파일이 특정 커밋아이디 시점으로 복구

#### revert
-  ex) commit이 3개 있는데, b파일이 문제가 많아서 b파일을 만든 d874b2b commit을 취소하고 싶을때
![[Pasted image 20230202003006.png]]

- `git revert 커밋아이디` : 그 커밋아이디에서 일어난 일만 취소해줌. 
![[Pasted image 20230202003139.png]]

![[Pasted image 20230202003132.png]]

#### reset
- 특정 commit 시절로 아예 **모든걸 되돌릴 수** 있습니다.
- `git reset --hard 커밋아이디` : 그 커밋이 생성될 때로 시간을 되돌려줍니다.
![[Pasted image 20230202012757.png]]

## 원격 저장소
- `git push -u 원격저장소주소 main` : 로컬저장소의 main 브랜치를 원격저장소에 올리기
![[Pasted image 20230202015801.jpg]]
- `git remote add 변수명 저장소주소` : 주소를 매번 입력하기 귀찮아 변수로 저장
- `git clone 원격저장소주소` : 원격저장소에 있던거 그대로 내려받기
- `git pull 원격저장소주소` : git pull 이용하면 현재 원격저장소 내용 가져올 수 있음
- git pull 명령어는 git fetch + git merge 축약어임. git fetch는 원격저장소에 있는 commit 중에 로컬에 없는 신규 commit을 가져오라는 뜻이고 git merge는 그걸 merge 하라는 뜻임. 그래서 git pull 할 때 팀원 2명이서 같은 파일을 건드리고 있을 경우 merge conflict가 날 수 있습니다.
💥git push가 안되면 원격저장소의 변경이 있는 것임. 그러면 git pull부터 한후 git push하자



## 발생했던 오류들
- nothing to commit, working tree clean
	✅원인
	1) 바뀐점이 없는데 add후, commit할때 생긴 오류
	2) add를 하지 않을 상태에서 commit할때
	- ![[Pasted image 20221020001056.png]]
	✅해결책
	- 수정 이후, add를 하고 commit을 하자
- Changes not staged for commit
- `error: failed to push some refs to 원격저장소주소` 
	- 원격 저장소의 내용과 로컬 저장소의 내용이 달라서 생기는 오류(팀원이 먼저 push함). 
	- 그러면 어떻게 해?` git pull`을 먼저하자
- `error: Your local changes to the following files would be overwritten by checkout:`
	- 