# Git branch 사용법 정리

- [참고 사이트](https://backlog.com/git-tutorial/kr/stepup/stepup1_1.html)를 참고하여 작성

---



## 1. Git branch 생성

- branch 생성

  ```bash
  git branch issue1(브랜치이름)
  ```

- branch 목록 확인

  ```bash
  git branch
  ```



## 2. branch 전환하기

- 우리가 만든 브랜치에서 작업을 수행하려면, 이 브랜치를 사용하겠다고 명시해 주어야 한다. 이 때 사용하는 명령어가 체크아웃(checkout)이다. 

- 사용할 branch 지정(체크아웃)

  ```bash
  git checkout issue1(브랜치이름)
  ```

- 사용할 branch 지정(체크아웃) + 작성

  ```bash
  git checkout -b issue1(브랜치이름)
  ```

- 브랜치를 체크아웃한 상태에서 커밋을 수행하면, 그 branch에 이력이 기록된다. 

  ```bash
  git add textfile.txt
  git commit -m "설명"
  ```

  

## 3. branch 병합하기

- 변경사항 main 브랜치에 병합

  - 지정한 commit 내용을 branch에 넣기 

    ```bash
    git merge 병합할commit이름
    ```

  - main에 해당 브랜치내용을 넣기 위해서는 main 브랜치에 HEAD 가 위치하도록 해야 한다. 

  - checkout 명령어를 이용하여 현재 사용중인 branch를 main으로 전환한다. 

    ```bash
    git checkout main
    ```

  - 병합

    ```bash
    git merge issue1(병합할branch이름)
    ```

  - 이제 main 브랜치가 가리키는 커밋이 'issue1'과 같은 위치로 이동했다. 이런 방식을 fast-forward 병합이라고 한다. 



## 4. branch 삭제하기

- issue1 브랜치의 내용이 모두 main에 병합되어 더 필요 없게 되었다. 

- 브랜치를 삭제하려면 -d 명령어를 사용하면 된다. 

  ```bash
  git branch -d issue1(삭제할branch이름)
  ```

- 제대로 삭제되었는지 확인

  ```bash 
  git branch
  ```

  - main 브랜치만 목록에 남아 있게 된다. 



## 5. 동시에 여러 작업하기

> 두 개의 브랜치를 생성하여 동시에 여러 작업을 처리하는 상황

- issue2, issue3 브랜치를 만든 후, issue2 브랜치로 전환

  ```bash
  git branch issue2 
  git branch issue3
  git checkout issue2
  git branch
  ```

- issue2 브랜치 아래에 myfile.txt 커밋 후 설명 추가

  ```bash
  git add myfile.txt
  git commit -m "설명"
  ```

- 이번에는 issue3 브랜치로 전환

  ```bash
  git checkout issue3
  ```

- pull 명령어로 변경사항 commit

  ```bash
  git add myfile.txt
  git commit -m "pull 설명"
  ```

  <br>

## 6. 병합할 때 발생하는 충돌 해결하기

>  내용이 달라진 myfile.txt 파일에 대한 충돌을 해결하는법

- issue2 브랜치에서 변경한 부분과 issue3 브랜치에서 변경한 부분을 모두 main 브랜치에 통합

  ```bash
  git checkout main
  git merge issue2
  git merge issue3  -- 충돌 발생
  ```

  - 충돌이 있는 부분에 Git이 자동으로 충돌 정보를 포함하여 파일 내용을 변경한다. 이 내용을 통해 어떤 브랜치에서 어떤 부분이 충돌되었는지 확인할 수 있다. 충돌이 일어난 부분은 일일이 확인해서 수정해 주어야 한다. 파일 내용을 변경 후, 다시 커밋한다. 

  ```bash
  git add myfile.txt
  git commit -m "issue3 브랜치 병합"
  ```

  - 이 병합은 충돌 부분을 수정했기 때문에 그 변화를 기록하는 병합 커밋이 새로 생성되었다. 그리고 main 브랜치의 HEAD가 그 위치로 이동한 것을 확인할 수 있다. 이와 같은 방식을 non fast-forward 병합이라고 한다. 



## 7. rebase로 병합하기

> issue2, issue3 두 개의 브랜치를 하나의 줄기로 만들어 병합하는 방법

- issue3 브랜치를 병합할 때 rebase를 먼저 실행한 후 병합을 시도하면 이력을 하나의 줄기로 만들 수 있다. 

- 이 실습을 위해 이전에서 마지막으로 병합했던 병합 명령 취소 

  ```bash
  git reset --hard HEAD~
  ```

- issue3 브랜치로 전환하여 main 브랜치에 rebase를 실행한다. 

  ```bash
  git checkout issue3
  git rebase main
  ```

- 충돌난 파일을 적절히 변경해준다. 

- rebase의 경우 충돌 부분을 수정한 후에는 commit이 아니라 rebase 명령에 --continue 옵션을 지정해 실행해야 한다. 

  ```bash
  git add myfile.txt
  git rebase --continue
  ```

- 만약 rebase 자체를 취소하려면 --abort 옵션을 지정하면 된다. 

- 이전 튜토리얼에서 merge 명령어를 사용했을 때처럼, rebase만 실행한 경우에는 issue3 브랜치가 두 브랜치(main, issue2)의 앞쪽으로 옮겨졌을 뿐, main 브랜치에는 아직 issue3의 변경사항이 적용되지 못한 상태이다. 이제 main 브랜치로 이동하여 issue3 브랜치의 변경사항을 병합할 차례이다.

  ```bash
  git checkout main
  git merge issue3
  ```

  - myfile.txt의 최종 내용은 merge 때와 동일하지만, 그 이력은 달라진다. 









