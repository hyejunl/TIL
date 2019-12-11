# Git branch

> Git 개발 흐름에서 branch는 매우 중요하다.
>
> 독립적인 개발환경을 제공하여 동시에 다양한 작업을 진행할 수 있도록 만들어준다.
>
> 일반적으로 브랜치의 이름은 해당 작업을 나타낸다.

## 1. 기초 명령어

```bash
$ git branch # branch 목록 확인
$ git branch {브랜치이름} # {브랜치이름} 생성
$ git checkout {브랜치이름} # {브랜치이름} 으로 이동
$ git branch -d {브랜치이름} # {브랜치이름} 삭제
```

```bash
$ git checkout -b {브랜치이름} # {브랜치이름} 생성 및 이동
```

* branch 병합

```bash
(master) $ git merge feature
# master 브랜치로 feature 브랜치 이력 가져오기 (병합)
```

---

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
   7e5785a (HEAD -> feature/test) test 기능 개발 완료
   926f261 (testbranch, master) Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
   ```


3. master 이동

   ```bash
   $ git checkout master
   ```
   
   ```bash
   $ git log --oneline
   926f261 (HEAD -> master, testbranch) Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
   ```


4. master에 병합

   ```bash
   $ git merge feature/test
   Updating 926f261..7e5785a
   # Fast-forward
    test.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   ```


5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   $ git log --oneline
   7e5785a (HEAD -> master, feature/test) test 기능 개발 완료
   926f261 (testbranch) Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
   ```

   **fast-forward는 그대로 위에 올린다.**

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
   be4df98 (HEAD -> feature/signout) Complete signout
   7e5785a test 기능 개발 완료
   926f261 Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
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
   f8f725c (HEAD -> master) Update master
   7e5785a test 기능 개발 완료
   926f261 Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
   ```

5. master에 병합

   ```bash
   (master) $ git merge feature/signout
   ```

6. 결과 -> 자동으로 *merge commit 발생*

   ```bash
   $ git log --oneline
   211a6d7 (HEAD -> master) Merge branch 'feature/signout'
   f8f725c Update master
   be4df98 (feature/signout) Complete signout
   7e5785a test 기능 개발 완료
   926f261 Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
   ```

7. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   211a6d7 (HEAD -> master) Merge branch 'feature/signout'
   |\
   | * be4df98 (feature/signout) Complete signout
   * | f8f725c Update master
   |/
   * 7e5785a test 기능 개발 완료
   * 926f261 Testbranch -test
   * 77af618 (origin/master) 집 add
   * dcbda41 멀캠 - add txt
   * 2278946 집 - main.html
   * 331cb9e 멀캠 - index.html
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
   ```

   ```bash
   $ git log --oneline
   f0a8ce6 (HEAD -> hotfix/test) hotfix test
   211a6d7 (master) Merge branch 'feature/signout'
   f8f725c Updata master
   be4df98 Complete signout
   7e5785a test 기능 개발 완료
   926f261 Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
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

   ```txt
   # open with code로 파일 열어서 충돌된 부분 어떻게 처리할지 선택
   ```
   
   


7. 충돌 확인 및 해결

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
    
    9.그래프 확인하기
```bash
   $ git log --oneline
   a2977de (HEAD -> master) Merge branch 'hotfix/test'
   81143f2 master test
   f0a8ce6 (hotfix/test) hotfix test
   211a6d7 Merge branch 'feature/signout'
   f8f725c Updata master
   be4df98 Complete signout
   7e5785a test 기능 개발 완료
   926f261 Testbranch -test
   77af618 (origin/master) 집 add
   dcbda41 멀캠 - add txt
   2278946 집 - main.html
   331cb9e 멀캠 - index.html
```


10. branch 삭제

    ```bash
    $ git branch -d hotfix/test
    Deleted branch hotfix/test (was f0a8ce6).
    ```