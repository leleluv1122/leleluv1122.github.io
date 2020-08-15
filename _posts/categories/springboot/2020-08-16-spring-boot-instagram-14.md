---
title: "스프링 부트 Jsp JPA Spring Security 인스타그램 따라해보기 (14) - 메인페이지1"
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

# 메인페이지
 
메인페이지에는 내 게시물과 내가 팔로우 한 사람의 게시물이 최신~오래된 순으로 게시된다.

## followRepository.java
 파일위치: src/main/java/out/stagram/repository/FollowRepository.java

```java
List<Follow> findByFollowingId(int id); // id유저가 팔로우 한 사람의 목록
```

## followService.java
 파일위치: src/main/java/out/stagram/service/FollowService.java

```java
public List<Follow> findByFollowingId(int id) {
	return followRepository.findByFollowingId(id);
}
```

## Post.java
 파일위치: src/main/java/out/stagram/domain/Post.java

```java
public class Post implements Comparator<Post> { // 위에 comparator를 추가해준다

	.
	.
	.

	@Override
	public int compare(Post p1, Post p2) { // 이렇게 해주면 최신글 ~ 오래된 순서대로 가능
		long l1 = p1.getCreate_date().getTime();
		long l2 = p2.getCreate_date().getTime();
		
		if(l1 > l2)
			return -1;
		else
			return 1;
			
	}	
}
```

## postRepository.java
 파일위치: src/main/java/out/stagram/repository/PostRepository.java

```java
List<Post> findByUserIdOrderByIdDesc(int id);
```

## postService.java
 파일위치: src/main/java/out/stagram/service/PostService.java

```java
public List<Post> findByUserIdOrderByIdDesc(int id) {
	return postRepository.findByUserIdOrderByIdDesc(id);
}
```

## PoCo.java
 파일위치: src/main/java/out/stagram/domain/PoCo.java

```java
@Data
public class PoCo { // post count 의 줄임... zzz
	int postid;
	int cnt;
}
```

## CommentRepsository.java
 파일위치: src/main/java/out/stagram/repository/CommentRepsository.java

```java
int countByPostId(int id);
```

## CommentService.java
 파일위치: src/main/java/out/stagram/repository/CommentService.java

```java
public int countByPostId(int id) {
	return commentRepository.countByPostId(id);
}
```

## MainController.java
 파일위치: src/main/java/out/stagram/controller/MainController.java
 
```java
@RequestMapping("/main")
public String main_page(Model model) throws Exception {
	String userId = SecurityContextHolder.getContext().getAuthentication().getName();
	User u = userService.findByUserId(userId);

	// login user가 following한 사람 리스트
	List<Follow> follower_list = followService.findByFollowingId(u.getId());

	// login user의 포스팅 select
	List<Post> posting = postService.findByUserIdOrderByIdDesc(u.getId());

	// following한 유저의 게시글 select 후 user 포스팅과 list 합치기
	for (Follow f : follower_list) {
		List<Post> post = postService.findByUserIdOrderByIdDesc(f.getFollower().getId());
		for (Post p : post) {
			posting.add(p);
		}
	}

	List<PoCo> cmtcnt = new ArrayList<>();
	for (Post po : posting) {
		PoCo p = new PoCo();
		p.setPostid(po.getId());
		p.setCnt(commentService.countByPostId(po.getId()));

		cmtcnt.add(p);
	}
	model.addAttribute("cmt_cnt", cmtcnt);

	// 포스팅 날짜순으로 거꾸로 정렬하기(최근~오래된순)
	Post p = new Post();
	Collections.sort(posting, p);

	model.addAttribute("user", u);
	model.addAttribute("posting", posting);
	model.addAttribute("psize", posting.size());
	model.addAttribute("img", piService.findAll());

	return "/main";
}
```

## main.jsp
 파일위치: src/main/webapp/WEB-INF/views/main.jsp

```html
<div id="contents">
	<div class="post">
		<div class="nav">
			<span class="title"> <a href="/main" class="title_ft">Outstagram</a>
			</span> <a href="/main"><span class="glyphicon glyphicon-send"
				aria-hidden="true"></span></a>
		</div>
		<c:choose>
			<c:when test="${psize == 0}">
				<div class="nopost">
					<span>게시물이 없습니다.</span> <br /> 
					<a href="/main/recommend">팔로우 하러 가기</a>
				</div>
			</c:when>
			<c:otherwise>
				<c:forEach var="p" items="${posting}">
					<div class="r">
						<div>
							<div class="title_image">
								<c:choose>
									<c:when test="${p.user.profile_photo == null}">
										<a href="/main/user/${p.user.id}"> <img
											src="/images/noimage.png" class="tiny_image" align="left">
										</a>
									</c:when>
									<c:otherwise>
										<a href="/main/user/${p.user.id}"> 
										<img src="/images/profile/${p.user.profile_photo}"
											class="tiny_image" align="left">
										</a>
									</c:otherwise>
								</c:choose>
								</div>
							<div class="userid_txt">
								<a href="/main/user/${p.user.id}">${p.user.userId}</a>
							</div>
						</div>
						<div id="gallery_wrap">
							<ul class="slide_gallery">
								<c:forEach var="img" items="${img}">
									<c:if test="${p.id == img.postId}">
										<li><img src="/images/${p.user.userId}/${img.filename}" class="imgg"></li>
									</c:if>
								</c:forEach>
							</ul>
						</div>
						<sec:authentication property="user.id" var="currentid" />
						<div class="bar">
							<div class="heart_${p.id}"></div> <!-- 좋아요는 나중에 함 -->
						</div>
						<div class="write" style="cursor: pointer;">
							<span onclick="location.href='/main/post/${p.id}'">${p.description}</span>
								<div class="tag_${p.id}"></div> <!-- 태그는 나중에 함 -->
								<br />
								<c:forEach var="cmt" items="${cmt_cnt}">
								<c:if test="${p.id == cmt.postid && cmt.cnt > 0}">
									<span onclick="location.href='/main/post/${p.id}'">
										댓글 ${cmt.cnt}개 모두 보기</span>
								</c:if>
							</c:forEach>
						</div>
					</div>
				</c:forEach>
			</c:otherwise>
		</c:choose>
	</div>
</div>
<div id="footer">
	<%@ include file="include/bottom.jsp"%>
</div>
```

메인페이지에 내게시글과 팔로우한 계정의 게시글이 뜬다

![메인페이지1](../../../assets/14-1.gif)

댓글개수가 뜨구 들어가면 댓글수만큼 댓글이 있다 

![메인페이지2](../../../assets/14-2.gif)
