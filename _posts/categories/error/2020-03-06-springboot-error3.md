---
title: "Don't know how to iterate over supplied items in &lt;forEach&gt;"
categories: error
comments: true
---

맨날 오류다ㅋㅋ

```
javax.servlet.jsp.JspTagException: Don't know how to iterate over supplied "items" in &lt;forEach&gt;
at org.apache.taglibs.standard.tag.common.core.ForEachSupport.toForEachIterator(ForEachSupport.java:274)

	at org.apache.taglibs.standard.tag.common.core.ForEachSupport.supportedTypeForEachIterator(ForEachSupport.java:238)

	at org.apache.taglibs.standard.tag.common.core.ForEachSupport.prepare(ForEachSupport.java:155)

	at javax.servlet.jsp.jstl.core.LoopTagSupport.doStartTag(LoopTagSupport.java:256)

```

```jsp
		<div>
			<c:forEach var="pi" items="${pi}">
				<img src="/images/${pi.filename}" class="imgg">
			</c:forEach>
		</div>
```

여기서 오류가 났다고 뜬다... 왜그런가 pi를 추적해보니...

repository에서 List로 안하고 단일로 해서 for문이 돌수가 없던거였다..

해결방법은 간단했다...ㅜㅜ 

오늘도 바보가튼 나는 결국 새벽두시.... 벌써 2시네~~~~ 

낼은 한솥도시락 먹어야지 ㅠㅠ 밥먹고싶다 

뭐먹지 돈까스카레도 먹고싶고 돈까스덮밥도 먹고싶은데...

불닭마요도 한번 도전해보고싶다... 냠냠쩝쩝
