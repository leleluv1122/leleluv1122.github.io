---
title: "Consider defining a bean of type 'net.lele.service.CategoryService' in your configuration."
categories: error
comments: true
---

오류가 뜨면 일단 무섭다.... 다 맞았는데 대체 뭐가틀린겨ㅁ_ㅁ

```
Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
2020-03-04 22:28:44.040 ERROR 12988 --- [  restartedMain] o.s.b.d.LoggingFailureAnalysisReporter   : 

***************************
APPLICATION FAILED TO START
***************************

Description:

Field categoryService in net.lele.controller.ShopController required a bean of type 'net.lele.service.CategoryService' that could not be found.

The injection point has the following annotations:
	- @org.springframework.beans.factory.annotation.Autowired(required=true)


Action:

Consider defining a bean of type 'net.lele.service.CategoryService' in your configuration.
```

service에 문제가 생겼다구 하니 한번 가보는데 아무문제가 없어보였다...

근데 고려해라 빈타입을 정의? 뭐래머래 서비스 어쩌구 하길래 설마하구 CategoryService를 보니

service에는 항상 꼭... `@Service` 를 붙여줘야 되는데 까묵었나보당... 바보그자체

바로 해결해버렸지모얌,,, 이런 기본적인 실슈슈슈ㅠㅠㅋㅋㅋ

바보가 따로업다구!!! 배고프니 내일은 버거킹 먹어야겠다...