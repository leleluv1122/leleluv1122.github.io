---
title: "JavaScript의 기본문법"
categories: Javascript_jQuery
comments: true
---

혼자 뭔가를 만들어보니 JS에 대한 이해도가 많이 떨어져서
javascript와 jquery를 간단하게 책으로 보기로했다.

책은 Do it! 자바스크립트 + 제이쿼리 입문 (이지스퍼블리싱)

이 곳에는 간단한 메서드들이나 문법들을 적어놓고 나중에 필요할 때 볼 예정!!

# 내장객체(Built-in-Object)
## 날짜 관련 메서드
 
### 날짜 정보를 가져올 때(GET)
 - getFullYear() : 연도 정보를 가져옴
 - getMonth() : 월 정보를 가져옴(현재 월 - 1)
 - getDate() : 일 정보를 가져옴
 - getDay() : 요일 정보를 가져옴(일:0 ~ 토:6)
 - getHours() : 시 정보를 가져옴
 - getMinutes() : 분 정보를 가져옴
 - getSeconds() : 초 정보를 가져옴
 - getMilliseconds() : 밀리초 정보를 가져옴(1/1.000초 단위)
 - getTime() : 1970년 1월 1일부터 경과된 시간을 밀리초로 표기함
 - toGMTString() : GMT 표준 표기 방식으로 문자형 데이터로 반환함

### 날짜 정보를 수정할 때(SET)
 - setFullYear() : 연도 정보만 수정함
 - setMonth() : 월 정보만 수정함(월 - 1)
 - setDate() : 일 정보만 수정함
 - '요일'은 날짜를 바꾸면 자동으로 바뀌므로 setDay()는 없음
 - setHours() : 시 정보만 수정함
 - setMinutes() : 분 정보만 수정함
 - setSeconds() : 초 정보만 수정함
 - setMilliseconds() : 초 정보만 수정함
 - setTime() : 1970년 1월 1일부터 경과된 시간을 밀리초로 수정함
 - toLocaleString : 운영 시스템 표기 방식으로 문자형 데이터로 변환함

## 수학 객체
### 수학 객체의 메서드 및 상수
 - Math.abs(숫자) : 숫자의 절대값을 반환합니다
 - Math.max(숫자 1, 숫자 2, 숫자 3, 숫자 4) : 숫자 중 가장 큰 값을 반환합니다
 - Math.min(숫자 1, 숫자 2, 숫자 3, 숫자 4) : 숫자 중 가장 작은 값을 반환합니다
 - Math.pow(숫자 제곱값) : 숫자의 거듭제곱값을 반환합니다
 - Math.random() : 0 ~ 1 사이의 난수를 반환합니다
 - Math.round(숫자) : 소수점 첫째 자리에서 반올림하여 정수를 반환합니다
 - Math.ceil(숫자) : 소수점 첫째 자리에서 무조건 올림하여 정수를 반환합니다
 - Math.floor(숫자) : 숫자의 제곱근값을 반환합니다
 - Math.sqrt(숫자) : 숫자의 제곱근값을 반환합니다
 - Math.PI : 원주율 상수를 반환합니다


# 브라우저 객체 모델
## 브라우저 객체란?
 브라우저에 내장된 객체를 '브라우저 객체'라고 합니다. window는 브라우저 객체의 최상위 객체이며, window 객체에는 하위 객체가 포함되어 있습니다. 즉, 계층적 구조로 이루어져 있으며, 이를 브라우저 객체 모델(BOM, Browser Object Model)이라고 합니다.

```
 window - document - body, div, img, a, form, input ...

        - screen

        - location

        - history

        - navigator
```

### window 객체
 window는 브라우저 객체의 최상위 객체이며, 다음과 같은 메서드를 사용할 수 있습니다.
  - open("URL", "새 창 이름", "새 창 옵션") : URL 페이지를 새 창으로 나타냅니다.
  - alert(data) : 경고 창을 나타내고 데이터를 보여줍니다. 방문자가 [확인] 버튼을 누르면 alert()를 사용한 다음 위치의 코드를 수행합니다
  - prompt("질문", "답변") : 질문과 답변으로 질의응답 창을 나타냅니다.
  - confirm("질문 내용") : 질문 내용으로 확인이나 취소 창을 나타냅니다. [확인] 버튼을 누르면 true를 반환하고, [취소] 버튼을 누르면 false를 반환합니다.
  - moveTo(x, y) : 지정한 새 창의 위치를 이동합니다.
  - resizeTo(width, height) : 지정한 새 창의 크기를 변경합니다.
  - setIntercal(function(){ 자바스크립트 코드 }, 일정 시간 간격 ) : 지속적으로 일정한 시간 간격으로 함수를 호출하여 코드를 실행합니다
  - setTimeout(function(){ 자바스크립트 코드 }, 일정 시간 간격 ) : 단 한 번 일정한 시간 간격으로 함수를 호출하여 코드를 실행합니다.

### location 객체
 location 객체는 사용자 브라우저와 관련된 속성과 메서드를 제공하는 객체입니다. 현재 URL에 대한 정보(속성)와 새로 고침(reload) 메서드를 제공합니다.
 - location 객체의 기본형
```java
location.속성;
location.메서드();

1. 사용자 브라우저의 URL 경로 값을 가져옵니다.
 location.href;

2. 사용자의 브라우저의 URL 경로를 지정한 URL(http://~~~) 주소로 변경합니다.
 location.href = "http://~~";

3. 사용자 브라우저를 새로 고침합니다.
 location.reload();
```

 - location.href : 주소 영역의 참조 주소를 설정하거나 URL을 반환합니다
 - location.hash : URL의 해시값(#에 명시된 값)을 반환합니다.
 - location.hostname : URL의 호스트 이름을 설정하거나 반환합니다.
 - location.host : URL의 호스트 이름과 포트 번호를 반환합니다.
 - location.protocol : URL의 프로토콜을 반환합니다.
 - location.search : URL의 쿼리(요청값)를 반환합니다.
 - location.reload() : 마치 브라우저에서 F5 키를 누른 것처럼 새로 고침 합니다.



### history 객체
 사용자가 방문한 사이트의 기록을 남기고 이전 방문 사이트와 다음 방문 사이트로 다시 돌아갈 수 있는 속성과 메서드를 제공합니다.

 - history 객체의 기본형

```
1. history.속성;
2. history.메서드();
3. history.메서드(n);

1. 사용자가 방문한 사이트의 기록을 남긴 총 수량입니다.
 history.length;

2. 사용자가 방문한 사이트 중 바로 이전에 방문한 사이트로 이동합니다.
 history.back();

3. 사용자가 방문한 사이트 중 두 단계 이전에 방문한 사이트로 이동합니다.
 history.back(2);
```

 - history.back() : 이전 방문 사이트로 이동합니다.
 - history.forward() : 다음 방문 사이트로 이동합니다.
 - history.go(이동 숫자) : 이동 숫자에 -2를 입력하면 2단계 이전의 방문 사이트로 이동합니다
 - history.length : 방문 기록에 저장된 목록의 개수를 반환합니다.

### navigator 객체
 navigator 객체는 현재 방문자가 사용하는 브라우저 정보와 운영체제 정보를 제공하는 객체

## 자바스크립트 내장 함수
### 내장함수
 내장 함수란 자바스크립트 엔진에 내장된 함수를 말한다. 지금까지는 개발자가 함수를 정의하고 호출문을 사용해 함수 안에 있는 코드를 실행했다. 하지만 내장 함수는 개발자가 함수를 직접 선언하지 않아도 된다. 즉, 자바스크립트에 이미 내장된 함수는 바로 호출할 수 있다.

 내장 함수의 종류
  - encodeURI() 
   문자를 유니 코드값으로 인코딩 (영문, 숫자, 일부 기호(; , / ? : @ & = + $)는 제외)
   ```
encodeURI("?query=값");
 -> "?query=%EA%B0%91"
   ```

  - encodeURIComponent()
   문자를 유니 코드값으로 인코딩합니다(영문, 숫자 제외)
   ```
encodeURIComponent("?query=값");
 -> "%3Fquery%3D%EA%B0%91"
   ```

  - decodeURI()
   유니 코드값을 디코딩해 다시 문자화합니다.
   ```
decodeURI("?query=%EA%B0%91");
 -> "?query=값"
   ```

  - decodeURIComponent()
   유니 코드값을 디코딩해 다시 문자화합니다.
   ```
decodeURIComponent("%3Fquery%3D%EA%B0%91");
 -> "?query=값"
   ```

  - parseInt()
   문자열 데이터를 정수형 데이터로 반환합니다.
   ```
parseInt("5.12");  ->  5
parseInt("15px");  ->  15
   ```

  - parseFloat()
   문자열 데이터를 실수형 데이터로 반환합니다.
   ```
parseFloat("5.12");  ->  5.12
parseFloat("65.5%");  ->  65.5
   ```

  - String()
   문자열 데이터로 반환합니다.
   ```
String(5);  ->  "5"
   ```

  - Number()
   숫자형 데이터로 반환합니다.
   ```
Number("5");  ->  5
   ```

  - Boolean()
   논리형 데이터로 반환합니다.
   ```
Boolean(5);  ->  true
Boolean(null);  ->  false
   ```

  - isNaN()
   is Nit a Number의 약자이며 숫자가 아닌 문자가 포함되어 있으면 true를 반환합니다.
   ```
isNaN("5-3");  ->  true
isNaN("53");  ->  false
   ```

  - eval()
   문자형 데이터를 따옴표가 없는 자바스크립트 코드로 처리합니다.
   ```
eval("10 + 5") -> 20
   ```
