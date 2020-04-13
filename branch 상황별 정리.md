### 상황 1. fast-foward

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
   de7c1cf (HEAD -> feature/test) test 기능 개발 완료
   f653956 (testbranch, master) Testbranch -test
   97871d5 (origin/master) 집 - main.html
   c1e3b55 멀캠 - index.html
   ```


3. master 이동

   ```bash
   $ git checkout master
   ```
   
   ```bash
   $ git log --oneline
   f653956 (HEAD -> master, testbranch) Testbranch -test
   97871d5 (origin/master) 집 - main.html
   c1e3b55 멀캠 - index.html
   
   ```


4. master에 병합

   ```bash
   $ git merge feature/test
   Updating f653956..de7c1cf
   # Fast-forward
    test.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   
   ```


5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   $ git log --oneline
   de7c1cf (HEAD -> master, feature/test) test 기능 개발 완료
   f653956 (testbranch) Testbranch -test
   97871d5 (origin/master) 집 - main.html
   c1e3b55 멀캠 - index.html
   
   ```

   

6. branch 삭제

   ```bash
   $ git branch -d feature/test
   ```
   
   

---

### 상황 2. merge commit

> feature 브랜치에서 작업하고 있는 동안,
>
> master 브랜치에서 이력이 추가적으로 발생한 경우

1. feature/signout branch 생성 및 이동

   ```bash
   $ git checkout -b feature/signout
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch signout.txt
   $ git add .
   $ git commit -m 'Complete signout'
   
   ```

   ```bash
   $ git log --oneline
   8ed4f94 (HEAD -> feature/signout) Complete signout
   de7c1cf test 기능 개발 완료
   f653956 Testbranch -test
   97871d5 (origin/master) 집 - main.html
   c1e3b55 멀캠 - index.html
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
     $ git commit -m 'Update master'
     ```

     ```bash
     $ git log --oneline
     ee70154 (HEAD -> master) Update master
     de7c1cf test 기능 개발 완료
     f653956 Testbranch -test
     97871d5 (origin/master) 집 - main.html
     c1e3b55 멀캠 - index.html
     
     ```

5. master에 병합

   ```bash
   (master) $ git merge feature/signout
   hint: Waiting for your editor to close
   Merge made by the 'recursive' strategy.
    signout.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.txt
   
   ```

6. 결과 -> 자동으로 *merge commit 발생*

   

7. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   eae24ee (HEAD -> master) Merge branch 'feature/signout'
   |\
   | * 8ed4f94 (feature/signout) Complete signout
   * | ee70154 Update master
   |/
   * de7c1cf test 기능 개발 완료
   * f653956 Testbranch -test
   * 97871d5 (origin/master) 집 - main.html
   * c1e3b55 멀캠 - index.html
   
   ```

8. branch 삭제

   ```bash
   $ git branch -d feature/signout
   ```

---

### 상황 3. merge commit 충돌

1. hotfix/test branch 생성 및 이동

   ```bash
   $ git checkout -b hotfix/test
   ```

2. 작업 완료 후 commit

   ```bash
   # 직접 test.txt 파일 수정
   $ git add .
   $ git commit -m 'hotfix test'
   $ git log --oneline
   bff1eda (HEAD -> hotfix/test) hotfix test
   eae24ee (master) Merge branch 'feature/signout'
   ee70154 Update master
   8ed4f94 (feature/signout) Complete signout
   de7c1cf test 기능 개발 완료
   f653956 Testbranch -test
   97871d5 (origin/master) 집 - main.html
   c1e3b55 멀캠 - index.html
   
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
   (master|MERGING)
   $
   ```


6. 결과 -> *merge conflict발생*

   


7. 충돌 확인 및 해결

   ```
   <<<<<<< HEAD
   마스터 test 긴급 수정!!
   성공했다!!
   =======
   hotfix 브랜치에서 수정
   우아아아!
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
```
   
   * vim 편집기 화면이 나타납니다.
   
   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.
   
9. 그래프 확인하기

    ```bash
   $ git log --oneline
   ef816d8 (HEAD -> master) Merge branch 'hotfix/test'
   25ca62c master test
   bff1eda (hotfix/test) hotfix test
   eae24ee Merge branch 'feature/signout'
   ee70154 Update master
   8ed4f94 (feature/signout) Complete signout
   de7c1cf test 기능 개발 완료
   f653956 Testbranch -test
   97871d5 (origin/master) 집 - main.html
   c1e3b55 멀캠 - index.html
   
   
```


10. branch 삭제

    ```bash
    $ git branch -d hotfix/test
    Deleted branch hotfix/test (was bff1eda).
    
    ```
    