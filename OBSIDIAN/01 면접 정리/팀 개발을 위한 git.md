# git CLI
## 명령어
- git init 
	`git init` 명령어를 실행하면, 현재 디렉토리를 기준으로 Git 저장소가 생성
- git add ~

## 1.설정과 초기화

- 전역 사용자명/이메일 구성하기
```
git config --global user.name “Your name”
git config --global user.email “Your email address”
```

- 저장소별 사용자명/이메일 구성하기 (해당 저장소 디렉터리로 이동후)
	참고로 user 설정이 되어 있지 않으면 Github에 있는 repository에 변경사항을 푸시 한다고 해도 commit count 집계도 안됌
```
git config user.name “Your name”
git config user.email “Your email address”
```

- 전역 설정 정보 조회

```
git config --global --list
```

- 저장소별 설정 정보 조회

```
git config - -list

```

- 저장소 초기화하기
	현재 디렉토리를 기준으로 Git 저장소가 생성
```
git init
```

- 저장소 복제하기
```
git clone <저장소 url>
```

- 새로운 원격 저장소 추가하기

```
git remote add <원격 저장소> <저장소 url>
```

## 2. 기본적인 사용법
- 새로운 파일을 추가하거나 존재하는 파일 스테이징하고 커밋하기
	- 💥실수한 부분
		1) 파일이 변경되지 않은 상태에서 commit하면?
		2) add를 하지 않은 상태에서 commit하면?
```
git add <파일>
git commit -m “<메시지>”
```

파일의 일부를 스테이징하기

git add -p [<파일> [<파일> [기타 파일들…]]]

add 명령에서 Git 대화 모드를 사용하여 파일 추가하기

git add -i

수정되고 추적되는 파일의 변경 사항 스테이징하기

git add -u [<경로> [<경로>]]

수정되고 추적되는 모든 파일의 변경 사항 커밋하기

git commit -m “<메시지>” -a

작업 트리의 변경 사항 돌려놓기

git checkout HEAD <파일> [<파일>]

커밋되지 않고 스테이징된 변경 사항 재설정하기

git reset HEAD <파일> [<파일>]

마지막 커밋 고치기

git commit -m “<메시지>” - -amend

이전 커밋을 수정하고 커밋 메시지를 재사용하기

git commit -C HEAD - -amend

## 발생했던 오류들
- nothing to commit, working tree clean
	✅원인
	1) 바뀐점이 없는데 add후, commit할때 생긴 오류
	2) add를 하지 않을 상태에서 commit할때
	- ![[Pasted image 20221020001056.png]]
	✅해결책
	- 수정 이후, add를 하고 commit을 하자
- Changes not staged for commit
	