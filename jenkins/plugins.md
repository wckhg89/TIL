# 젠킨스 플러그인 정리

### 플러그인 목록

1. 젠킨스 빌드 사용자 정보 조회 플러그인
: [Build User Var Plugin](https://wiki.jenkins.io/display/JENKINS/Build+User+Vars+Plugin)

2. 젠킨스 워크스페이스 클린 플러그인
: [Workspace Clean Plugin](https://wiki.jenkins.io/display/JENKINS/Workspace+Cleanup+Plugin)

3. 젠킨스 크리덴셜 아이디 조회 플러그인
: [Credentials Plugin](https://wiki.jenkins.io/display/JENKINS/Credentials+Plugin)

4. 젠킨스 웹훅 플러그인
: [Generic Webhook Plugin](https://wiki.jenkins.io/display/JENKINS/Generic+Webhook+Trigger+Plugin)
: url 버그 있음 -> curl -vs http://localhost:8080/~jenkins~/generic-webhook-trigger/invoke?token=abc123
: [인증이슈참고](https://github.com/janinko/ghprb/issues/48)
