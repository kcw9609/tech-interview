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












