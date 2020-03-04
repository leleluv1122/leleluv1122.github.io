---
title: "Path with WEB-INF or META-INF: [WEB-INF/views/shop/index.jsp] "
categories: error
comments: true
---

Path with "WEB-INF" or "META-INF": [WEB-INF/views/shop/index.jsp]

새로운 프로젝트생성하구 셋팅하는데 실행했더니 이런 오류가 났다....

왜그런가 구글링끝에 답은 넘 간단...ㅠ

```java
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-taglibs</artifactId>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>
```

pom.xml에다가 아파티 톰켓도 해야된다...ㅎㅎㅎ 

톰캣안써서 안해도 되는줄 알았더니 아니구먼 

실행했더니 잘 된다!! 

