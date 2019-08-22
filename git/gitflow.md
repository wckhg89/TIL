# GIT feature -> develop 머지

```
$ git checkout -b feature/TASK-001 develop
$ git add .
$ git commit -m 'Add -TASK-001'
$ git checkout develop
$ git merge --no-ff feature/TASK-001
1 Merge branch 'feature/TASK-001' into develop
2
3 # Please enter a commit message to explain why this merge is necessary,
4 # especially if it merges an updated upstream into a topic branch.
5 #
6 # Lines starting with '#' will be ignored, and an empty message aborts
7 # the commit.
:wq!

$ git push origin develop
```

--no-ff 옵션
새로운 커밋 객체를 만들어 ‘develop’ 브랜치에 merge 한다.
이것은 ‘feature’ 브랜치에 존재하는 커밋 이력을 모두 합쳐서 하나의 새로운 커밋 객체를 만들어 ‘develop’ 브랜치로 병합(merge)하는 것이다.
