# git 정리
## 참고 : https://learngitbranching.js.org/?locale=ko 및 블로그

### git 명령어 
- 커밋 ```git commit```

- 브랜치 생성 ```git branch bugFix```

- 특정 브랜치로 이동 ```git checkout bugFix```

- 병합(merge) ```git merge main``` main을 bugFix에 병합

- 재지정 (rebase) ```git rebase main```
  - 히스토리를 선형적으로 명확하게 만들 수 있다. 불필요한 병합 커밋 제거
  - 협업 워크플로우에서 치명적일 수 있음

- HEAD : 현재 체크아웃된 커밋(현재 작업 중인)을 가르킴.
  - 작업 트리 이동할때 브랜치명 대신 해시 사용 가능

- 브랜치 강제로 옮기기 : -f 옵션
    - ex) ```git branch -f main HEAD~3``` (강제로) main 브랜치를 HEAD에서 3번 뒤로 이동

- ```git reset``` 
    - 브랜치로 하여금 예전의 커밋을 가리키도록 이동시키는 방식으로 변경 내용을 되돌림. (히스토리를 고쳐쓴다)
	- 애초에 커밋하지 않은 것 처럼 예전 커밋으로 브랜치를 옮기는것
		- ```ex) git reset HEAD~1``` 현재 HEAD에서 1번 뒤로 변경 내용을 리셋한다.

- ```git revert``` 로컬 브랜치의 경우 리셋을 쓸 수 있지만, 다른 사람의 작업하는 리모트 브랜치에서는 쓸 수 없다.
	변경분을 뒤돌리고, 이 되돌린 내용을 다른 사람들과 공유하기 위해서는 git revert를 써야한다.
	- ex) ```git revert HEAD```
		
- ```git cherry-pick```
    - ex) ```git cherry-pick <Commit1> <Commit2> <...>```
	- 현재 위치(HEAD) 아래에 있는 일련의 커밋들에 대한 복사본을 만들겠다는 것을 간단히 줄인 말
		- ex) ```git checkout main; git cherry-pick C2 C4``` 메인으로 C2, C4 작업을 복사함
			
- Git 인터렉티브 리베이스(Interactive Rebase) : 체리-픽은 원하는 커밋이나 각 작업의 해시값이 무엇인지 알때 유용함.
	원하는 커밋을 모를 때 인터렉티브 리베이스를 사용.
    - ex) ```git rebase -i main``` 해당 명령어로 어떤 커밋을 취하거나 버릴지 쉽게 선택 가능, 커밋 순서 변경 가능
		
- 딱 한개의 커밋만 가져오기
	- ```git rebase -i / git cherry-pick``` 작업 후 해당 명령어로 특정 원하는 작업만 가져 올 수 있음
	
- ```git commit --amend``` 커밋 내용 정정 C2 -> C2'

- git 태그 : 특정 지점을 표시하기 위한 닻 역할
	- ex) ```git tag v1 C1``` 태그 v1을 커밋 작업 C1에 부여 / 커밋을 지정해주지 않으면 git은 HEAD가있는 지점에 태그를 붙임
	
- git describe : 해당 지점을 설명해줌
	- ex) ```git describe main``` main을 다음과 같은 형태로 출력한다.<br>
		- "tag"_"numCommits"_g"hash"
		- tag는 가장 가까운 부모 태그 / numCommits은 그 태그가 몇 커밋 멀리있는지 /<hash>는 묘사하고있는 커밋의 해시

- ^ 연산자에서 병합이 된 상태에서 어떤 부모를 참조할지 선택 할 수 있음
	- ex) ```git checkout HEAD^``` 병합된 상태에서 첫 부모 위에 체크아웃
    - ex) ```git checkout HEAD^2``` 병합된 상태에서 다른(다른 커밋트리 줄기의) 부모를 선택
	- ex) ```git checkout HEAD~^2~2``` 해당 위치 체크아웃  ```git branch bugWork HEAD~^2~``` 해당 위치에 브랜치 생성
            
### 참고) 상대 참조 연산자
- 캐럿 연산자(^)
    - ```main^``` - main의 부모를 가르킴
    - ```main^^``` - main의 조부모(부모의 부모)를 의미 / ^ 연산자 뒤에 숫자 사용 가능

- 틸드 연산자 (~) ```git checkout HEAD~4``` - HEAD에서 4번 뒤로 이동


### Git Remote (원격) GitHub 관련 명령어

- ```git clone``` 원격 저장소의 복사본을 로컬에 생성
    - ex) ```git clone https://github.com/hesongg/etc.git```

- ```git fetch```
    - 원격 저장소의 내용을 업데이트 받음 
    - 로컬에서 나타내는 원격 저장소의 상태를 실제 원격저장소의 상태와 동기화 시킴
	- 자신의 로컬 상태는 바꾸지 않음
	
- ```git pull``` 로컬에서 fetch 와 merge 가 동시에 일어남

- ```git fakeTeamwork``` 원격 저장소에 커밋을 가장할 수 있음
	- ex) ```git faketeamwork foo 3``` 원격 저장소의 foo 브랜치에 세개의 커밋을 push 한것처럼 가장함
	
- ```git push``` 작업을 원격 저장소에 업로드하고 그 원격 저장소가 새 커밋들을 합치고 갱신하게 함.

- 로컬과 원격 저장소의 내용이 다를 때
	- fetch로 최신버전 확인한 다음, merge 또는 rebase 후 push
	- ```git pull; git push```
	- ```git pull --rebase; git push``` (위와 다르게 merge 대신 rebase 수행)
	
- 원격 저장소 거부 (Remote Rejected)
	- 협업할 경우 보통 원격저장소의 main 브랜치는 잠겨있음. 보통 변경사항을 적용하려면 pull request 과정을 거쳐야하는데
	만약 로컬 저장소의 main 브랜치에서 커밋을 한 후 push를 하려고 시도한다면, 오류가 발생할 수 있음
	- main 브랜치에 커밋을 잘못한 경우, 로컬에서 git reset 후 브랜치를 생성한 후 push 할 것

- git reset 옵션
	- ```--hard``` 돌아가려는 이력 이후의 모든 내용을 지운다.
	- ```--soft``` 돌아가려는 이력으로 돌아갔지만, 이후의 내용이 지워지지 않고 해당 내용의 인덱스도 그대로 있음 (바로 다시 커밋 가능한 상태)
	- ```--mixed``` git의 기본 옵션 / 이후에 변경된 내용에 대해서는 남아있지만, 인덱스는 초기화 됨 (커밋하려면 다시 변경된 내용 추가 필요)

- ```git checkout -b feature C2``` feature 브랜치 생성 후 C2로 이동

- merge 또는 rebase : 이력이 보존되는 것을 원하면 merge, 커밋트리가 깔끔한건 rebase


#### 원격 저장소 관련 명령어 활용

- 브랜치 원격 추적 : 보통 main의 원격 저장소의 main(o/main)을 추적한다
	- ex) foo 브랜치가 o/main을 추적하도록 설정
	 - ```git checkout -b foo o/main; git commit; git push```
	 - ```git checkout -b foo o/main; git pull```
	 - ```git branch -u o/main foo``` 또는 현재 작업하고 있는 브랜치면 생략가능 ```git branch -u o/main``` -> 브랜치 생성되어있어야 함
	
- ```git push origin main``` 내 저장소에 있는 main 브랜치로 가서 모든 커밋들을 수집하고, 그 다음 origin 의 main 브랜치로 가서
	이 브랜치에 부족한 커밋들을 채워넣고 완료되면 알려줌
	- 인자가 없을 경우, HEAD가 원격 저장소를 추적하는 브랜치에 체크아웃되어 있지 않으면 push가 실패함
	
- ```git push origin <source>:<destination>``` push할때 소스와 목적지를 다르게 할 수 있다.
	- ex) ```git push origin main:newBranch``` main 이라는 소스를 newBranch라는 브랜치명으로 오리진에 push함
	
- ```git fetch origin foo``` 커밋들을 foo 브랜치에서만 내려받은 후 로컬의 o/foo 브랜치에만 적용한다.

- ```git fetch origin foo:bar``` origin의 foo를 로컬의 bar 브랜치에 추가함

- ```git push <source>``` 옵션 생략 시
    - ex) ```git push origin :foo``` 원격 저장소의 foo 브랜치 삭제

- ```git fetch <source>``` 옵션 생략 시
    - ex) ```git fetch origin :bar``` 로컬에 bar 브랜치 생성 -> 실제로 사용 많이 하지는 않을듯?
	
git pull은 결국 merge + fetch
	- git pull origin foo == git fetch origin foo; git merge o/foo
	- git pull origin bar~1:bugFix == git fetch origin bar~1:bugFix; git merge bugFix
	
### 사용해보면서 추가한 내용
- ```git config --global user.email "이메일주소"``` 이메일 주소 설정
- ```git config --global user.name "이름"``` 이름 설정

- ```mkdir git_repository``` 로컬에 git 저장소로 사용할 폴더 생성

- ```git init``` init 명령어로 로컬 저장소 생성

- (저장소의 내용을 로컬에 가져올 때 사용) ```git clone https://github.com/hesongg/etc.git```

- 로컬 파일을 커밋 시 Untracked file 이라는 내용 발생
    - git add 명령어로 관리대상에 추가 후 커밋 확인
    - git status 로 상태 확인

- ```git add <file>``` file을 git 관리 대상에 추가

- ```git commit -m "커밋내용"``` 커밋 내용 입력

- 커밋 후, github에 올릴 때 ```git remote add origin "자신의 git 주소"``` 를 입력 (remote 저장소와 연결)

- ```git push origin master``` 입력으로 master 내용을 origin 주소로 push -> 입력하면 로그인창 나오는데 로그인하면 push 됨
    - 오류발생 시, 기존 내용을 pull로 가져오고, push할 필요성이 있음 
    - 강제로 수행하려면 git push origin +master 사용 - 기존에 있는 내용 삭제

- ```git log``` git 로그 확인

- ```git diff``` 다른 커밋과 working directory를 비교하는 명령어 ( - 는 삭제, + 는 더해진 부분 )

- '''git remote rm origin''' git remote origin 삭제
