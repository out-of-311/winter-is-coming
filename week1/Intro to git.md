## Intro to git 



### Summary

본 문서는 git을 처음 시작할 때 겪는 어려움과 그 해결과정을 공유하고자 작성되었습니다. 구성은 다음과 같습니다.

1) [git pull origin master 코드 의미 이해](#understanding_git)
2) [git branch 생성](#make_branch)
   1) [git branch, git checkout](#sub_mg_a)
   2) [branch push 과정에서의 이슈 & 해결 과정](#sub_mg_b)
   3) [pull request 과정에서의 이슈 & 해결 과정](#sub_mg_c)

 

### out of 311의 첫 과제

본 프로젝트의 첫 과제로 git branch를 생성하는 과제가 주어졌습니다. 구체적인 과제 내용은 다음과 같습니다.

>  본인 브랜치를 만들어서, README에 본인 이름적고, 역할 써오기



#### git pull origin master 코드 의미 이해

첫 시간에는 git의 기초 중의 기초를 배웠습니다. 그 중 remote repository에 있는 파일들을 local repository로 내려받기 위해서는 **git pull origin master** 코드를 써야했습니다.

본 내용에 대한 recap으로 해당 코드가 가진 의미를 찾아보고자 하였습니다.

- git pull ~~, git push ~~ 에서 ~~에 들어가는 단어들의 의미

  > [What is "origin" in Git? - Stack Overflow](https://stackoverflow.com/questions/9529497/what-is-origin-in-git)
  >
  > [Origin, Master, Head의 의미 - 인프런 | 질문 & 답변 (inflearn.com)](https://www.inflearn.com/questions/27696)
  >
  > [git clone 사용법: 원격 Git 저장소 복제 - LainyZine](https://www.lainyzine.com/ko/article/git-clone-command/)

- origin : 원격 저장소의 이름. 

  우리가 아래와 같이 repository를 clone해서 올 때 내부적으로는 해당 repository의 이름을 이미 origin이라고 명명하였음.

  ```
  git clone https://github.com/out-of-311/winter-nlp-study.git
  ```

  따라서 git pull origin은 우리가 사전에 clone한 repository의 full name을 대신하는 이름(alias) 이라고 할 수 있음.

- master : branch 이름.

  어떤 repository에서 가장 중심이 되는 branch의 이름이 master임. 



#### git branch 생성

지난 시간의 내용을 recap한 뒤 과제를 수행하기 위해 branch를 생성하고자 하였습니다. 

- git branch, git checkout

  > [갓대희의 작은공간 :: [Git (9)\] Git Branch(1) - 기초(Branch 생성 및 사용) (tistory.com)](https://goddaehee.tistory.com/274)

  ```
  git branch
  ```

  - 현재 속한 branch 이름을 알고 싶을 때 위의 명령어를 사용

  ``` 
  git branch mhk
  git checkout mhk
  git checkout -b mhk
  ```

  - 나의 branch를 만들고 싶으면 위의 명령어를 사용
  - 그런데 branch를 만든다고 해서 바로 그 branch로 생성이 되는 것이 아님. 따라서 checkout으로 해당 branch로 이동해야 함
  - checkout 뒤에 -b를 붙이고 branch 이름을 명명하면 해당 branch를 생성하고 그 branch로 바로 이동함

- branch push 과정에서의 이슈 & 해결 과정

  - remote repository인 origin에 mhk branch를 등록시키기. 

    ```
    git push
    
    kang_lp@DESKTOP-IH8F9RR MINGW64 ~/Desktop/out-of-311/winter-nlp-study (mhk)
    $ git push
    fatal: The current branch mhk has no upstream branch.
    To push the current branch and set the remote as upstream, use
    
        git push --set-upstream origin mhk
    ```

  - git push를 처음에 하면 local repository가 remote repository에 연결되지 않은 상태이기 때문에 위와 같은 에러 메세지가 발생함

  - 이에 대한 이유는 다음과 같음

    [git - What does '--set-upstream' do? - Stack Overflow](https://stackoverflow.com/questions/18031946/what-does-set-upstream-do)

    > When you push to a remote and you use the `--set-upstream` flag git sets the branch you are pushing to as the remote tracking branch of the branch you are pushing.
    >
    > Adding a remote tracking branch means that git then knows what you want to do when you `git fetch`, `git pull` or `git push` in future. It assumes that you want to keep the local branch and the remote branch it is tracking in sync and does the appropriate thing to achieve this.
    >
    > You could achieve the same thing with `git branch --set-upstream-to` or `git checkout --track`. See the git help pages on [tracking branches](http://git-scm.com/book/en/Git-Branching-Remote-Branches#Tracking-Branches) for more information.

  - 즉 local repository와 remote repository를 동기화시키기겠다는 의미

  - 그런데 또 다른 error가 발생

    ```
    kang_lp@DESKTOP-IH8F9RR MINGW64 ~/Desktop/out-of-311/winter-nlp-study (mhk)
    $ git push --set-upstream origin mhk
    remote: Permission to out-of-311/winter-nlp-study.git denied to Metalchaos8527.
    fatal: unable to access 'https://github.com/out-of-311/winter-nlp-study.git/': The requested URL returned error: 403
    ```

    - 이 부분은 repository의 개설자가 권한을 안줘서 발생한 문제. 해결 됨 
    - branch 생성 이후 git push --set-upstream origin mhk 하면 정상적으로 branch가 생성됨

- test.md 파일 변경하기

  - 기존에 생성한 test.md 파일을 변경하기. 파일을 수정하기 위해서는 다음의 과정을 거쳐야함
    - git add test.md
    - git commit -m "~"
    - git push

- pull request 과정에서의 이슈 & 해결 과정

  - readme.md 파일을 변경하려면 이전에 master 브랜치에 있는 파일들을 받아서 작업을 수행해야 함

  - git pull origin master 명령어를 실행

    - 다음과 같은  message가 뜸

      ```
      kang_lp@DESKTOP-IH8F9RR MINGW64 ~/Desktop/out-of-311/winter-nlp-study (mhk)
      $ git pull origin master
      From https://github.com/out-of-311/winter-nlp-study
       * branch            master     -> FETCH_HEAD
      CONFLICT (add/add): Merge conflict in test.md
      Auto-merging test.md
      Automatic merge failed; fix conflicts and then commit the result.
      ```

    - 내가 변경한 test.md에서 conflict가 일어남. 즉  origin에서 받은 test.md와 내 로컬에 있는 test.md의 상태가 달라서 일어나는 문제. 내 branch에 있는 test로 유지하고 싶으므로 현재 파일로 merge할 수 있는 방법을 알아야 함

    - 받은 test.md 파일이 다음과 같이 변함. 

      ```
      <<<<<<< HEAD
      내 작성 내용
      
      승준 작성 내용
      ```

    - terminal에 merge를 중단하고 싶으면 git merge --abort를 사용하라고 떠서 merge를 중단. pull 이전의 상태로 되돌아옴

    - 다만 test.md 파일이 pull 하고 변경된 것을 다시 되돌렸기 때문에 변경이 필요하다고 git status에 떠서 add후 commit 하였음

    - 다시 pull origin master 하였으나 동일한 문제 발생.

    - vscode에서 이전 부분 head와 이후 부분이 구분자로 구분되어 있어서 이후 부분을 모두 삭제하고 add, commit 하여서 정상작동하게 함