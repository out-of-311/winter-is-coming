## 들어가며

Out of 311의 첫번째 스터디 주제는 **git** 사용하기 입니다. 본 포스팅은 **git**을 처음 시작할 때 겪는 어려움과 그 해결과정을 공유하고자 작성되었습니다. 구성은 다음과 같습니다.

1) [git pull origin master 코드 의미 이해](#git-pull-origin-master-코드-의미-이해)
2) [git branch 생성 및 push](#git-branch-생성-및-push)
   1) [git branch, git checkout](#git-branch,-git-checkout)
   2) [pull request 과정에서의 이슈 & 해결 과정](#pull-request-과정에서의-이슈-&-해결-과정)
   3) [branch push 과정에서의 이슈 & 해결 과정](#branch-push-과정에서의-이슈-&-해결-과정)



## 자주 사용하는 Git 명령어 이해

지난 시간의 내용을 recap한 뒤 과제를 수행하기 위해 branch를 생성하고자 하였습니다. 과제의 내용은 다음과 같습니다.

>  본인 브랜치를 만들어서, README에 본인 이름적고, 역할 써오기



### git pull origin master 코드 의미 이해

첫 시간에는 git의 기초 중의 기초를 배웠습니다. 그 중 remote repository에 있는 파일들을 local repository로 내려받기 위해서는 **git pull origin master** 코드를 써야했습니다.

본 내용에 대한 recap으로 해당 코드가 가진 의미를 찾아보고자 하였습니다.

- `git pull {query}`, `git push {qeury}` 에서 `{query}`에 들어가는 단어들의 의미는 다음과 같습니다.

- `origin` : 원격 저장소의 이름.

  따라서, `git pull origin`은 우리가 사전에 clone한 repository의 full name을 대신하는 이름(alias) 이라고 할 수 있습니다.

- `master` : branch 이름을 의미합니다. master는 어떤 reopository에서 가장 중심이 되는 branch의 이름입니다.

따라서 우리는 `git pull origin {query}`가 작동할 수 있도록 `{query}`에 들어갈 branch를 생성해야 함을 알 수 있습니다.



### git branch 생성 및 push

#### git branch, git checkout

자신의 branch를 생성하기 위한 작업을 진행하였습니다. 

먼저 git bash terminal에서 현재 작업중인 branch의 이름을 확인합니다. 현재 HEAD가 가리키는 branch 이름을 알고 싶을 때 아래의 명령어를 사용합니다.

```bash
git branch
* master
```

이와 같이 명령어를 사용하면 `* master` 출력물이 나옵니다. 이는 HEAD가 현재 master  branch에 위치하고 있음을 알려줍니다.

이제 자신의 branch를 만들도록 하겠습니다. 명훈은 branch의 이름을 mhk로 설정하였습니다.

``` bash
git branch mhk
* master
  mhk
```

출력물을 보면 mhk라는 branch가 새로 생긴것을 알 수 있습니다. 하지만 아직 우리의 terminal은 master를 가리키고 있음을 알 수 있습니다. 따라서 HEAD가 바라보고 있는 branch를 변경해야 합니다.

```bash
git checkout mhk
  master
* mhk
```

`git checkout {query}` 명령어를 사용하면 `{query}`에 해당하는 branch로 HEAD가 이동하게 됩니다. 우리는 비로소 주어진 과제에서 **'' 본인 브랜치를 만들어서 ''** 부분을 완료하게 되었습니다.

한편 branch를 생성하고 생성한 branch로 바로 이동하고자 할 때는 아래의 명령어를 입력하시면 됩니다.

```bash
git checkout -b mhk
```

이렇게 원하고자 하는 branch를 만들어봤습니다. 그러나 지금까지의 모든 작업은 local에서 진행되었습니다. 즉, remote repository에는 새로운 branch가 만들어지지 않았겠죠. 이제 remote repository에도 등록될 수 있도록 `git push`를 해봅시다.



#### pull request 과정에서의 이슈 & 해결 과정

우리에게는 아직 **"README에 본인 이름적고, 역할 써오기"** 과제가 남아있습니다. 수정해야 하는 README.md 파일이 존재해야 하므로 master branch에서 `pull`을 먼저 하겠습니다. 그런데 다음과 같은 error가 발생하였습니다.

```bash
git pull origin master
From https://github.com/out-of-311/winter-nlp-study
 * branch            master     -> FETCH_HEAD
CONFLICT (add/add): Merge conflict in test.md
Auto-merging test.md
Automatic merge failed; fix conflicts and then commit the result.
```

현재 local에 있는 test.md 파일과 master branch에 있는 test.md 파일과 달라서 충돌이 발생했다고 합니다. 따라서 해당 파일을 적절히 수정하고 commit 한 다음에 pull을 진행해보겠습니다.

```bash
<<<<<<< HEAD
충돌유발- new_branch
=======
충돌유발- master
>>>>>>> master
```

merge된 test.md 파일은 다음과 같이 작성되어 있습니다. 어떤 자료만을 남길지는 본인의 선택입니다. 저는 local에 있었던 제 파일만을 남기고 싶으므로 master에서 해당한 모든 부분을 지웠습니다. 이후 git commit을 진행하였습니다.

```bash
git add test.md
git commit -m {query}
```

이제 우리는 master에 있는 README.md 파일을 정상적으로 받을 수 있게 되었습니다. 



#### branch push 과정에서의 이슈 & 해결 과정

READEME.md 파일을 수정했다고 가정하고 mhk branch를 remote repository로 `push` 해봅시다.

```bash
git push
fatal: The current branch mhk has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin mhk
```

그런데 `git push`를 하게 되면 위와 같은 error message가 뜨면서 명령어가 정상작동하지 않습니다. error 구문을 보니 현재 로컬에 존재하는 mhk branch가 upstream branch로 존재하지 않다고 말하고 있습니다. 

한편 error 구문에서 `git push --set-upstream origin mhk` 명령어를 사용해야 함을 추천해주고 있습니다. 왜 이 명령어가 우선적으로 수행되어야 하는지는 다음의 내용이 잘 설명해주고 있습니다.


> When you push to a remote and you use the `--set-upstream` flag git sets the branch you are pushing to as the remote tracking branch of the branch you are pushing.
>
> Adding a remote tracking branch means that git then knows what you want to do when you `git fetch`, `git pull` or `git push` in future. It assumes that you want to keep the local branch and the remote branch it is tracking in sync and does the appropriate thing to achieve this.
>
> You could achieve the same thing with `git branch --set-upstream-to` or `git checkout --track`. See the git help pages on [tracking branches](http://git-scm.com/book/en/Git-Branching-Remote-Branches#Tracking-Branches) for more information.

이는 현재 local에 있는 mhk를 remote repository의 mhk와 동기화시키기겠다는 의미입니다. 해당 명령어를 실행해줍시다.

```bash
git push --set-upstream origin mhk
remote: Permission to out-of-311/winter-nlp-study.git denied to Metalchaos8527.
fatal: unable to access 'https://github.com/out-of-311/winter-nlp-study.git/': The requested URL returned error: 403
```

한편 저에게는 위와 같은 에러가 발생하였습니다. 해당 에러의 원인은 다양했지만 저의 경우에는 remote repository에서 제게 쓰기 및 읽기를 할 수 있는 '관리자' 권한을 주지 않아서 발생한 문제였습니다. reposiotry의 관리자가 저에게 권한을 부여한 뒤로는 코드가 정상 작동 하였습니다.



![image-20211208172028477](https://s2.loli.net/2021/12/08/RNcwEYUjeP2fLs8.png)

이렇게 저만의 branch를 만들고 remote repository에 push하는 과정까지 거쳐서 과제를 최종 완료할 수 있었습니다!



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

![스크린샷 2021-11-25 오후 2.26.06](https://s2.loli.net/2021/12/13/WPZNJAVMvGtsbTi.png)



2. 체크 버튼 선택

![스크린샷 2021-11-25 오후 2.29.34](https://s2.loli.net/2021/12/13/6wNUGto3jZygxMF.png)



3. 커밋 메시지 입력 후 엔터

![스크린샷 2021-11-25 오후 2.31.08](https://s2.loli.net/2021/12/13/R8zs6IfiU2mWyCe.png)



4. 변경 내용 동기화 버튼 선택

![스크린샷 2021-11-25 오후 2.33.49](https://s2.loli.net/2021/12/13/uzUSXhAKJP42mDG.png)



5. 확인 선택하면 완료!

![스크린샷 2021-11-25 오후 2.34.32](https://s2.loli.net/2021/12/13/iJtQpv6SVPxf3Uq.png)





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

[git - What does '--set-upstream' do? - Stack Overflow](https://stackoverflow.com/questions/18031946/what-does-set-upstream-do)

[What is "origin" in Git? - Stack Overflow](https://stackoverflow.com/questions/9529497/what-is-origin-in-git)

[Origin, Master, Head의 의미 - 인프런 | 질문 & 답변 (inflearn.com)](https://www.inflearn.com/questions/27696)

[git clone 사용법: 원격 Git 저장소 복제 - LainyZine](https://www.lainyzine.com/ko/article/git-clone-command/)
