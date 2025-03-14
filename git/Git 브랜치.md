# git 브랜치

브랜치가 어떤 커밋을 가리키는지도 확인
```
$ git log --online --decorate
```
브랜치 이동
```
$ git checkout [이동할 브랜치명]
```
갈라치는 브랜치 그래프로 확인
```
$ git log --online --decorate --graph --all
```

### 브랜치와 Merge 기초
- 예제1
1. 웹사이트가 있고 뭔가 작업을 진행하고 있다.
2. 새로운 이슈를 처리할 새 Branch를 하나 생성한다.
3. 새로 만든 Branch에서 작업을 진행한다.
이때 중요한 문제가 생겨서 그것을 해결하는 Hotfix를 먼저 만들어야 한다. 그러면 아래와 같이 할 수 있다.
1. 새로운 이슈를 처리하기 이전의 운영(Production) 브랜치로 이동한다.
2. Hotfix 브랜치를 새로 하나 생성한다.
3. 수정한 Hotfix 테스트를 마치고 운영 브랜치로 Merge 한다.
4. 다시 작업하던 브랜치로 옮겨가서 하던 일 진행한다.

$ git checkout -b [issueA]]
$ git checkout main
$ git checkout -b [hotfixB]
$ git commit -a -m '수정한 내용'
$ git checkout main
$ git merge main [hotfixB]
$ git branch -d [hotfixB]
$ git checkout [issueA]

### 충돌
Merge 충돌이 일어났을 때 Git이 어떤 파일을 Merge 할 수 없었는지 확인
```
$ git status
```
충돌된 코드 확인
```
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```
======= 위쪽의 내용은 HEAD 버전(merge 명령을 실행할 때 작업하던 master 브랜치)의 내용이고 아래쪽은 iss53 브랜치의 내용이다. 충돌을 해결하려면 위쪽이나 아래쪽 내용 중에서 고르거나 새로 작성하여 Merge 한다. 아래는 아예 새로 작성하여 충돌을 해결하는 예제다.

## 브랜치 관리
브랜치 목록 확인
```
$ git branch
```
## 브랜치 워크플로
- Long Running 브랜치 : 장기간에 걸쳐서 한 브랜치를 다른 브랜치와 여러 번 Merge 하는 것이 쉬운 편이다.
- 토픽 브랜치 : 어떤 한 가지 주제나 작업을 위해 만든 짧은 호흡의 브랜치다.

### 브랜치 추적
리모트 트래킹 브랜치를 로컬 브랜치로 Checkout 하면 자동으로 “트래킹(Tracking) 브랜치” 가 만들어진다 (트래킹 하는 대상 브랜치를 “Upstream 브랜치” 라고 부른다). 
트래킹 브랜치는 리모트 브랜치와 직접적인 연결고리가 있는 로컬 브랜치이다. 
트래킹 브랜치에서 git pull 명령을 내리면 리모트 저장소로부터 데이터를 내려받아 연결된 리모트 브랜치와 자동으로 Merge 한다.

트래킹 브랜치를 직접 만들 수 있는데 리모트를 origin 이 아닌 다른 리모트로 할 수도 있고, 브랜치도 master 가 아닌 다른 브랜치로 추적하게 할 수 있다. 
```
$ git checkout -b [브랜치명] [remote]/[브랜치명]
```
자동으로 브랜치를 생성하도록 할 수 있다.
```
$ git checkout --track [remote]/[브랜치명]
```
=> 해당 [브랜치명]과 같은 브랜치가 없는 경우, 로컬에 [브랜치명] 브랜치 생성

리모트 브랜치명과 다른 이름의 브랜치 생성
```
$ git checkout [새로운 브랜치명] [remote]/[브랜치명]
```
=> [새로운 브랜치명]으로 브랜치가 생성되며 push/pull 사용 시 [remote]/[브랜치명] 과 연결하여 자동으로 데이터를 불러오거나 가져온다.

이미 로컬에 존재하는 브랜치가 리모트의 특정 브랜치를 추적하도록
```
$ git branch -u [연결하고자하는브랜치] [remote]/[브랜치명] 혹은
$ git branch --set-upstream-to [연결하고자하는브랜치] [remote]/[브랜치명] 
```

### 추적 브랜치 확인
```
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
```
- ahead : 로컬브랜치가 remote브랜치보다 커밋 2개가 앞서있다
- behind : 1개 뒤쳐져있다
- trying something new : 추적하는 브랜치가 없다

서버로부터 최신 데이터를 가져온 뒤, 추적 사항 확인
```
$ git fetch --all; git branch -vv
```
### 리모트 브랜치 삭제
```
$ git push origin --delete [삭제할 브랜치명]
```
