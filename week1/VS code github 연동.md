# VS code에서 GitHub 사용하기



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

```python
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

![스크린샷 2021-11-25 오후 2.26.06](https://s2.loli.net/2021/12/13/sM1gVT7rwOEJ2Qx.png)



2. 체크 버튼 선택

![스크린샷 2021-11-25 오후 2.29.34](https://s2.loli.net/2021/12/13/lMVbuhfrwQLYg3P.png)



3. 커밋 메시지 입력 후 엔터

![스크린샷 2021-11-25 오후 2.31.08](https://s2.loli.net/2021/12/13/Jim5KwxrpsghTXP.png)







4. 변경 내용 동기화 버튼 선택

![스크린샷 2021-11-25 오후 2.33.49](https://s2.loli.net/2021/12/13/AErhQzDNPFk9sct.png)



5. 확인 선택하면 완료!

![스크린샷 2021-11-25 오후 2.34.32](https://s2.loli.net/2021/12/13/WbfQTtq8G4Rgm3D.png)





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

[1] VS code 깃허브 연동https://velog.io/@blair-lee/VSCode%EC%97%90%EC%84%9C-Github-%EC%97%85%EB%A1%9C%EB%93%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95%EC%A7%B1%EC%89%AC%EC%9B%80%E3%85%8B%E3%85%8B

[2] 깃 id, email 설정.https://www.lainyzine.com/ko/article/how-to-set-git-repository-username-and-email/