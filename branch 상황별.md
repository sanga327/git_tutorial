### 상황 1. fast-foward

![image-20191210133651103](images/image-20191210133651103.png)

1. feature/test branch 생성 및 이동

   ```bash
   $ git checkout -b feature/test
   ```

   

2. 작업 완료 후 commit

   ```bash
   $ touch test.txt
   $ git add .
   $ git commit -m 'test 기능 개발 완료'
   ```
   
   ```bash
   $ git log --oneline
   7c4f55f (HEAD -> feature/test) test 기능 개발 완료
   a7207ab (testbranch, master) Testbranch -test
   823cb3d (origin/master) lunch in mul
   cbb34ac  집 - a.txt 추가
   6f9091b 집 - main.html
   62afb8d 멀캠 - index.html
   ```
   
   


3. master 이동

   ```bash
   $ git checkout master
   ```
   
   ```bash
   $ git log --oneline
   a7207ab (HEAD -> master, testbranch) Testbranch -test
   823cb3d (origin/master) lunch in mul
   cbb34ac  집 - a.txt 추가
   6f9091b 집 - main.html
   62afb8d 멀캠 - index.html
   ```
   
   


4. master에 병합

   ```bash
   $ git merge feature/test
   Updating a7207ab..7c4f55f
   Fast-forward
    test.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   ```
   
   


5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   $ git log --oneline
   7c4f55f (HEAD -> master, feature/test) test 기능 개발 완료
   a7207ab (testbranch) Testbranch -test
   823cb3d (origin/master) lunch in mul
   cbb34ac  집 - a.txt 추가
   6f9091b 집 - main.html
   62afb8d 멀캠 - index.html
   ```

   

6. branch 삭제

   ```bash
   $ git branch -d feature/test
   ```
   
   

---

### 상황 2. merge commit

![image-20191210134933777](images/image-20191210134933777.png)

![image-20191210135323486](images/image-20191210135323486.png)

![image-20191210135454742](images/image-20191210135454742.png)

> feature branch 에서 작업하고 있는 동안, master branch에서 이력이 추가적으로 발생한 경우 

1. feature/signout branch 생성 및 이동

   ```bash
   $ git checkout -b feature/signout
   ```

   

2. 작업 완료 후 commit

   ```bash
   $ git touch signout.txt
   $ git add .
   $ git commit -m 'Complete signout'
   ```

   ```bash
   $ git log --oneline
   bd6580f (HEAD -> feature/signout) Complete signout
   7c4f55f test 기능 개발 완료
   a7207ab Testbranch -test
   823cb3d (origin/master) lunch in mul
   cbb34ac  집 - a.txt 추가
   6f9091b 집 - main.html
   62afb8d 멀캠 - index.html
   ```

   

3. master 이동

   ```bash
   $ git checkout master
   ```

   

4. *master에 추가 commit 이 발생시키기!!*

   * **다른 파일을 수정 혹은 생성하세요!**

     ```bash
     $ touch master.txt
     $ git add .
     $ git commit
     ```

     ```bash
     $ git log --oneline
     9e80599 (HEAD -> master) Update master
     7c4f55f test 기능 개발 완료
     a7207ab Testbranch -test
     823cb3d (origin/master) lunch in mul
     cbb34ac  집 - a.txt 추가
     6f9091b 집 - main.html
     62afb8d 멀캠 - index.html
     ```

     

5. master에 병합

   ```bash
   (master) $ git merge feature/signout
   Merge made by the 'recursive' strategy.
    signout.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.txt
   
   ```

   

6. 결과 -> 자동으로 *merge commit 발생*

   

7. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   de8b7ec (HEAD -> master) Merge branch 'feature/signout'
   |\
   | * bd6580f (feature/signout) Complete signout
   * | 9e80599 Update master
   |/
   * 7c4f55f test 기능 개발 완료
   * a7207ab Testbranch -test
   * 823cb3d (origin/master) lunch in mul
   * cbb34ac  집 - a.txt 추가
   * 6f9091b 집 - main.html
   * 62afb8d 멀캠 - index.html
   ```

   

8. branch 삭제

   

---

### 상황 3. merge commit 충돌

1. feature/board branch 생성 및 이동

   ```bash
   $ git checkout -b hotfix/test
   ```

   

2. 작업 완료 후 commit

   ```bash
   # 직접 test.txt 파일 수정
   $ git add .
   $ git commit -m 'hotfix test'
   ```
   
   ```bash
   $ git log --oneline
   39b4292 (HEAD -> hotfix/test) hotfix test
   de8b7ec (master) Merge branch 'feature/signout'
   9e80599 Update master
   bd6580f (feature/signout) Complete signout
   7c4f55f test 기능 개발 완료
   a7207ab Testbranch -test
   823cb3d (origin/master) lunch in mul
   cbb34ac  집 - a.txt 추가
   6f9091b 집 - main.html
   62afb8d 멀캠 - index.html
   ```
   
   


3. master 이동

   ```bash
   $ git checkout master
   ```
   
   


4. *master에 추가 commit 이 발생시키기!!*

   * **동일 파일을 수정 혹은 생성하세요!**

   ```bash
   # test.txt 수정
   $ git add .
   $ git commit -m 'master test'
   ```

   

5. master에 병합

   ```bash
   (master) $ git merge hotfix/test
   Auto-merging test.txt
   CONFLICT (content): Merge conflict in test.txt
   Automatic merge failed; fix conflicts and then commit the result.
   ```
   
   


6. 결과 -> *merge conflict발생*

   


7. 충돌 확인 및 해결

   ```bash
   <<<<<<< HEAD
   이건 master에서 수정했어요 !
   충돌나니 ?????
   =======
   hotifx/test 브랜치에서 수정했어요!
   루루루 ㅎㅎㅎ 하이
   >>>>>>> hotfix/test
   ```
   
   ```bash
   $ git status
   On branch master
   Your branch is ahead of 'origin/master' by 6 commits.
     (use "git push" to publish your local commits)
   
   You have unmerged paths.
     (fix conflicts and run "git commit")
     (use "git merge --abort" to abort the merge)
   
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
           both modified:   test.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```
   
   


8. merge commit 진행

    ```bash
    $ git add .
    $ git commit
```
   
   * vim 편집기 화면이 나타납니다.
   
   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.
   
9. 그래프 확인하기

    ```bash
   $ git log --oneline
   64f076a (HEAD -> master) Merge branch 'hotfix/test'
   a387f3a master에서 test.txt 내용 수정함git add .
   3c889c7 (hotfix/test) hotfix 브랜치에서 test.txt 파일 내용 수정함
   39b4292 hotfix test
   de8b7ec Merge branch 'feature/signout'
   9e80599 Update master
   bd6580f (feature/signout) Complete signout
   7c4f55f test 기능 개발 완료
   a7207ab Testbranch -test
   823cb3d (origin/master) lunch in mul
   cbb34ac  집 - a.txt 추가
   6f9091b 집 - main.html
   62afb8d 멀캠 - index.html
    ```
   
   


10. branch 삭제

    ```bash
    $ git branch -d hotfix/test
    Deleted branch hotfix/test (was 3c889c7).
    
    ```

    

