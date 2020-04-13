# Git 기초

> Git은 분산형버전관리시스템(DVCS)이다.
>
> 소스코드의 이력을 확인하고, 협업 단계에서 활용 가능하다. 

## 0. 기본 설정

윈도우에서 git을 활용하기 위해서는 `git bash`가 필요하다. [설치링크](https://gitforwindows.org/)

설치 이후에 `commit`을 작성하는 `author` 설정이 필요하다. 

``` bash
$ git config --global user.name {github username}
$ git config --global user.email {github email}

$ git config --global user.name sanga327
$ git config --global user.email dltkddk327@naver.com
```

설정 내용을 확인하기 위해서는 아래의 명령어를 입력한다. 

```bash
$ git config --global -l
user.email = dltkddk327@naver.com
user.name=sanga327
```

### gitignore

프로젝트를 진행할 때, `git`으로 관리하지 않을 파일 혹은 폴더들을 설정할 수 있다. 

```
*.xlsx  # 확장자가 xlsx인 파일
a.txt   # a.txt 파일 
.ipynb_checkpoints/ # .ipynb_checkpoints 폴더
```

프로젝트 시작시 어떠한 내용을 담아야 할 지 모르겠다면, [gitignore](http://gitignore.io/)에서 검색한다.

예) `python`, `r`, `jupyter notebook` 

---

## 로컬 저장소에서 활용하기 

### 1. git 저장소 설정

특정 프로젝트 폴더에서 `git`을 활용하기 위해서는 아래의 명령어를 입력한다. 

```bash
$ git init
Initialized empty Git repository in C:/Users/student/Desktop/git/.git/
(master) $ 
```

* 해당 디렉토리 내에 `.git`이라는 숨김 폴더가 생성되며, 모든 `git`과 관련된 동작은 해당 폴더에 기록된다. 
* git bash에서 `(master)`라는 브랜치 정보가 표기된다. 

### 2. add

`git`에서 커밋할 대상 파일을 `staging area`로 이동시키는 명령어이다. 

```bash
$ git add a.txt  # 특정 파일을 stage
$ git add images/  # 특정 폴더를 stage
$ git add .  # 모든 디렉토리 파일 및 폴더를 stage
```

* `add` 전 상태 

  ```bash
  $ touch a.txt
  $ touch b.txt
  $ git status
  On branch master
  
  No commits yet
  
  Untracked files:   # git 이력에 담기지 않은 파일 생성
    (use "git add <file>..." to include in what will be committed)
    # git add 명령어를 통해서 commit될 곳에 추가해라
          a.txt
          b.txt
  
  nothing added to commit but untracked files present (use "git add" to track)
  ```

  

* `add` 후 상태

  ```bash
  $ git add a.txt
  $ git status
  On branch master
  
  No commits yet
  
  Changes to be committed:  # commit될 변경사항
    (use "git rm --cached <file>..." to unstage)
          new file:   a.txt    # a.txt 새로운 파일 생성
  
  Untracked files:  # tracking되고있지 않은 파일들 
    (use "git add <file>..." to include in what will be committed)
          b.txt
  
  ```

  **항상 `git status` 명령어를 통해 현재 상태를 확인하는 것이 중요하다!!**

### 3. commit

git에서 이력을 남기기 위해서는 항상 `commit`을 통해서 진행한다. 

`commit`을 남길 때에는 항상 커밋 메시지를 작성한다. 

메시지는 해당 이력에 대한 정보를 담는다. 

```bash
$ git commit -m '커밋메시지'
```

```bash
$ git add .
$ git commit -m 'Add files'
[master (root-commit) 30a394f] Add files
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt
 create mode 100644 b.txt

```

커밋 이력을 확인하기 위해서는 아래의 명령어를 활용한다. 

```bash
$ git log
commit 30a394f7d7396a71685826819332641977b26c22 (HEAD -> master)
Author: sanga327 <dltkddk327@naver.com>
Date:   Mon Dec 9 14:25:13 2019 +0900

    Add files
    
$ git status
On branch master
nothing to commit, working tree clean  # 작업 공간 clean ->모든 이력이 저장되었으므로 
```

이후 변경사항이 발생하게 된다면, `add` -> `commit`을 한다. 

`add`: 커밋할 대상 파일 선정

`commit`: 이력의 확정

## 원격 저장소(remote repository) 활용하기

> 원격 저장소를 제공해주는 서비스는 다양하다. 
>
> 우리는 github을 기준으로 활용해보겠다.

### 0. 기본 설정

Github에 저장소(repository) 생성

### 1. 원격 저장소 설정

```bash
$ git remote add origin https://~
```

원격 저장소(`remote`)을 `origin`이라는 이름으로 `https://~`을 url로 설정한다. 

아래의 명령어를 통해서 저장된 원격 저장소 목록을 확인할 수 있다. 

```bash
$ git remote add origin https://github.com/sanga327/gittest.git
$ git remote -v
origin  https://github.com/sanga327/gittest.git (fetch)
origin  https://github.com/sanga327/gittest.git (push)
```

혹시 잘못 설정되었다면 아래의 명령어를 통해 삭제 가능하다. 

```bash
$ git remote rm origin
$ git remote -v
```

### 2. push

원격 저장소에 업로드하기 위해서는 `push`명령어가 필요하다 

```bash
$ git push origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 212 bytes | 212.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/sanga327/gittest.git
 * [new branch]      master -> master

```

`origin`으로 설정된 원격 저장소에 `push`한다. 

이후에 변경된 사항(`commit`)이 발생하였을 때, `git push origin master`명령어를 통해서 매번 업로드를 해줄 수 있다. 