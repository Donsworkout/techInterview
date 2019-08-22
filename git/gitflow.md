[목록으로](https://github.com/Donsworkout/techInterview/blob/master/README.md)

## 0. Git 기본
1. -u 키워드는 브랜치 명시 안하면, git push 할때 master로 푸시 하고  
그냥 pull 할때는 origin의 master를 fetch하고 merge 하라는 뜻이다.
~~~
git push -u origin master
~~~

2. **fetch**  
fetch는 merge 하지 않고 그냥 변경사항만 확인할때 사용한다.  
이 때 가져온 최신 커밋 이력은 이름 없는 브랜치로 로컬에 저장됨    
이 브랜치는 **'FETCH_HEAD'** 의 이름으로 체크아웃도 가능
<img src="https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup3_2_1.png">  

이 상태에서 원격 저장소의 내용을 로컬 저장소의 'master'에 통합하고 싶은 경우에는,   
'FETCH_HEAD' 브랜치를 merge 하거나 다시 pull 을 실행하면 됨  

<img src="https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup3_2_2.png">

## 1. Clone 과 Fork의 차이
> Fork와 Clone은 거의 동일, Fork는 깃허브에서 제공하는 기능이다.   
단순 저장소 복제 -> **clone**  
공동작업 기여 -> **fork**
### 1.1 Clone
> 저장소를 그대로 복사하는 것 
~~~
git clone '클론 주소'
~~~
### 1.2 Fork
fork 된 저장소는 clone 된 저장소와는 다르게 실제 오리지날 프로젝트와 연결되어있는 것이 가장 큰 차이점이다.  
본인의 저장소에서 해당 프로젝트에 수정을 가하고 Pull Request 를 하였고,  
오리지날 프로젝트가 이를 수용했을 경우 기여가 가능하다.  
오리지널 저장소 변경 시, fork된 나의 저장소에도 변경이 적용된다.

#### 일반적인 워크플로우
1. 오리지널 저장소 fork
2. fork 한 개인 repo에서 local로 clone 
3. 로컬 repo에서 오리지널 저장소를 remote로 추가 
~~~
git remote add upstream 'original.repo'
~~~

출처: https://pasudo123.tistory.com/287 [unique individuals]

## 2. Merge / Rebase 
### 2.1 Merge  
> 같은 조상(Base) 과 가장 말단 커밋 두개를 이용하는 3-way Merge
<img src="https://git-scm.com/figures/18333fig0327-tn.png"/>  
<img src="https://git-scm.com/figures/18333fig0328-tn.png"/>  

### 2.2 Rebase
> 깨끗한 히스토리를 만들기 위해 사용한다. (브랜치 정리 등)  
1. 원래의 상태
<img src="https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup2_8_1_1.png"/>

2. 아래 스크립트 실행
~~~
$ git checkout issue3
Switched to branch 'issue3'
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: pull 설명을 추가
Using index info to reconstruct a base tree...
<stdin>:13: new blank line at EOF.
+
warning: 1 line adds whitespace errors.
Falling back to patching base and 3-way merge...
Auto-merging myfile.txt
CONFLICT (content): Merge conflict in myfile.txt
Failed to merge in the changes.
Patch failed at 0001 pull 설명을 추가

When you have resolved this problem run "git rebase --continue".
If you would prefer to skip this patch, instead run "git rebase --skip".
To check out the original branch and stop rebasing run "git rebase --abort".
~~~

이때 커밋 한단계 한단계를 merge 하며 conflict를 해결하게 된다.  
rebase 의 경우 충돌 부분을 수정 한 후에는 commit 이 아니라 rebase 명령에 --continue 옵션을 지정하여 실행해야 함

~~~
$ git add myfile.txt
$ git rebase --continue
Applying: pull 설명을 추가
~~~

<img src="https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup2_8_1.png"/>

이렇게 하면 issue3 브랜치는 rebase되어 master과 같은 베이스를 갖게 된다.  
그리고 나서 아직 issue3가 적용되지 않은 master 브랜치를 fast forward 해준다!

~~~
$ git checkout master
Switched to branch 'master'
$ git merge issue3
Updating 8f7aa27..96a0ff0
Fast-forward
 myfile.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
 ~~~
 
<img src="https://backlog.com/git-tutorial/kr/img/post/stepup/capture_stepup2_8_2.png"/>

## 3. Pull Request
> 당장 merge가 아니라 책임자가 확인후 merge 하는 것, 코드리뷰나 push 권한이 없는 경우에 사용  

### 일반적인 진행 순서 
1. Upstream Repository를 자신의 저장소로 Fork(Origin Repository)한다.

2. Fork 한 Repository를 clone 하여 로컬에 내려받는다.

3. 로컬 repo에서 오리지널 저장소를 remote로 추가 
~~~
git remote add upstream 'original.repo'
~~~

4. 로컬에 개발용 branch(feature/xx 등) 를 만들고 작업 진행한다.

5. 작업 완료시 remote의 origin에 알맞는 branch에 push 한다.

6. 깃허브에서 Pull Request를 보낸다. (Compare & pull request)

7. 책임자가 승인 여부를 결정하고, 승인이 되었다면 merge 한다.

8. 로컬 저장소도 해당 변경사항 fetch 하고 merge 하여 동기화 시킨다.

## 4. Git 브랜치 종류와 gitflow
https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html

## 5. 커밋 메시지 이름 바꾸기
~~~
// 최근부터 최근에서 두번째 커밋 헤드까지를 rebase 
git rebase -i HEAD~2
~~~

나오는 화면에서 맨 앞부분 edit 으로 바꾸고,
커밋 메시지 수정하면서 git rebase --continue 하면 하나하나 바꿀 수 있음 