## 들어가며

Out of 311의 첫번째 스터디 주제는 **git** 사용하기 입니다. 본 포스팅은 **git**을 처음 시작할 때 겪는 어려움과 그 해결과정을 공유하고자 작성되었습니다. 구성은 다음과 같습니다.

1) [git pull origin master 코드 의미 이해](#git-pull-origin-master-코드-의미-이해)
2) [git branch 생성](#git-branch-생성)
   1) [git branch, git checkout](#git-branch,-git-checkout)
   2) [branch push 과정에서의 이슈 & 해결 과정](#branch-push-과정에서의-이슈-&-해결-과정)
   3) [pull request 과정에서의 이슈 & 해결 과정](#pull-request-과정에서의-이슈-&-해결-과정)



## 자주 사용하는 Git 명령어 이해

지난 시간의 내용을 recap한 뒤 과제를 수행하기 위해 branch를 생성하고자 하였습니다.

두괄식으로 개요 설명하기

### git pull origin master 코드 의미 이해

첫 시간에는 git의 기초 중의 기초를 배웠습니다. 그 중 remote repository에 있는 파일들을 local repository로 내려받기 위해서는 **git pull origin master** 코드를 써야했습니다.

본 내용에 대한 recap으로 해당 코드가 가진 의미를 찾아보고자 하였습니다.

- `git pull {query}`, `git push {qeury}` 에서 `{query}`에 들어가는 단어들의 의미는 다음과 같습니다.

- `origin` : 원격 저장소의 이름.

  따라서, `git pull origin`은 우리가 사전에 clone한 repository의 full name을 대신하는 이름(alias) 이라고 할 수 있음.

- `master` : branch 이름.

다음과 같이 사용해 볼 수 있습니다.

```bash
git push orign master
# orgin:
# master:
```



### git branch 생성

git branch, git checkout

```bash
git branch
```

현재 속한 branch 이름을 알고 싶을 때 위의 명령어를 사용

``` bash
git branch mhk
git checkout mhk
git checkout -b mhk
```

나의 branch를 만들고 싶으면 위의 명령어를 사용그런데 branch를 만든다고 해서 바로 그 branch로 생성이 되는 것이 아님. 따라서 checkout으로 해당 branch로 이동해야 함

checkout 뒤에 -b를 붙이고 branch 이름을 명명하면 해당 branch를 생성하고 그 branch로 바로 이동함

#### branch push 과정에서의 이슈 & 해결 과정

- remote repository인 origin에 mhk branch를 등록시키기.

  ```bash
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



## Keyword 정리

위에서 설명한 내용들을 키워드로 정리하면 다음과 같습니다.

- **저장소 (Repository)** : 작업자가 변경한 모든 내용을 추적하는 공간
- **작업 트리 (Working Tree)** : 저장소를 어느 한 시점을 바라보는 작업자의 현재 시점이다.
- **체크 아웃 (Checkout)** : 작업자의 작업트리를 저장소의 특정 시점과 일치 하도록 변경하는 작업
- **스테이징 영역** : 저장소에 커밋하기 전에 커밋을 준비하는 위치 (변경사항을 적용하기 전에 한번 더 변경사항을 정리하고 다듬을수 있는 기회를 제공한다. 변경사항을 추가하기 위해서는 git add 를 사용한다. 커밋 예정인 변경사항이 있다고 보면 된다.)
- **브렌치(branch)** : 브랜치라는 것은 하나의 개발 라인을 의미 한다.
한개의 프로젝트에서도 여러개의 개발 라인이 존재 할 수 있다. 가장 기본이 되는 master branch에서 버그 수정이나 특정 기능을 추가하기 위해서 개발라인을 따로 두고 작업할 수 있다.
이러한 브랜치에는 HEAD(branch head)라는 것이 있는데 이는 한개의 브랜치 내에서 가장 최근에 커밋이 된 reference이다. 예를 들면 branch apple에 3개의 commit이 있는데 이중에 가장 최근에 추가된 커밋이 HEAD가 된다.
- **master** : master 브랜치는 복사해온 저장소 내의 HEAD의 복사본이다.
- **origin** : origin 은 단지 git가 복사해 온 저장소를 가리키기 위해 기본적으로 사용하는 이름



## 오류과정 해결

--> 오류 발생한거 따로 두괄식으로 -->



### FAQ

#### Origin

   원격 저장소의 이름입니다.

​    원격저장소 시간때 원격저장소 추가 명령어는

​    git remote add <이름> <url>로 붙인다고 말씀드렸죠? :)

​    마찬가지로 git remote add origin <url> 형식으로 원격저장소를 추가하거나

​    git clone을 통해 원격저장소를 복사한다면

​    자동으로 origin이라는 이름의 원격저장소가 등록되게 됩니다.



#### master :

   브랜치 중 가장 중심이 되는 기본적인 branch를 master 브랜치라고 부릅니다



#### HEAD :

   현재 내가 어떤 작업공간에 있는지를 나타냅니다.

​    예를 들어 만약 제가 master 브랜치에서 작업을 하고 있다면

​    제 HEAD는 master 브랜치에 있게 되는 것이고,

​    다른 작업을 위해 feature 브랜치를 만들어줬다면

​    제 HEAD는 feature 브랜치에 있게 되는 겁니다 :)



#### 현재 현결된 remote(원격 저장소 표시)

```bash
git remote -v
```



## VS code에서 GitHub 사용하기

위에서 설명한 내용으로

#### Git 사용자 이름, 이메일 설정(global 옵션)

사용자 이름, 이메일 설정

```python
$ git config --global user.name "Your Name"

$ git config --global user.email you@example.com
```



확인

```python
cat ~/.gitconfig
```



#### 브랜치

생성 방법

>1. [Command] +[Shift] + p
>
>2. Git: Create Branch 검색해서 선택
>3. Brach name 입력 후 엔터



깃 전환 방법

```bash
git checkout 브랜치이름
```



어떤 리포지토리로 올라가는지 확인

```python
git remote -v
```





#### VS code Initial Repository

터미널 열기

```python
[Control] + `
```



업로드 할 repository 설정

```python
git remote origin add (repository 주소)
```



확인

```python
git remote -v
```





#### VS code 파일 깃허브에 올리기

1. Commit할 대상 선택

> [+] 버튼 선택

![스크린샷 2021-11-25 오후 2.26.06](/Users/kooseonmin/Library/Application Support/typora-user-images/스크린샷 2021-11-25 오후 2.26.06.png)



2. 체크 버튼 선택

![스크린샷 2021-11-25 오후 2.29.34](/Users/kooseonmin/Library/Application Support/typora-user-images/스크린샷 2021-11-25 오후 2.29.34.png)



3. 커밋 메시지 입력 후 엔터

![스크린샷 2021-11-25 오후 2.31.08](/Users/kooseonmin/Library/Application Support/typora-user-images/스크린샷 2021-11-25 오후 2.31.08.png)



4. 변경 내용 동기화 버튼 선택

![스크린샷 2021-11-25 오후 2.33.49](/Users/kooseonmin/Library/Application Support/typora-user-images/스크린샷 2021-11-25 오후 2.33.49.png)



5. 확인 선택하면 완료!

![스크린샷 2021-11-25 오후 2.34.32](/Users/kooseonmin/Library/Application Support/typora-user-images/스크린샷 2021-11-25 오후 2.34.32.png)





#### VScode 단축키

마크다운 미리보기

```python
[Command] +[Shift] +  V #맥북

[CTRL] + [SHift] + V #윈도우
```



터미널 열기

```python
[Control] + `
```





---

[참고]

[1] [VS code 깃허브 연동](https://velog.io/@blair-lee/VSCode%EC%97%90%EC%84%9C-Github-%EC%97%85%EB%A1%9C%EB%93%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95%EC%A7%B1%EC%89%AC%EC%9B%80%E3%85%8B%E3%85%8B)

[2] 깃 id, email 설정.https://www.lainyzine.com/ko/article/how-to-set-git-repository-username-and-email/

[갓대희의 작은공간 :: [Git (9)\] Git Branch(1) - 기초(Branch 생성 및 사용) (tistory.com)](https://goddaehee.tistory.com/274)

[What is "origin" in Git? - Stack Overflow](https://stackoverflow.com/questions/9529497/what-is-origin-in-git)

[Origin, Master, Head의 의미 - 인프런 | 질문 & 답변 (inflearn.com)](https://www.inflearn.com/questions/27696)

[git clone 사용법: 원격 Git 저장소 복제 - LainyZine](https://www.lainyzine.com/ko/article/git-clone-command/)
