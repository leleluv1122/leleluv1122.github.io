﻿---
title: "jQuery event"
categories: Javascript_jQuery
comments: true
---

jQuery 기본적인 문법들과 jQuery의 event들에 대해서!


# 선택자
## 기본 선택자
### 직접 선택자
 - 전체 선택자: `$(*)` : 모든 요소를 선택한다
 - 아이디 선택자: `$("#아이디명")` : id 속성에 지정한 값을 가진 요소를 선택한다
 - 클래스 선택자: `$(".클래스명")` : class 속성에 지정한 값을 가진 요소를 선택한다
 - 요소 선택자: `$("요소명")` : 지정한 요소명과 일치하는 요소들만 선택한다.
 - 그룹 선택자: `$("선택1, 선택2, 선택3, ... 선택 n")` : 선택1, 선택2, 선택3, ... 선택n에 지정된 요소들을 한 번에 선택한다.
 - 종속 선택자: `$("p.txt_1")`  `$("p#txt_1")` : <p> 요소 중 class값이 txt_1인 요소 또는 id값이 txt_1인 요소를 선택한다.

### 인접 관계 선택자
 - 부모 요소 선택자: `$("요소 선택").parent()` : 선택한 요소의 부모 요소를 선택한다.
 - 상위 요소 선택자: `$("요소 선택").parents()` : 선택한 요소의 상위 요소를 모두 선택한다.
 - 가장 가까운 상위 요소 선택자: `$("요소 선택").closet("div")` : 선택한 요소의 상위 요소 중 가장 가까운 <div>만 선택한다.
 - 하위 요소 선택자: `$("요소 선택 하위 요소")` : 선택한 요소에 지정한 하위 요소를 선택한다.
 - 자식 요소 선택자: `$("요소 선택>자식 요소")` : 선택한 요소를 기준으로 자식 관계에 지정한 요소만 선택한다.
 - 자식 요소들 선택자: `$("요소 선택").children()` : 선택한 요소의 모든 자식 요소를 선택한다.
 - 형(이전) 요소 선택자: `$("요소 선택").prev()` : 선택한 요소의 바로 이전 요소를 선택한다.
 - 형(이전) 요소들 선택자: `$("요소 선택").prevAll()` : 선택한 요소의 이전 요소 모두를 선택한다.
 - 지정 형(이전) 요소들 선택자: `$("요소 선택").prevUntil("요소명")` : 선택한 요소의 다음 요소를 선택한다.
 - 동생(다음) 요소 선택자: `$("요소 선택").next()` `$("요소 선택 + 다음 요소")` : 선택한 요소의 다음 요소를 선택한다.
 - 동생(다음) 요소들 선택자: `$("요소 선택).nextAll()` : 선택한 요소의 다음 요소 모두를 선택한다.
 - 지정 동생(다음) 요소들 선택자: `$("요소 선택).nextUntil("h2")` : 선택한 요소부터 지정한 요소의 다음 요소까지 모두 선택한다.
 - 전체 형제 요소 선택자: `$(".box_1").siblings()` : class 값이 box_1인 요소의 형제 요소 전체를 선택한다.

### 속성 탐색 선택자
 - `$("요소 선택[속성]")` : `$("li[title]")` - `<li>` 요소 중 title 속성이 포함된 요소만 선택한다.
 - `$("요소 선택[속성=값]")` : `$("li[title='리스트']")` - `<li>` 요소 중 title 속성값이 '리스트'인 요소만 선택한다.
 - `$("요소 선택[속성^=텍스트]")` : `$("a[href^='http://']")` - `<li>` 요소 중 href 속성값이 'http://'로 시작하는 요소 만 선택한다.
 - `$("요소 선택[속성$=텍스트]")` : `$("a[href$='.com']")` - `<li>` 요소 중 href 속성값이 '.com'으로 끝나는 요소 만 선택한다.
 - `$("요소 선택[href*=텍스트]")` : `$("a[href*='easyspub']")` - `<li>` 요소 중 href 속성값중에서 'easyspub'을 포함하는 요소만 선택한다.
 - `$("요소 선택:hidden")` : `$("li:hidden")` - `<li>` 요소 중 숨겨져 있는 요소만 선택한다.
 - `$("요소 선택:visible")` : `$("li:visible")` - `<li>` 요소 중 보이는 요소만 선택한다.
 - `$(".text")` : `$(".text")` - `<input>` 요소 중 type 속성값이 "text"인 요소만 선택한다.
 - `$(".selected")` : `$(".selected")` - selected 속성이 적용된 요소만 선택한다.
 - `$(".checked")` : `$(".checked")` - checked 속성이 적용된 요소만 선택한다.


# jQuery event
## 이벤트 등록 메서드
 이벤트란 사이트에서 방문자가 취하는 모든 행위를 말하고, 이벤트 핸들러는 이벤트가 발생했을 때 실행되는 코드를 말한다. 이 메서드를 이용하면 방문자가 지정한 요소에서 어떠한 특정 동작이 일어났을 때 저장된 코드를 실행시킬 수 있다.

 이벤트 등록 메서드에는 하나의 이벤트만 등록할 수 있는 단독 이벤트 등록 메서드와 2개 이상의 이벤트를 등록할 수 있는 그룹 이벤트 등록 메서드가 있다. 단독 이벤트 메서드는 한 동작에 대한 이벤트를 등록할 때 사용하는 메서드이다. 

### 이벤트 등록 메서드의 종류
 - **로딩이벤트**
   - load() : 선택한 이미지 또는 프레임 요소에 연동된 소스의 로딩이 완료된 후 이벤트가 발생한다.
   - ready() : 지정한 HTML 문서 객체의 로딩이 완료된 후 이벤트가 발생한다.
   - error() : 이벤트 대상 요소에서 오류가 발생하면 이벤트가 발생한다.

 - **마우스이벤트**
   - click() : 선택한 요소를 클릭했을 때 이벤트가 발생한다.
   - dbclick() : 선택한 요소를 연속해서 두 번 클릭했을 때 이벤트가 발생한다.
   - mouseout() : 선택한 요소의 영역에서 마우스 포인터가 벗어났을 때 이벤트가 발생한다. 이때 하위 요소의 영향을 받는다.
   - mouseover() : 선택한 요소의 영역에 마우스 포인터를 올렸을 때 이벤트가 발생한다.
   - hover() : 선택한 요소에 마우스 포인터를 올렸을 때와 벗어났을 때 각각 이벤트가 발생한다.
   - mousedown() : 선택한 요소에서 마우스 버튼을 눌렀을 때 이벤트가 발생한다.
   - mouseup() : 선택한 요소에서 마우스 버튼을 눌렀다 떼었을 때 이벤트가 발생한다.
   - mouseenter() : 선택한 요소 범위에 마우스 포인터를 올렸을 때 이벤트가 발생한다.
   - mouseleave() : 선택한 요소 범위에서 마우스 포인터가 벗어났을 때 이벤트가 발생한다.
   - mousemove() : 선택한 요소 범위에서 마우스 포인터를 움직였을 때 이벤트가 발생한다.
   - scroll() : 가로, 세로 스크롤바를 움직일 때마다 이벤트가 발생한다.

 - **포커스이벤트**
   - focus() : 선택한 요소에 포커스가 생성되었을 때 이벤트를 발생하거나 선택한 요소에 강제로 포커스를 생성한다.
   - focusin() : 선택한 요소에 포커스가 생성되었을 때 이벤트가 발생한다.
   - focusout() : 포커스가 선택한 요소에서 다른 요소로 이동되었을 때 이벤트가 발생한다.
   - blur() : 포커스가 선택한 요소에서 다른 요소로 이동되었을 때 이벤트가 발생하거나 선택한 요소의 포커스가 강제로 사라지도록 한다.
   - change() : 이벤트 대상인 입력 요소의 값이 변경되고, 포커스가 이동하면 이벤트가 발생한다. 그리고 강제로 change 이벤트를 발생시킬때도 사용한다.

 - **키보드이벤트**
   - keypress() : 선택한 요소에서 키보드를 눌렀을 때 이벤트가 발생한다. 그리고 문자 키를 제외한 키의 코드값을 반환한다.
   - keydown() : 선택한 요소에서 키보드를 눌렀을 때 이벤트가 발생한다. 키보드의 모든 키의 코드값을 반환한다.
   - keyup() : 선택한 요소에서 키보드에서 손을 떼었을 때 이벤트가 발생한다.

### 이벤트 등록 방식 알아보기
 - **단독이벤트**
  단독 이벤트 등록 메서드는 대상에 한 가지 동작에 대한 이벤트만 등록할 수 있다.

```java
$("이벤트 대상 선택").이벤트 등록 메서드(function(){
   자바스크립트 코드;
});
```
```java
<button id="btn1">버튼</button>
$("#btn1").click(function(){
     alert("welcome");
});
```


 - **그룹이벤트**
  그룹 이벤트 등록 메서드는 대상에 한 가지 동작 이상의 이벤트를 등록할 수 있다. on() 메서드를 사용하여 이벤트를 등록한다.

```java
1. on() 메서드 등록 방식1
$("이벤트 대상 선택").on("이벤트 종류1 이벤트 종류2 ... 이벤트 종류n",
function(){
    자바스크립트코드;
});

2. on() 메서드 등록 방식2
$("이벤트 대상 선택").on({
   "이벤트 종류1 이벤트 종류2 ... 이벤트 종류n" : function() {
      자바스크립트 코드;
   }
});

3. on() 메서드 등록 방식3
$("이벤트 대상 선택").on({
   "이벤트 종류1" : function(){ 자바스크립트 코드; 1 },
   "이벤트 종류2" : function(){ 자바스크립트 코드; 2 },
   "이벤트 종류3" : function(){ 자바스크립트 코드; 3 },
           ....
   "이벤트 종류n" : function(){ 자바스크립트 코드; n },
```

 1. on() 메서드 등록 방식1

```java
$("#btn1").on("mouseover focus", function(){
   console.log("welcome");
})
```

 2. on() 메서드 등록 방식1

```java
$("#btn1").on({
   "mouseover focus" : function(){
     console.log("welcome");
   }
});
```

 3. on() 메서드 등록 방식1

```java
$("#btn1").on({
   "mouseover" : function(){
     console.log("welcome");
   },
   "focus" : function(){
     console.log("welcome");
   }
});
```

### 이벤트 객체와 종류

```java
$("이벤트 대상 선택").mousemove(function(매개변수){
   매개변수(이벤트 객체).속성;
});
```

 - **마우스 이벤트**
   - clientX : 마우스 포인터의 X 좌푯값 반환(스크롤 이동 거리 무시)
   - clientY : 마우스 포인터의 Y 좌푯값 반환(스크롤 이동 거리 무시)
   - pageX : 스크롤 X축의 이동한 거리를 계산하여 마우스 포인터의 X 좌푯값을 반환
   - pageY : 스크롤 Y축의 이동한 거리를 계산하여 마우스 포인터의 Y 좌푯값을 반환
   - screenX : 화면 모니터를 기준으로 마우스 포인터의 X 좌푯값을 반환
   - screenY : 화면 모니터를 기준으로 마우스 포인터의 Y 좌푯값을 반환
   - layerX : position을 적용한 요소를 기준으로 마우스 포인터의 X 좌푯값을 반환
   - layerY : position을 적용한 요소를 기준으로 마우스 포인터의 Y 좌푯값을 반환
   - button : 마우스 버튼의 종류에 따라 값을 반환 (왼쪽: 0, 휠: 1, 오른쪽:2)

 - **키보드 이벤트**
   - keyCode : 키보드의 아스키 코드값을 반환
   - altKey : 이벤트 발생 시 Alt 키가 눌렸으면 true를, 아니면 false를 반환
   - ctrlKey : 이벤트 발생 시 Ctrl 키가 눌렸으면 true를, 아니면 false를 반환
   - shiftKey : 이벤트 발생 시 Shift 키가 눌렸으면 true를, 아니면 false를 반환

 - **전체 이벤트**
   - target : 이벤트가 전파된 마지막 요소를 가리킨다.
   - cancelBubble : 이벤트의 전파를 차단하는 속성으로, 기본값은 false며, true로 설정하면 전파가 차단된다.
   - stopPropagation() : 이벤트의 전파를 차단한다.
   - preventDefault() : 기본 이벤트를 차단한다. 예를 들어 `<a>`에 클릭 이벤트를 적용하고 사용자가 이벤트를 발생시키면 기본 이벤트가 등록되어 있어 링크 주소로 이동하는데, 이런 기본 이벤트를 차단할 수 있다.

### scroll() 이벤트 메서드
 scroll() 이벤트 메서드는 대상 요소의 스크롤바가 이동할 때마다 이벤트를 발생시키거나 강제로 scroll이벤트를 발생시키는 데 사용한다.

```java
1. scroll 이벤트 등록
 $("이벤트 대상 선택").scroll(function() { 자바스크립트 코드; });
 $("이벤트 대상 선택").on("scroll", function() { 자바스크립트 코드; });

2. scroll 이벤트 강제 발생
 $("이벤트 대상 선택").scroll();
```

## 포커스 이벤트
 포커스는 마우스로 `<a>` 또는 `<input>` 태그를 클릭하거나 Tab 를 누르면 생성된다.

### focus()/blur()/focusin()/focusout() 이벤트 메서드
 - focus() : 대상 요소로 포커스가 이동하면 이벤트를 발생시킨다.
 - blur() : 포커스가 대상 요소에서 다른 요소로 이동하면 이벤트를 발생시킨다.

```java
1. focus 이벤트 등록
 $("이벤트 대상 선택").focus(function() { 자바스크립트 코드; });
 $("이벤트 대상 선택").on("focus", function() { 자바스크립트 코드; });

2. focus 이벤트 강제 발생
 $("이벤트 대상 선택").focus();

3. blur 이벤트 등록
 $("이벤트 대상 선택").blur(function() { 자바스크립트 코드; });
 $("이벤트 대상 선택").on("blur", function() { 자바스크립트 코드; });

4. blur 이벤트 강제 발생
 $("이벤트 대상 선택").blur();
```
 - focusin() : 대상 요소의 하위 요소 중 입력 요소로 포커스가 이동하면 이벤트를 발생시킨다.
 - focusout() : 대상 요소의 하위 요소 중 입력 요소에서 외부 요소로 이동하면 이벤트를 발생시킨다.

```java
1. focusin 이벤트 등록
 $("이벤트 대상 선택").focusin(function() { 자바스크립트 코드; });
 $("이벤트 대상 선택").on("focusin", function() { 자바스크립트 코드; });

2. focusin 이벤트 강제 발생
 $("이벤트 대상 선택").focusin();

3. focusout 이벤트 등록
 $("이벤트 대상 선택").focusout(function() { 자바스크립트 코드; });
 $("이벤트 대상 선택").on("focusout", function() { 자바스크립트 코드; });

4. focusout 이벤트 강제 발생
 $("이벤트 대상 선택").focusout();
```

## 키보드로 마우스 이벤트 대응하기
 키보드 접근성이란 어떤 대상 요소에 마우스 이벤트를 등록했을 때 마우스 없이 키보드로 대상 요소를 사용할 수 있게 하는 것을 말한다.

 **마우스 이벤트에 대한 키보드 대응 이벤트**

```java
     마우스 이벤트  :  키보드 이벤트
        mouseover   :   focus
        mouseout    :   blur 
```

### 1. 키보드 접근성을 배려하지 않은 이벤트 예 (비추천)

```java
<button class="btn">버튼</button>
<p class="txt_1">내용1</p>

$(".btn").mouseover(function(){
   $(".txt_1").hide();
});
```

### 3. 키보드 접근성을 배려한 이벤트 예 (추천)

```java
<button class="btn">버튼</button>
<p class="txt_1">내용1</p>

$(".btn").on("mouseover focus", function(){
   $(".txt_1").hide();
});
```

### change() 이벤트 메서드
 change() 이벤트 메서드는 선택한 폼 요소의 값(value)을 새 값으로 바꾼다. 그리고 포커스가 다른 요소로 이동하면 이벤트를 발생시킨다.

```java
1. focus 이벤트 등록
 $("이벤트 대상 선택").change(function() { 자바스크립트 코드; });
 $("이벤트 대상 선택").on("change", function() { 자바스크립트 코드; });

2. focus 이벤트 강제 발생
 $("이벤트 대상 선택").change();
```

## 키보드 이벤트
 키보드 이벤트는 사용자가 키보드로 입력을 하면 발생한다.

### keydown()/keyup()/keypress() 이벤트 메서드
 keydown()와 keypress() 이벤트 메서드는 선택한 요소에서 키보드 자판을 눌렀을 때 이벤트를 발생시키거나 해당 이벤트를 강제로 발생시킨다. 두 이벤트의 차이점을 보면 keydown()은 모든 키(한글 키 제외)에 대해서 이벤트를 발생시키지만, keypress()는 기능키(F1 ~ F12, Alt, Ctrl, Shift, Backspace, Caps Lock, 한/영, Tab 등)에 대해서는 이벤트를 발생시키지 않는다.

```java
1. keydown 이벤트 등록
  1. $("이벤트 대상 선택").keydown(function() { 자바스크립트 코드; });
  2. $("이벤트 대상 선택").on("keydown", function() { 자바스크립트 코드; });

2. keydown 이벤트 강제 발생 
  $("이벤트 대상 선택").keydown();

3. keyup 등록
  1. $("이벤트 대상 선택").keyup(function() { 자바스크립트 코드; });
  2. $("이벤트 대상 선택").on("keyup", function(){ 자바스크립트 코드; });

4. keyup 이벤트 강제 발생
  $("이벤트 대상 선택").keyup();

5. keypress 등록
  1. $("이벤트 대상 선택").keypress(function() { 자바스크립트 코드; });
  2. $("이벤트 대상 선택").on("keypress", function(){ 자바스크립트 코드; });

6. keypress 이벤트 강제 발생
  $("이벤트 대상 선택").keypress();
```

## 이벤트가 발생한 요소 추적하기
 사이트 방문자가 이벤트를 발생시킨 요소의 정보를 구해오는 방법이다. 이벤트가 발생한 요소를 선택해 오는 선택자 $(this) 사용하기

### $(this) 선택자
 이벤트 핸들러에서 $(this)를 사용하면 이벤트가 발생한 요소를 선택하여 이벤트가 발생한 요소를 추적할 수 있습니다.

### index() 인덱스 반환 메서드
 index() 인덱스 반환 메서드는 이벤트를 등록한 요소 중 이벤트가 발생한 요소의 인덱스값을 반환한다.
```java
$("이벤트 대상 선택").on("이벤트 종류", function(){
   $("이벤트 대상 선택").index(this);
});
```


# 그룹 이벤트 등록 및 삭제하기
 
## 그룹 이벤트 등록 메서드
 그룹 이벤트 등록 메서드를 사용하면 한 번에 2개 이상의 이벤트를 등록할 수 있다. 즉, 선택한 요소에 이벤트 등록 메서드를 한 번만 적용하여 '마우스 포인터를 올렸을 때'와 '포커스가 생성되었을 때'처럼 두 종류의 이벤트가 발생하도록 만들 수 있다.

### 그룹 이벤트 등록 메서드의 종류
 - on() : 이벤트 대상 요소에 2개 이상의 이벤트를 등록한다. 사용 방식에 따라 이벤트를 등록한 이후에도 동적으로 생성되거나 복제된 요소에도 이벤트가 적용된다.
 - bind() : 이벤트 대상 요소에 2개 이상의 이벤트를 등록한다.
 - delegate() : 선택한 요소의 하위 요소에 이벤트를 등록한다. 이벤트를 등록한 이후에도 동적으로 생성되거나 복제된 요소에도 이벤트가 적용된다.
 - one() : 이벤트 대상 요소에 1개 이상의 이벤트를 등록한다. 지정한 이벤트가 1회 발생하고 자동으로 해제된다.

### on() 메서드
 on() 메서드는 선택한 요소에 이벤트를 등록한 이후에도 새롭게 생성되거나 복제된 요소에 이벤트를 적용할 수 있다. 즉, 동적으로 생성되거나 복제된 요소에도 이벤트가 등록된다.

```java
$([document | "이벤트 대상의 상위 요소 선택"]).on("이벤트 종류", "이벤트 대상 선택", function() {
   자바스크립트 코드;
});
```

### delegate()/one() 이벤트 등록 메서드
 delegate() 이벤트 등록 메서드는 선택한 요소의 하위 요소에 이벤트를 등록한다. 그리고 이벤트를 등록한 이후에도 동적으로 생성된 요소와 복제된 요소에도 이벤트를 등록한다.

```java
$([document | "이벤트 대상의 상위 요소 선택"]).delegate("이벤트 대상 요소 선택", "이벤트 종류", function() {
   자바스크립트 코드;
});
```

 one() 이벤트 등록 메서드는 이벤트가 1회 발생하면 자동으로 등록된 이벤트가 제거된다. 즉, 일회성 이벤트를 등록할 때 사용한다. one() 이벤트 등록 메서드도 등록 방식에 따라 '라이브 이벤트'의 등록이 가능하다.

```java
1. one() 기본 이벤트 등록 방식
 $("이벤트 대상 선택").one("이벤트 종류", function() {
    자바스크립트 코드;
 });

2. one() 라이브 이벤트 등록 방식
 $([document | "이벤트 대상의 상위 요소 선택"]).one("이벤트 종류", "이벤트 대상 요소 선택", function() {
    자바스크립트 코드;
 });
```