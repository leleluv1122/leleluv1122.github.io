---
title: "jQuery 다양한 효과, 애니메이션"
categories: Javascript_jQuery
comments: true
---

jQuery 다양한 효과와 애니메이션

# 효과 및 애니메이션 메서드 
 문자 객체를 보이게 했다가 안 보이게 하려면 스타일(CSS)의 display 속성을 이용해야 한다.

## 효과 메서드
 효과(Effect) 메서드에는 선택한 요소를 역동적으로 숨겼다가 보이게 만드는 기능을 가진 메서드가 있다.

 **효과 메서드 종류**
  - 숨김 
    - hide() : 요소를 숨긴다.
    - fadeOut() : 요소가 점점 투명해지면서 사라진다.
    - slideUp() : 요소가 위로 접히며 숨겨진다.
  - 노출
    - show() : 숨겨진 요소가 노출된다.
    - fadeIn() : 숨겨진 요소가 점점 선명해진다.
    - slideDown() : 숨겨진 요소가 아래로 펼쳐진다.
  - 노출, 숨김
    - toggle() : hide(), show() 효과를 적용한다.
    - fadeToggle() : fadeIn(), fadeOut() 효과를 적용한다.
    - slideToggle() : slideUp(), slideDown() 효과를 적용한다.
    - fadeTo() : 지정한 투명도를 적용한다.

### 효과 메서드의 기본형
 효과 메서드는 효과 소요 시간, 가속도, 콜백 함수를 인자값으로 전달할 수 있다.
```
$("요소 선택").효과 메서드(효과 소요 시간, 가속도, 콜백 함수);

1. 효과 소요 시간
 방법1: "slow", "normal", "fast"
 방법2: 1.000(1초), 500(0.5초)

2. 가속도
 방법1: "swing" 시작과 끝은 느리게, 중간은 빠른 속도로 움직인다. 
 방법2: "linear" 일정한 속도로 움직인다.

3. 콜백함수
 노출과 숨김효과가 끝난 후 실행할 함수 / 생략 가능

$("#box").slideUp(2000, "linear", function() {
   alert("hello");
});
```

### fadeTo() 메서드
```
$("요소 선택").fadeTo(효과 소요 시간, 투명도, 콜백 함수);
```

## 동작을 불어넣는 애니메이션 메서드
 애니메이션 메서드를 적용하면 스타일을 적용한 요소를 움직이게 할 수 있다.

### animate() 메서드
 animate() 메서드를 이용하면 선택한 요소에 다양한 동작(motion) 효과를 적용할 수 있다. 예를 들어 요소를 앞으로 이동시키거나 점차 커지게 하는 등 다양한 동작을 적용할 수 있다.

```java
$("요소 선택").animate({스타일 속성}, 적용 시간, 가속도, 콜백 함수)
```

ex
```html
<script>
$(function(){
	$(".btn1").on("click", function( ) {
		$(".txt1").animate({
			marginLeft:"500px",
			fontSize:"30px"
		},
		2000, "linear", function( ) {
			alert("모션 완료!"); 
		});
	});
</script>
<style>
	.txt1{background-color: aqua;}
</style>
```

# 애니메이션 효과 제어 메서드

## 애니메이션 효과 제어 메서드란?
 애니메이션 효과 제어 메서드란 '효과' 또는 '애니메이션'이 적용된 요소의 동작을 제어하는 메서드이다.

### 애니메이션 적용 원리와 큐의 개념
 애니메이션 메서드는 함수가 적용된 순서대로 큐(queue)라는 저장소(memory)에 저장된다. 큐는 FIFO(First In First Out)이다. 큐에 저장된 애니메이션 대기열이 있다면 먼저 들어간 애니메이션이 먼저 실행된다.

```html
$(".txt").animate({marginLeft : "200px"}, 1000)   -   1
         .animate({marginTop : "200px"}, 1000)    -   2
         .animate({marginLeft : 0}, 1000)         -   3
         .animate({marginTop : 0}, 1000)          -   4
```
1 -> 2 -> 3 -> 4 순서대로 실행된다.

  **애니메이션 효과 제어 메서드의 종류**
   - stop() : 현재 실행 중인 효과를 모두 정지시킨다
   - delay() : 지정한 시간만큼 지연했다가 애니메이션을 진행한다.
   - queue() : 큐에 사용자 정의 함수를 추가하거나 큐에 대기 중인 함수를 배열에 담아 반환한다. 그리고 queue() 메서드 이후의 애니메이션 효과 메서드는 모두 취소한다.
   - clearQueue() : 큐에서 처음으로 진행하고 있는 애니메이션만 제외하고 대기 중인 애니메이션은 모두 제거한다.
   - dequeue() : queue() 메서드를 이용하면 대기하고 있는 애니메이션 메서드는 제거된다. 하지만 dequeue() 메서드를 이용하면 메서드가 제거되는 것을 막을 수 있다.
   - finish() : 선택한 요소의 진행 중인 애니메이션을 강제로 완료 시점으로 보낸 후 종료한다.

### stop()/delay() 메서드

```java
1. $("요소 선택").stop();
2. $("요소 선택").stop(clearQueue, finish);
```

```java
1. 진행 중인 애니메이션만 정지
 $(".txt1").animate({opacity:0.5}, 1000)
  .animate({marginLeft:"500px"}, 1000);
 $(".txt").stop();

2. 대기 중인 애니메이션은 제거하고 진행 중인 애니메이션은 강제로 종료하는 경우
 $(".txt2").animate({opacity:0.5}, 1000)
  .animate({marginLeft:"500px"}, 1000);
 $(".txt2").stop(true, true);
```

```java
$("요소 선택").delay(지연 시간).애니메이션 효과 메서드();
```

```java
$(".txt1").delay(3000).animate({marginLeft:"500px"}, 1000);
```

### queue()/dequeue() 메서드
 queue() 메서드는 큐(queue)에 적용된 애니메이션 함수를 반환하거나 큐에 지정한 함수를 추가한다. queue() 메서드를 실행하면 실행 이후의 모든 애니메이션이 취소된다. dequeue() 메서든느 queue() 메서드 실행 이후에 적용된 애니메이션 메서드가 취소되지 않게 연결해 준다.

```java
1. 큐(Queue)의 함수 반환
 $("요소 선택").queue();

2. 큐(Queue)에 함수 추가
 $("요소 선택").queue(function() { 자바스크립트 코드 });

3. dequeue() 메서드
 $("요소 선택").dequeue();
```

### clearQueue() 메서드
 진행 중인(첫 번째) 애니메이션을 제외하고 큐에서 대기하는 모든 애니메이션 함수를 제거한다.

```java
$("요소 선택").clearQueue();
```