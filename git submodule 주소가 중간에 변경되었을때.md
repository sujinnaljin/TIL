# git submodule 주소가 중간에 변경되었을때

서브모듈 추가하려면 git submodule add 통해서 추가해줌

```
git submodule add git://foo.com/git/lib.git
```

그러면 `.gitmodules` 파일에 서브 모듈에 대한 정보가 써짐

```
[submodule "libfoo"]
	path = include/foo
	url = git://foo.com/git/lib.git
```

근데 만약 이 "libfoo" 라는 서브모듈의 url 이 변경되었다면?!

`.gitmodules` 들어가서 여기 선언된 url 값 변경하면 되잖어~ 라고 생각할 수 있겠지만 변경해도 로컬에서 반영 제대로 안됨!

왜냐?  `.gitmodules` 에 선언된 url 은 **submodule 을 초기화 할때만 사용** 되기 때문에, 로컬의 서브모듈은 여전히 old URL 을 point 하고 있음. 

따라서 `.gitmodules` 를 변경하는거 외에도 아래 두가지 step 이 더 필요함

1. `.git/config` 파일에서도 `oldUrl `로 되어있던 것을 `newUrl` 으로 변경
2. `git submodule sync --recursive` 명령어를 실행

# 참고
- [Git submodules and ssh access](https://stackoverflow.com/questions/6031494/git-submodules-and-ssh-access)
- [How to change the remote repository for a git submodule?](https://stackoverflow.com/questions/913701/how-to-change-the-remote-repository-for-a-git-submodule)
- [git submodule update vs git submodule sync](https://stackoverflow.com/questions/45678862/git-submodule-update-vs-git-submodule-sync)
- [git modules](https://git-scm.com/docs/gitmodules)
- [git configuration](https://git-scm.com/docs/git-config/2.6.7#:~:text=The%20Git%20configuration%20file%20contains,git%2Fconfig%20file.)

