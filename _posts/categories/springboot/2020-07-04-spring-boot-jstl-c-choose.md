---
title: "[JSTL] c:if와 c:choose c:when c:otherwise 사용법"
categories: springboot
comments: true
---


# c:if
 c:if는 자바의 if문과 같은 역할을 한다.  

```html
<c:if test="${user.profile_image == null}>
	<img src="/images/noimage.png" class="profile_image">
</c:if>
```

이렇게 사용가능하지만  
만약, 사용자의 프로필이미지가 null이 아닐경우에는?  
이런 경우에 생각하는게 `<c:else>` 를 떠올리겠지만(내가그랬음..) JSTL에는 존재하지 않다.  
그래서 사용하는 방법이 밑의 방법이다.  

# c:choose

 `<c:choose>` 는 자바의 switch문과 비슷한 역할을 합니다.  

## c:when

 `<c:when>` 은 case문과 같은 역할을 하고, 속성은 `test=""` 을 필수로 사용한다.  
 `<c:choose>` 안에 사용합니다.  

## c:otherwise

 `<c:otherwise>` default문과 같은 역할을 하고, 속성은 따로 존재하지 않습니다.  
 `<c:choose>` 안에 사용합니다.  


## 예제

```html
<c:choose>
	<c:when test="${user.profile_photo == null}">
		<img src="/images/noimage.png" class="profile_image">
	</c:when>
	<c:otherwise>
		<img src="/images/profile/${user.profile_photo}" class="profile_image">
	</c:otherwise>
</c:choose>
```



