# 젠킨스 패스워드 파라미터

젠킨스의 패스워드 파라미터에 기본값(Default value)을 설정할 경우 이는 젠킨스 서버의 config.xml 파일에 암호화 되어 저장되어진다.
(정확한 경로는  /var/lib/jenkins/jobs/$!{GROUP_NAME}/jobs/{JOB_NAME}/config.xml)
이때 사용되어지는 암호화 방법은 ``AES-128-ECB `` 알고리즘으로 생성되어 진다. 

```
Jenkins uses AES-128-ECB for all its encryptions.  
It basically uses the master.key file to encrypt the key stored in hudson.util.Secret file. 
This key is then used to encrypt the password in credentials.xml.
```

참고 링크 :
- [스택오버플로우](https://stackoverflow.com/questions/25547381/what-password-encryption-jenkins-is-using)
- [Credential storage in jenkins](http://xn--thibaud-dya.fr/jenkins_credentials.html)
- [암호화코드](https://github.com/jenkinsci/jenkins/blob/9c443c8d5bafd63fce574f6d0cf400cd8fe1f124/core/src/main/java/hudson/util/Secret.java)
