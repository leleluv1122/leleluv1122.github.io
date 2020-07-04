---
title: "스프링 부트 Jsp JPA Spring Security 인스타그램 따라해보기 (3) "
categories: springboot
comments: true
---

## 실행 환경
 > STS3, MySQL

## 사용언어(환경)
 > Spring boot(JAVA, JSP), JPA, Bootstrap(부트스트랩), 스프링 시큐리티



오늘은 가입창 만들기~

저번에 이미 user를 save되게 하는 코드들은 이미 작성했기 때문에 view랑 controller만 좀 만져주면 회원가입완성..^^

login.jsp를 보면 가입하기에 href로 /guest/register 가 있는걸 볼 수 있다. 아직은 error가 뜰것이다

```html
<a href="/guest/register" class="btn btn-dark">가입하기</a>
```

controller로 가서 /guest/register에 연결이 되게 하는 코드를 작성해볼게요

## GuestController.java
 파일위치: outstagram/src/main/java/out/stagram/controller

```java
@RequestMapping(value = "guest/register", method = RequestMethod.GET) // url에 접속을 하는 코드
public String register(UserRegistrationModel userModel, Model model) throws Exception{ 
	// userModel이 있어야 form을 만들고 나중에 post로 전송가능
	return "/guest/register";
}
```

화면 자체랑 연결이 되었으니 view를 생성해보러 gogo

## register.jsp
 파일위치: outstagram/src/webapp/WEB-INF/views/guest

 먼저 화면에 사진을 띄워둔다

```html
<div class="layer">
	<img src="/images/insta_register.JPG">
</div>
```

```html
<div>
	<form:form method="post" modelAttribute="userRegistrationModel" autocomplete="off"> <!-- 자동으로 완성해주는 기능 없애기 off -->
		<div class="form-group">
			<form:input path="phone" class="form-control w300" placeholder="휴대폰 번호" />
			<form:errors path="phone" class="error" />
		</div>
		<div class="form-group">
			<form:input path="name" class="form-control w300" placeholder="성명" />
			<form:errors path="name" class="error" />
		</div>
		<div class="form-group">
			<form:input path="userid" class="form-control w300" placeholder="사용자 이름" />
			<form:errors path="userid" class="error" />
		</div>
		<div class="form-group">
			<form:password path="passwd1" class="form-control w300" placeholder="비밀번호" />
			<form:errors path="passwd1" class="error" />
		</div>
		<button type="submit" onclick="return confirm('회원가입 하시겠습니까?')" class="btn">가입</button>
	</form:form>
	<div>
		<a href="../login">로그인</a> <!-- 이미 계정이 있는데 가입하기를 잘못 누른경우 로그인 url로 이동가능 --> 
	</div>
</div>
```

자 이렇게 form도 생성해봤으니... post로 전송하는 코드를 작성해야겠음.. 다시 guestController로 기기


## GuestController.java
 파일위치: outstagram/src/main/java/out/stagram/controller

```java
@RequestMapping(value = "guest/register", method = RequestMethod.POST) // 위에 register랑 같은 url이고 이름이지만 post여서 중복가능
public String register(@Valid UserRegistrationModel userModel, BindingResult bindingResult, Model model) throws Exception{
	if (userService.hasErrors(userModel, bindingResult)) {
		return "/guest/register"; // 문제가 있다면 가입창다시 띄우기
	}
	userService.save(userModel); 
	// 에러가 없다면 정상적으로 usermodel객체를 userservice로 보내서 user객체로 바꾼뒤 user 테이블에 저장함
	return "redirect:/guest/login"; 
	// 모두 종료후 로그인 페이지로 redirect함
}
```


![회원가입창](../../../assets/I-9.JPG)

![회원가입창2](../../../assets/I-10.JPG)

![db저장이미지](../../../assets/I-11.JPG)

저장이 잘 된 모습을 볼 수 있다.. 또한 회원가입버튼을 누르면 정상적으로 될 경우 로그인페이지로 redirect되는 모습을 볼 수 있다.

또한 회원가입에 에러가 발생할 경우 당연히 db에는 저장이 되지않고, 에러 문구가 나옴
 
![error페이지](../../../assets/I-12.JPG)


이렇게 로그인을 할 수 있는 아이디를 생성했다~ 로그인해보면 아직 그 다음페이지는 만들지 않아서 오류가 발생하지만 로그인 자체는 된 모습을 볼 수 있다.

오늘은 여기까지하면서... 오늘의 추천곡은 아이즈원 - 해바라기  진짜 갓띵곡★