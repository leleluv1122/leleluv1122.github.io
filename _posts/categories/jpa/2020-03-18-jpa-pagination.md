---
title: "JPA pagination 구현"
categories: jpa
comments: true
---

# 1) 기초지식
## 1) 액션 메소드의 파라미터 객체
 액션 메소드의 파라미터가 객체일 경우에, 다음과 같은 일이 자동으로 일어난다.
  - 그 객체의 set 메소드가 호출되어 request parameter 데이터가 자동으로 채워진다. 이때 set 메소드 이름과 request parameter 데이터 이름이 일치해야 한다.

    예: pg = 3  / void setPg(int value);

  - 그 객체가 모델에 자동으로 채워진다. 이때 모델 데이터 이름은, 변수명이 아니고 그 클래스 이름이다. (첫 글자만 소문자로 바꿈)

    예: 
```java
String actionMethod(Pagination p) {
    model.addAttribute("pagination", p)
    . . .
}
```

## 2) interface의 default method
 Java 8에서 추가된 문법 중의 하나는, interface에 default 메소드를 구현할 수 있는 것이다. 흔히 interface를 설명할 때, 구현할 메소드가 없으면 interface에 default 메소드를 구현한다고 설명했는데, default 메소드 때문에 그 설명은 틀린 것이 되었다.
 - 요약
```
	abstract method	구현된 메소드	멤버 변수     static final 멤버 변수
class	        X	       O	   O	               O
abstract class	O	       O	   O	               O
interface	O	       O	   X	               O
```
 이제 interface에서 할 수 없는 것은 instance멤버 변수를 만들 수 없다는 점 뿐이다. 그래서 interface에 생성한 멤버 변수는 전부 자동으로 public static final 멤버 변수가 된다.

