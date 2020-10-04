---
title: "스프링 부트 Jsp JPA Spring Security 인스타그램 따라해보기 (15) - 메인페이지2"
categories: springboot
comments: true
---

## 실행 환경
 > STS3, MySQL

## 사용언어(환경)
 > Spring boot(JAVA, JSP), JPA, Bootstrap(부트스트랩), 스프링 시큐리티

## 이전포스팅  
<https://leleluv1122.github.io/springboot/spring-boot-instagram-1/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-2/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-3/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-4/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-5/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-6/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-7/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-8/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-9/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-10/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-11/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-12/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-13/>

<https://leleluv1122.github.io/springboot/spring-boot-instagram-14/>

이번에는 메인페이지에서 좋아요를 누를 수 있게 해볼겁니당...  
좀 복잡하게 만들었는데... 내가 생각할 수 있는거에 한계가 이거라서...  
더 조은 방법 있으면 알려주세용ㅎㅎ  
json 으로 넘기고 받는거 하느라 삼일은 걸린듯=_=;;  
어렵다어려워.....ㅜㅜ  

# 메인페이지
 
## main.jsp
 파일위치: src/main/webapp/WEB-INF/views/main.jsp

사진 밑에부분에 추가시켜준다

```html
<div class="bar">
	<div class="heart_${p.id}"></div> <!-- product id별로 다르게 움직여야 되므로 class를 다르게 설정함! -->
</div>
.
.
.

<!-- 이건 만들고 나서 주석풀어주기 안그러면 파일이 없어서 오류발생함! -->
<!-- <%@ include file="main/main_heart.jsp"%> --> 
```

insert랑 delete는 [9번째 게시글](https://leleluv1122.github.io/springboot/spring-boot-instagram-9/) 에서 이미 작성해서 설명이 생략되었습니당~~

## PoU.java
 파일위치: src/main/java/out/stagram/domain/PoU.java

```java
@Data
public class PoU {
	int postid;
	boolean b; // 여기에는 좋아요가 눌렸는지 여부가 들어갈 예졍!!
}
```

## HeartController.java
 파일위치: src/main/java/out/stagram/controller/HeartController.java

```java
// /main page post heart
@RequestMapping("/heart/view")
@ResponseBody
private Map<String, Object> heart_view(@RequestParam String jsonData) throws Exception {
	String userId = SecurityContextHolder.getContext().getAuthentication().getName();
	User u = userService.findByUserId(userId);

	Map<String, Object> m = new HashMap<String, Object>();
	List<Object> lst = list_find(jsonData); // 밑에 list_find함수로 gogo

	List<PoCo> pst = new ArrayList<>();
	for (int i = 0; i < lst.size(); i++) {
		int pid = Integer.parseInt(lst.get(i).toString());
		int cnt = heartService.countByPostId(pid);
		PoCo pc = new PoCo();
		pc.setCnt(cnt);
		pc.setPostid(pid);
		pst.add(pc);
	}
	// pst는 postid와 그 id의 좋아요 개수

	List<PoU> pu = new ArrayList<>();
	for (int i = 0; i < lst.size(); i++) {
		int pid = Integer.parseInt(lst.get(i).toString());
		boolean bb;
		if (heartService.countByPostIdAndUserId(pid, u.getId()) == 1)
			bb = true; // 좋아요가 눌러져있음
		else
			bb = false; // 안눌러져있음
		
		PoU pou = new PoU();
		pou.setB(bb);
		pou.setPostid(pid);
		pu.add(pou);
	}

	m.put("pst", pst); // postid에 따른 like cnt
	m.put("pu", pu); // user가 post에 like를 눌렀는지 여부 boolean으로

	return m;
}

private List<Object> list_find(String jsonData) {
	// 직렬화 시켜 가져온 오브젝트 배열을 JSONArray 형식으로 바꿔준다.
	JSONArray array = JSONArray.fromObject(jsonData);
	List<Map<String, Object>> resendList = new ArrayList<Map<String, Object>>();

	for (int i = 0; i < array.size(); i++) {
		JSONObject obj = (JSONObject) array.get(i);
		Map<String, Object> resendMap = new HashMap<String, Object>();

		resendMap.put("id", obj.get("id"));

		resendList.add(resendMap);
	}

	List<Object> lst = new ArrayList<Object>(); // ㅠ.ㅠ 드디어... id값이 넘어왔다ㅠ.ㅠ

	for (int i = 0; i < resendList.size(); i++) {
		String str = resendList.get(i).toString();
		String restr = str.replaceAll("[^0-9]", ""); // 숫자만 추출되게함
		lst.add(restr);
	}
	return lst;
}
```


## main_heart.jsp
 파일위치: src/main/webapp/WEB-INF/views/main/main_heart.jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<script>
	var param = [];
	
	<c:forEach items="${posting}" var="item">
		var data = {
				id : ${item.id}
		};
		param.push(data);
	</c:forEach>
	// posting id들을 넣음 -> postid로 좋아요의 개수를 알아내서 할예정!

	function heartview() {
	    var jsonData = JSON.stringify(param); // 자바스크립트 값을 json문자열로 변환
	    jQuery.ajaxSettings.traditional = true;
		$.ajax({
			url : '/heart/view',
			type : 'post',
			async : true,
			data: {"jsonData" : jsonData},
			dataType : "json",
			success : function(data) {
				$.each(data.pu, function(key, value){
					var a = '';
					if(value.b == false){ // 좋아요를 안눌렀으므로
						a += '<a onclick="likeInsert('+ value.postid +');"' // 좋아요를 누름
							+'class="glyphicon glyphicon-heart-empty heart" aria-hidden="true"></a>'
						a += '<a href="" class="glyphicon glyphicon-comment" aria-hidden="true" /> <br />'
						$.each(data.pst, function(key, val){ 
						// 개수는 postid와 하나하나 맞추ㅓ서 맞는거 한개만 출력됨... 여기가 너무 오버헤드 발생할거같은데 좋은방법이없당...ㅋㅋㅎ
							if(val.postid == value.postid){
								a += '<span><b>좋아요 ' + val.cnt + '개</b></span>'	
							}
						});
					}
					else{
						a += '<a onclick="likeDelete('+ value.postid +');"'
							+ 'class="glyphicon glyphicon-heart heart" aria-hidden="true"></a>'
						a += '<span class="glyphicon glyphicon-comment" aria-hidden="true"></span> <br />'
						$.each(data.pst, function(key, val){
							if(val.postid == value.postid){
								a += '<span><b>좋아요 ' + val.cnt + '개</b></span>'
							}
						});
					}
					
					$('.heart_' + value.postid).html(a); // 각 postid에 맞게 넣어줌
				});
			}
		});
	}
	
	// insert랑 delete는 9번게시글에서 이미 다뤄서 설명생략!
	function likeInsert(pid){
		$.ajax({
			url : '/like/insert/' + pid,
			type : 'post',
			success : function(data){
				if(data == 1)
					heartview();
			}
		});
	}
	
	function likeDelete(pid){
		$.ajax({
			url : '/like/delete/' + pid,
			type : 'post',
			success : function(data){
				if(data == 1)
					heartview();
			}
		});
	}
	
	$(document).ready(function() {
		heartview();
	});
</script>
```

![메인페이지1](../../../assets/15-1.gif)
