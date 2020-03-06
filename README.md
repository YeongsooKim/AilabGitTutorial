# AilabGitTutorial

# Git 이란

### 버전관리
관리 시스템(*VCS* - Version Control System)을 사용하면 파일을 이전 상태로 되돌릴 수 있고, 프로젝트를 통째로 이전 상태로 되돌릴 수 있고, 시간에 따라 수정 내용을 비교해 볼 수 있고, 누가 문제를 일으켰는지도 추적할 수 있고, 누가 언제 만들어낸 이슈인지도 알 수 있다. **VCS를 사용하면 파일을 잃어버리거나 잘못 고쳤을 때도 쉽게 복구할 수 있다.**


### 로컬 버전 관리
많은 사람은 버전을 관리하기 위해 디렉토리로 파일을 복사하는 방법을 사용한다. 하지만 작업하던 디렉토리를 지워버리거나, 실수로 파일을 잘못 고칠 수도 있고, 잘못 복사할 될 가능성이 있다.

이런 이유로 프로그래머들은 오래전에 로컬 VCS라는 걸 만들었다. 이 VCS는 아주 간단한 데이터베이스를 사용해서 파일의 변경 정보를 관리한다.
![local](https://user-images.githubusercontent.com/51704629/76032702-ce9a7080-5f7d-11ea-93fe-fe8ac4053b7b.png)

Figure1. 로컬 버전 관리.
- 컴퓨터 시스템 내 전자 방식으로 저장된 구조화된 정보 또는 데이터의 조직적 집합


### 중앙집중식 버전 관리(CVCS)
 위 문제와 프로젝트를 진행하다 보면 다른 개발자와 함께 작업해야 하는 경우 생기는 문제를 해결하기 위해 *CVCS*(중앙집중식 VCS)가 개발되었다. 이 시스템은 **파일을 관리하는 서버가 별도로 있고 클라이언트가 중앙 서버에서 파일을 받아서 사용(Checkout).**
![centeral](https://user-images.githubusercontent.com/51704629/76033107-c68f0080-5f7e-11ea-87b0-050567ba89d9.png)

Figure 2. 중앙집중식 버전 관리(CVCS).

* CVCS 환경은 로컬 VCS에 비해 장점
  * 모두 누가 무엇을 하고 있는지 알 수 있음
  * 관리자는 누가 무엇을 할지 꼼꼼하게 관리할 수 있음
  * 모든 클라이언트의 로컬 데이터베이스를 관리하는 것보다 VCS 하나를 관리하기가 훨씬 쉬움

* CVCS 환경 단점
  * 중앙 서버에 발생한 문제
    * 서버가 한 시간 동안 다운되면 그동안 아무도 다른 사람과 협업할 수 없고 사람들이 하는 일을 백업할 방법도 없음. 그리고 중앙 데이터베이스가 있는 하드디스크에 문제가 생기면 프로젝트의 모든 히스토리를 잃음.


### 분산 버전 관리 시스템
*DVCS*(분산 버전 관리 시스템)에서 **클라이언트는 단순히 파일의 마지막 스냅샷을 Checkout 하지 않고, 그냥 저장소를 히스토리와 더불어 전부 복제**. 서버에 문제가 생기면 이 복제물로 다시 작업을 시작할 수 있다. 클라이언트 중에서 아무거나 골라도 서버를 복원할 수 있다. **Clone은 모든 데이터를 가진 진정한 백업.**
![distributed](https://user-images.githubusercontent.com/51704629/76033716-3baf0580-5f80-11ea-9d9d-8329a9207051.png)

Figure 3. 분산 버전 관리 시스템(DVCS).

# Git Repository 구성


### Git 공간
Git 디렉토리는 Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳

* Local: 컴퓨터의 저장소(ex. 내 컴퓨터)
 Local에서 작업한 내용은 서버로 push 하지 않으면 서버 저장소에 변경 내용이 반영되지 않는다.

* Remote: 서버의 저장소(ex. Github)
 Local에서 push 받아서 작업한 내용을 저장하는 공간이다.

* working directory: 우리가 지금 사용하고 있는 컴퓨터에 있는 작업 디렉토리
 각자 컴퓨터에서 어떤 파일들을 만들거나, 파일의 내용을 수정하였거나 등등 각자 어떠한 작업을 한 디렉토리

* staging area: 변경 내용을 local repo 공간에 올리기 전 임시 저장소
 깃은 파일이 추가되고, 삭제되고, 수정되는 등 변경사항을 전부 파악할 수 있다. working directory 변경사항이 있는 파일들 중 최종적으로 다음 단계인 local repo에 저장할 파일들을 설정할 수 있고, 우리가 설정한 파일들이 staging area에 임시적으로 저장한다.

* local repo: 컴퓨터의 저장소
 전 단계인 staging area의 파일들을 최종적으로 우리 컴퓨터에 저장하는 저장소(working directory와 다르다). git은 저장소를 버전으로 관리하는데 working directory에서 변경된 내용은 저장(commit. commit에 대한 자세한 내용은 뒤에서)되지 않으면 변경사항이 저장소에 반영되지 않는다.
 
![image10](https://user-images.githubusercontent.com/51704629/76034501-73b74800-5f82-11ea-902c-3a25491be604.png)


#### 세 가지 상태
Git은 파일을 Committed, Modified, Staged 이렇게 세 가지 상태로 관리
* Committed란 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미
 
* Modified는 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것
 staging area, repo에 올라가지도 않은 상태
 
* Staged란 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태
 staging area에 파일이 올라간 상태

![areas](https://user-images.githubusercontent.com/51704629/76039076-b1ba6900-5f8e-11ea-8326-9402d0335dfb.png)

-------- version 사이에 편집내용 있고 이 내용이 앞선 version에 포함되지 않는다는 image 만들어서 넣을 것 --------

# 사전 작업

### Git 회원 가입
link: https://github.com/

(_Remote repository push 권한을 부여하기 위한 username 교육자에게 알려주세요_)
![invite_collaborator](https://user-images.githubusercontent.com/51704629/76041964-e03c4200-5f96-11ea-967e-323e439dbbc0.png)


### terminal color
편한 편집기를 이용하여 _~/.bashrc_ 열기

아래 줄을 찾은 후 '#' 제거

```python
#force_color_prompt=yes
```

저장 후 종료한 다음 아래 명어 실행

```python
source ~/.bashrc
```


### Git 설치 되어있는지 확인
아래 명령어를 통해서 git이 설치되어 있는지 확인

```
$ cd ~
$ git --version
git version 2.25.0
```

만약 git version이 표시되지 않는다면 ```sudo apt-get install git``` 명령어를 실행하여 gitd을 설치한다.


### 사용자 정보 입력
Git을 설치하고 나면 Git의 사용 환경을 적절하게 설정해 주어야 한다. 환경 설정은 한 컴퓨터에서 한 번만 하면 된다. 설정한 내용은 Git을 업그레이드해도 유지된다. 언제든지 다시 바꿀 수 있는 명령어도 있다.

```
$ git config --global user.name "yeongsookim"
$ git config --global user.email "kimkys768@gmail.com"
$ git config --list
user.email=kimkys768@gmail.com
user.name=yeongsookim
 ...
```


# Git 실습

### Git 저장소 만들기
주로 다음 주 가지 중 한 가지 방법으로 Git 저장소를 쓰기 시작한다.
* 아직 버전관리를 하지 않는 로컬 디렉토리 하나를 선택해서 Git 저장소를 적용하는 방법
* 다른 어딘가에서 Git 저장소를 Clone 하는 방법

#### 기존 디렉토리를 Git 저장소로 만들기
버전관리를 하지 아니하는 기존 프로젝트를 Git으로 관리하고 싶은 경우 우선 프로젝트의 디렉토리로 이동한다.

```
$ cd ~
$ mkdir git_tutorial_ws
$ mkdir local_repo && cd local_repo
$ git init
```

이 명령은 .git 이라는 하위 디렉토리를 만든다. .git 디렉토리에는 저장소에 필요한 뼈대 파일(Skeleton)이 들어 있다. 이 명령만으로는 아직 프로젝트의 어떤 파일도 관리하지 않는다. 

Git이 파일을 관리하게 하려면 저장소에 파일을 추가하고 커밋해야 한다. git add 명령으로 파일을 추가하고 git commit 명령으로 커밋한다(readme.txt 파일은 편한 편집기를 이용하여 만들어서 추가하면 된다).

```
$ git add readme.txt
$ git commit -m 'initial project version'
$ git log

commit 2a2f49f8ee1973b3befa0ad429651ca44ab4cacd (HEAD -> master)
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 10:26:57 2020 +0900

    initial project version

```

#### 기존 저장소를 Clone 하기
다른 프로젝트에 참여하려거나(Contribute) Git 저장소를 복사하고 싶을 때 git clone 명령을 사용한다. 

Clone YeongsooKim repository
link: https://github.com/YeongsooKim/remote_repo

![remote_repo](https://user-images.githubusercontent.com/51704629/76041736-407eb400-5f96-11ea-8c53-710c680908cb.png)

*Clone or download* 버튼 클릭해서 표시되는 링크 복사

링크 복사 후 Terminal 하나 새로 실행시킨후 아래 명령어 입력

```
$ cd ~
$ git clone "paste link"
```

이 명령은 “remote_repo” 라는 디렉토리를 만들고 그 안에 .git 디렉토리를 만든다. 그리고 저장소의 데이터를 모두 가져와서 자동으로 가장 최신 버전을 Checkout 해 놓는다. 그리고 저장소의 데이터를 모두 가져와서 자동으로 가장 최신 버전을 Checkout 해 놓는다. remote_repo 디렉토리로 이동하면 Checkout으로 생성한 파일을 볼 수 있고 당장 하고자 하는 일을 시작할 수 있다.

아래과 같은 명령을 사용하여 저장소를 Clone 하면 `remote_repo`이 아니라 다른 디렉토리 이름으로 Clone 할 수 있다.

```
$ cd ~
$ git clone "paste link" rename_repo
```

디렉토리 이름이 rename_repo 이라는 것만 빼면 이 명령의 결과와 앞선 명령의 결과는 같다.


### 수정하고 저장소에 저장하기
현재 상태: Git 저장소를 하나 만들었고 워킹 디렉토리에 Checkout
수행할 내용: 파일을 수정하고 파일의 스냅샷을 커밋

워킹 디렉토리의 모든 파일은 크게 Tracked(관리대상임)와 Untracked(관리대상이 아님)로 나눈다. Tracked 파일은 이미 스냅샷에 포함돼 있던 파일이다. Tracked 파일은 또 Unmodified(수정하지 않음)와 Modified(수정함) 그리고 Staged(커밋으로 저장소에 기록할) 상태 중 하나이다. 간단히 말하자면 Git이 알고 있는 파일이라는 것이다.

그리고 나머지 파일은 모두 Untracked 파일이다. Untracked 파일은 워킹 디렉토리에 있는 파일 중 스냅샷에도 Staging Area에도 포함되지 않은 파일이다. 처음 저장소를 Clone 하면 모든 파일은 Tracked이면서 Unmodified 상태이다. 파일을 Checkout 하고 나서 아무것도 수정하지 않았기 때문에 그렇다.

마지막 커밋 이후 아직 아무것도 수정하지 않은 상태에서 어떤 파일을 수정하면 Git은 그 파일을 Modified 상태로 인식한다. 실제로 커밋을 하기 위해서는 이 수정한 파일을 Staged 상태로 만들고, Staged 상태의 파일을 커밋한다. 이런 라이프사이클을 계속 반복한다.

![lifecycle](https://user-images.githubusercontent.com/51704629/76043203-23e47b00-5f9a-11ea-8be7-ec6a40932666.png)

#### 파일의 상태 확인하기
파일의 상태를 확인하려면 git status 명령을 사용한다. Clone 한 후에 바로 이 명령을 실행하면 아래과 같은 메시지를 볼 수 있다.

```
$ cd ~/git_tutorial_ws/remote_repo/yeongsoo
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

위의 내용은 파일을 하나도 수정하지 않았다는 것을 말해준다. Tracked 파일은 하나도 수정되지 않았다는 의미다. Untracked 파일은 아직 없어서 목록에 나타나지 않는다. 그리고 현재 작업 중인 브랜치를 알려주며 서버의 같은 브랜치로부터 진행된 작업이 없는 것을 나타낸다. 기본 브랜치가 master이기 때문에 현재 브랜치 이름이 “master” 로 나온다. 브랜치 설명은 차후에 다룬다.

#### 파일을 새로 추적하기
프로젝트에 개인 파일을 만들어보자. yeongsoo.md 파일은 새로 만든 파일이기 때문에 **git status** 를 실행하면 'Untracked files’에 들어 있다:
```
$ echo 'Hello ^______^' > yeongsoo.md
$ git status

On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	yeongsoo.md

nothing added to commit but untracked files present (use "git add" to track)
```

README 파일이 Untracked 상태라는 것을 말한다. Git은 Untracked 파일을 아직 스냅샷(커밋)에 넣어지지 않은 파일이라고 본다. 파일이 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 커밋하지 않는다.

추적할 수 있도록 **git add** 명령 사용.

```
$ git add yeongsoo.md
$ git status

On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   yeongsoo.md
```

yeongsoo.md 파일이 Tracked 상태이면서 커밋에 추가될 Staged 상태라는 것을 확인할 수 있다. “Changes to be committed” 에 들어 있는 파일은 Staged 상태라는 것을 의미한다.

#### Modified 상태의 파일을 Stage 하기
이미 Tracked 상태인 파일을 수정. readme.md 라는 파일을 수정하고 나서 git status 명령을 다시 실행하면 결과는 아래와 같다.

```
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   yeongsoo.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   readme.md
```

readme.md 파일은 “Changes not staged for commit” 에 있다. 이것은 수정한 파일이 Tracked 상태이지만 아직 Staged 상태는 아니라는 것이다. Staged 상태로 만들려면 git add 명령을 실행해야 한다. git add 명령은 파일을 새로 추적할 때도 사용하고 수정한 파일을 Staged 상태로 만들 때도 사용한다.

git add 명령을 실행하여 CONTRIBUTING.md 파일을 Staged 상태로 만들고 git status 명령으로 결과를 확인

```
$ git add readme.md
$ git status

On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   readme.md
	new file:   yeongsoo.md
```

#### 변경사항 커밋하기
수정한 것을 커밋하기 위해 Staging Area에 파일을 정리했다(Unstaged 상태의 파일은 커밋되지 않는다). 커밋하기 전에 git status 명령으로 모든 것이 Staged 상태인지 확인 후에 git commit 을 실행하여 커밋한다(선택적으로 commit을 한다면 모든 것이 Staged 상태일 필요는 없다).

명령어 git commit -m "comment"는 comment를 추가하여 commit을 한다.

```
$ git commit -m "ADD,REMOVE yeongsoo.md, readme.md"
[master 89beefa] ADD,REMOVE yeongsoo.md, readme.md
 2 files changed, 3 insertions(+)
 create mode 100644 workspace/yeongsoo/yeongsoo.md
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

commit 명령은 몇 가지 정보를 출력하는데 위 예제는 (master) 브랜치에 커밋했고 체크섬은 (463dc4f)이라고 알려준다. 그리고 수정한 파일이 몇 개이고 삭제됐거나 추가된 라인이 몇 라인인지 알려준다.

Git은 Staging Area에 속한 스냅샷을 커밋한다는 것을 기억해야 한다. 수정은 했지만, 아직 Staging Area에 넣지 않은 것은 다음에 커밋할 수 있다. 커밋할 때마다 프로젝트의 스냅샷을 기록하기 때문에 나중에 스냅샷끼리 비교하거나 예전 스냅샷으로 되돌릴 수 있다.


### 커밋 히스토리 조회하기
Git에는 히스토리를 조회하는 명령어인 git log 가 있다. 이 프로젝트 디렉토리에서 git log 명령을 실행하면 아래와 같이 출력된다.

```
$ git log
commit 89beefa5f7cedf74c050ed4a99368bfb8b6212a9 (HEAD -> master)
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:56:45 2020 +0900

    ADD,REMOVE yeongsoo.md, readme.md

commit ec511216aaaca376971f9177b5df0d1f418e36e2 (origin/master, origin/HEAD)
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:44:49 2020 +0900

    REMOVE yeongsoo.md

commit 4cbcd1e7a9350e725123ff413b3268fccb39398d
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:41:32 2020 +0900

    ADD files of education

commit 582a344c2d765aad4d4a7f1a4718219c23f7887a
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:37:43 2020 +0900

    ADD files of education

commit 4e8bc7da6e2fe0988773d18b1d20e8246f542061
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:20:54 2020 +0900

    ADD workspace folder

commit f9230be0117d01d1bcf4d51fe4f3bff52da90757
:
```

특별한 아규먼트 없이 git log 명령을 실행하면 저장소의 커밋 히스토리를 시간순으로 보여준다. 즉, 가장 최근의 커밋이 가장 먼저 나온다. 그리고 이어서 각 커밋의 SHA-1 체크섬, 저자 이름, 저자 이메일, 커밋한 날짜, 커밋 메시지를 보여준다.


### 되돌리기
종종 완료한 커밋을 수정해야 할 때가 있다. 너무 일찍 커밋했거나 하는 경우에 커밋 수정하는 것을 알아본다.

#### 파일 상태를 Unstage로 변경하기
다음은 Staging Area와 Working directory 사이를 넘나드는 방법을 설명한다. 예를 들어 파일을 두 개 수정하고서 따로따로 커밋하려고 했지만, 실수로 git add * 라고 실행해 버린 경우.

```
$ echo 'very gooooood' > yeongsoo.md
$ echo 'Hi @______@' > yeongsoo_.md
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   yeongsoo.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	yeongsoo_.md

no changes added to commit (use "git add" and/or "git commit -a")

$ git add *
$ git status 

On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   yeongsoo.md
	new file:   yeongsoo_.md

```

결과를 보면 두개 파일이 Stage Area 에 추가되어 있는 것을 볼 수 있다. 이것을 git reset HEAD <file>... 이 명령으로 Unstaged 상태로 변경할 수 있다. yeongsoo_.md 파일을 Unstaged 상태로 변경해보자.
	
```
$ git reset HEAD yeongsoo_.md
$ git status

On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   yeongsoo.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	yeongsoo_.md

```

yeongsoo_.md 파일은 Unstaged 상태가 됐다.


### commit 한 파일 되돌리기
git은 이전 버전 내용을 전부 기억하고 있기 때문에 다시 이전 체크섬으로 되돌릴 수 있다. 먼저 현재 저장되어 있는 readme.md 를 변경한다. 그 후 git add 후 git commit 명령어로 commit.

```
$ git add *
$ git commit -m "MODIFY readme.md"
$ git push
$ git log
commit 87bc6797a6d831e83961207262e7a8d19c8fd08f (HEAD -> master, origin/master, origin/HEAD)
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 12:40:56 2020 +0900

    MODIFY readme.md

commit ebe236cae5ab50cd58efe279c8b516f5700422e9
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 12:36:31 2020 +0900

    ADD yeongsoo_.md

commit 89beefa5f7cedf74c050ed4a99368bfb8b6212a9
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:56:45 2020 +0900

    ADD,REMOVE yeongsoo.md, readme.md

commit ec511216aaaca376971f9177b5df0d1f418e36e2
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:44:49 2020 +0900

    REMOVE yeongsoo.md

commit 4cbcd1e7a9350e725123ff413b3268fccb39398d
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:41:32 2020 +0900

    ADD files of education

commit 582a344c2d765aad4d4a7f1a4718219c23f7887a
:
```

편집기를 이용하여 readme.md 파일이 변경된 것을 확인한다. 그 후 git reset HEAD ebe236c 명령어를 통해서 체크섬 ebe236c으로 되돌린다(체크섬 hash는 7개 까지만 복사하면 된다).

```
$ git reset --hard ebe236c
HEAD is now at ebe236c ADD yeongsoo_.md
$ git log
commit ebe236cae5ab50cd58efe279c8b516f5700422e9 (HEAD -> master)
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 12:36:31 2020 +0900

    ADD yeongsoo_.md

commit 89beefa5f7cedf74c050ed4a99368bfb8b6212a9
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:56:45 2020 +0900

    ADD,REMOVE yeongsoo.md, readme.md

commit ec511216aaaca376971f9177b5df0d1f418e36e2
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:44:49 2020 +0900

    REMOVE yeongsoo.md

commit 4cbcd1e7a9350e725123ff413b3268fccb39398d
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:41:32 2020 +0900

    ADD files of education

commit 582a344c2d765aad4d4a7f1a4718219c23f7887a
Author: yeongsookim <kimkys768@gmail.com>
Date:   Fri Mar 6 11:37:43 2020 +0900

    ADD files of education

commit 4e8bc7da6e2fe0988773d18b1d20e8246f542061
:

```

내용물을 확인해보면 이전 readme.md 로 되돌려져 있는것을 확인할 수 있고, git log에서 이전 체크섬 87bc6797a6d831e83961207262e7a8d19c8fd08f 이 사라져 있는것을 확인할 수 있다. 하지만 git은 웬만해서는 삭제를 하지 않으므로 이전 체크섬을 git reflog 명령어로 확인할 수 있다.

```
$ git reflog
ebe236c (HEAD -> master) HEAD@{0}: reset: moving to ebe236c
87bc679 (origin/master, origin/HEAD) HEAD@{1}: commit: MODIFY readme.md
ebe236c (HEAD -> master) HEAD@{2}: commit: ADD yeongsoo_.md
89beefa HEAD@{3}: commit: ADD,REMOVE yeongsoo.md, readme.md
ec51121 HEAD@{4}: commit: REMOVE yeongsoo.md
4cbcd1e HEAD@{5}: commit: ADD files of education
582a344 HEAD@{6}: commit: ADD files of education
4e8bc7d HEAD@{7}: commit: ADD workspace folder
f9230be HEAD@{8}: commit: REMOVE initialize file
6929cda HEAD@{9}: pull: Fast-forward
445973f HEAD@{10}: clone: from https://github.com/YeongsooKim/remote_repo.git
```

결과에서 이전 체크섬 ebe236c 이 보이는 것을 확인

### 리모트 저장소
리모트 저장소에서 데이터를 가져오려면 간단히 아래와 같이 실행한다.

* fetch
	* 원격 저장소로부터 필요한 파일을 다운 (병합은 따로 해야 함)
* pull
	* 원격 저장소로부터 필요한 파일을 다운 + 병합
	* 

```
$ git fetch <remote>


### 커밋 충돌 해결하기

