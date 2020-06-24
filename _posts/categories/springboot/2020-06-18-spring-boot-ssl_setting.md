---
title: "Spring boot SSL 설정하기 (security)"
categories: springboot
comments: true
---

간단하지만 확실한 보안 https 설정하기 !

일단 그 프로젝트의 경로를 가진 cmd창을 킨다

```
keytool -genkey -alias spring -storetype PKCS12 -keyalg RSA -keysize 2048 -keystore keystore.p12 -validity 4000
```

 -alias : 처리할 항목의 별칭 이름

 -keyalg : 알고리즘

 -keysize : 키의 비트 크기 

 -keystore : 키 저장소 이름

 -validity : 유효 기간 일 수


이렇게 하고 비밀번호를 입력한 뒤 

```
What is your first and last name?
  [Unknown]: ___
What is the name of your organizational unit?
  [Unknown]: ___
What is the name of your organization?
  [Unknown]: ___
What is the name of your City or Locality?
  [Unknown]:  seoul
What is the name of your State or Province?
  [Unknown]:  seoul
What is the two-letter country code for this unit?
  [Unknown]:  KR
Is CN=___   , OU=___, O=___, L=seoul, ST=seoul, C=KR correct?
  [no]:  y
```

밑줄친 부분은 재량것 입력하자ㅎㅎ
이러면 프로젝트 폴더에는 keystore.p12라는 개인정보교환 파일이 생성되어있다.

이제 application.properties로 가서 밑에 내용을 추가한다
```java
# 키 형식
server.ssl.key-store=keystore.p12
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password=yourpassword
server.ssl.key-alias=spring

# 스프링 시큐리티사용시
security.require-ssl=true
```

이제 완성되었다!  http://localhost:8080 으로 접속해보자 

```
Bad Request
This combination of host and port requires TLS.
```

라는 문구가 뜨며 에러가 뜰것이다!

이제 https://localhost:8080 으로 접속해보자 보안오류가 뜬다.. 

고급으로 들어가서 괜찮다구 해주니
원래 만들어놓았던 페이지가 https로 뜬다!ㅎㅎ

