# 젠킨스 파이프라인 삽질기

##젠킨스 파이프 라인 삽질기 (1) - 파이프 라인 설정 관련

[사용한 플러그인들]
- user build var plugin : 카톡 알람보낼때 어떤 사용자가 빌드했는지 알기 위해서 젠킨스 잡을 실행시킨 사용자 값을 얻어오는 플러그인으로 사용됨.
https://wiki.jenkins.io/disp…/JENKINS/Build+User+Vars+Plugin

- Workspace clean plugin : 젠킨스 워크스페이스를 한번 비워줌 (``var/lib/jenkins/workspace/job_name``)

groovy 에서는 cleanWs() 함수를 호출하면 됨
https://wiki.jenkins.io/di…/JENKINS/Workspace+Cleanup+Plugin

- Pipeline plugin : 젠킨스 파이프라인을 사용하기 위해 씀
https://wiki.jenkins.io/display/JENKINS/Pipeline+Plugin

- Credentials Plugin : 깃 인증을 위해 사용함.
https://wiki.jenkins.io/display/JENKINS/Credentials+Plugin

[깃 소스 체크아웃]
- 소스 체크아웃 위해서는 credentailId를 설정해주어야 함. (인증을 위해 사용됨 -> jenkins credentials에서 추가해준 값을 사용해서 인증해야함)

(추가한 인증값은 ``/var/lib/jenkins/credential.xml`` 에 등록됨)

[메이븐 빌드]
- 메이븐 빌드를 위한 자바와 메이븐은 Jenkins 관리의 'Global Tool Configuration' 에서 미리 세팅해 주어 사용해야함.

groovy 스크립트에서 사용할때는 아래와 같이 사용함.

"${tool '{글로벌 툴에 세팅한 이름}'}"
ex)
- Java8
"${tool, 'Java8'}"
- Maven
"${tool, 'MAVEN'}"

(일단 기억나는건 이정도..?)

## 젠킨스 파이프 라인 삽질기 (2) - 그루비 인터폴레이션

카톡 API 호출하는 부분을 함수로 빼려는데 함수 인자를 문자열에 넣으려고 하는데 잘 안들어가는 삽질을 했는데 문서를 읽어보니 아래와 같 문제였다.

[그루비 인터폴레이션]
그루비에서는 문자열을 'test', "test" 등 싱글 쿼테이션 혹은 더블 쿼테이션으로 표현이 가능하다고 함.

- 싱글 쿼테이션으로 표기시 : java.lang.String 오브젝트로 인식되어 인터폴레이션이 적용이 안됨.

- 더블 쿼테이션으로 표기시 : groovy.lang.GString 오브젝트로 인식되어 인터폴레이션이 적용됨.

결론 인터폴레이션을 쓰려면 더블쿼테이션으로 써야함

ex)
```
def name = 'Guillaume' // a plain string
def greeting = "Hello ${name}"

assert greeting.toString() == 'Hello Guillaume'
```
cf)
``
Interpolation is the act of replacing a placeholder in the string with its value upon evaluation of the string.

The placeholder expressions are surrounded by ${} or prefixed with $ for dotted expressions.
``

http://groovy-lang.org/syntax.html


## 젠킨스 파이프 라인 삽질기 (3)

얼추 작업하던 것이 완료 된듯... (리뷰 필요...)
정리하면...
파이프 라인은 아래와 같음

1) 소스 체크아웃 
- 소스 체크아웃이야 깃에서 소스 받아오는거니 특별한 작업은 없음.

- 주의 해야할 것은 젠킨스 groovy 파이프라인에서 소스 체크아웃을 위해서는 CRENTAILS_ID가 필요함 이는 젠킨스 credentials에서 등록 및 수정할 수 있음 (아마 사용하는 플러그인은 https://wiki.jenkins.io/display/JENKINS/Credentials+Plugin
이거인것 같음)

- 그리고 두번째로 주의할 점은 체크아웃 전에 기존 잡이 작업했던 내용을 삭제하기 위해 워크스페이스(/var/lib/jenkins/jobs/{JOB_NAME}/workspce)를 한번 비워줘야 함.
사용한 플러그인 https://wiki.jenkins.io/di…/JENKINS/Workspace+Cleanup+Plugin 요거임)

2) 메이븐 빌드
- 체크아웃 받은 스프링 메이븐 프로젝트를 빌드해야함.

- 이때 젠킨스 서버에 자바, 메이븐이 설치 되어야 있어야 하는게 당연함 그런걸 편하게 관리해주기 위해 젠킨스에서는 Global Tool Configuration 이라는 것을 지원함 (해당 내용 https://shortstories.gitbooks.io/…/jenkins_pipeline_c0bd_c9… 이쪽 사이트를 참고했음)

3) 도커 이미지 생성 및 DCOS 배포 
- 도커 이미지를 만들고 (Dockerfile에 작성한 스팩으로) 이 이미지를 도커 레파지 토리에 올린 후에 마라톤 API를 호출해서 DCOS 에 배포하는 역할을 함. (여기서 고민은 도커 레파지토리를 어떤식으로 프로덕션과 개발을 잘 계층화 할지임 경험한 바로는 존재하지 않는 레파지토리면 이미지 푸시가 안됫던거 같음)

- 마라톤 API 스팩은 http://mesosphere.github.io/marathon/api-console/index.html 해당 문서를 참고했음.

- 아직 해결하지 못한게 가끔 메소스 배포가 실패하는데 이때 예외처리를 어떻게 해야할지 모르겠음...

4) 사내 DNS 브랜치 이름으로 도메인 등록
- 브랜치 이름을 적당한 도메인 명으로 바꾸어서 사내 DNS에 등록하는 API 를 호출 하는 역할을함 딱히 어려운 작업이 없는 파이프라인이었음.

- 추가적인 작업은 아파치 프록시 타도록 와일드카드 써서 서버얼라이언스 세팅이 필요했음
