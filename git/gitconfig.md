[GITCONFIG]

로컬 세팅이 우선 적용되며, 로컬에 없는 값들은 글로벌 세팅이 적용됨

- global
```
~/.gitconfig
```
- local
```
/repository/.git/config
```

[GIT RESPOSITORY]

- https
https 로 깃을 클론 받으면 별도의 인증없이 push pull commit 등이 가능

- git (ssh)
git 으로 클론을 받으면 ssh 키 인증이 필요하여, 계정에 ssh-key.pub 을 설정해두고, `~/.ssh` 에 private 키가 생성되어 있어야함.