---
title: "기술면접 정리"
categories: technical_interview
comments: true
---

기술면접준비 !

## OOP(객체지향 프로그래밍)
 객체지향 프로그래밍은 프로그래밍에서 필요한 데이터를 추상화 시켜서 상태(속성, 어트리뷰트)와 행위(메서드)를 가진 객체로 만들고
 그 객체간의 상호작용을 통해 로직을 구성하는 방법이다.

### 장점
 - 다른 클래스를 가져와 사용할 수 있고, 상속받을 수 잇어 코드의 재사용성 증가
 - 유지보수가 절차지향보다는 간단하다.
 - 클래스 단위로 모듈화가 가능하여, 대형 프로젝트에 적합하다.

### 단점
 - 처리속도가 상대적으로 느리다.
 - 객체가 많으면 용량이 커진다.
 - 설계시 많은 노력과 시간이 필요하다.

### OOP의 6가지 키워드
 - 클래스
  현실 세계의 객체를 추상화시켜 속성과 메서드로 정의한 것

 - 인스턴스
  클래스에서 정의한 것을 토대로 만든 실제 메모리상에 할당된 것, 실제 데이터

 - 추상화
  객체지향 관점에서 클래스를 정의하는 것, 불필요한 정보 외 중요한 정보만 표현함으로써 공통의 속성과 기능을 묶어 이름을 붙이는 것

 - 캡슐화
  코드를 수정없이 재활용 하는 것을 목적으로 함. 클래스라는 캡슐에 기능과 특성을 담아 묶는다.
  목적을 기준으로 묶는다.

  은닉화와의 차이 - 은닉화는 캡슐화의 일부라고 볼 수 있으며, 목적으로 묶인 캡슐안을 사용자는 볼 수 없는 것이 은닉화

 - 상속
  클래스로부터 속성과 메서드를 물려받는 것을 말함. 다른 클래스를 가져와서 수정할 일이 있다면, 그 클래스를 직접 수정하는 대신 상속을 받아 변경하고자 하는 부분만 변경 

 - 다형성
  하나의 변수명이나 함수명이 상황에 따라 다르게 해석될 수 있음. 대표적으로 virtual 가상함수가 있다.

## C++과 C의 차이점

### 개념적인 차이

#### 패러다임
 우리가 기본적으로 아는 차이점은 C는 **절차지향적인** 언어이고, C++은 **객체지향적인** 언어이다. 

#### 개발방식
 - C 하향식 접근방법
   C는 절차적 언어이기 때문에 기본 요소간의 순차적 수행이 되도록 서로간의 연결이 중요하다.
  단계별 공식화(linked-together) 이후 세부사항으로 나눠진다.

 - C++ 상향식 접근방법
   추상적인 개념을 통해서 구성을 한 후 그 다음 그것들을 조합하는 방법으로 만들어진다.
  
### 문법적인 차이
#### Class
 C++에는 class가 생겼다.

 1. C++의 class는 class내의 함수를 가질 수 있다. struct도 함수를 가질 수 있다. C의 struct는 함수호출을 할 수있다.
 2. C++의 class는 class 맴버가 기본적으로 private. struct는 public. C의 struct 접근 지정자 자체가 없다.
 3. C++의 class는 상속이 가능하다, struct 또한 상속이 가능하다, C는 개념 자체가 없다.
 4. C++의 class는 템플릿 사용 가능, struct 또한 가능, C는 개념 자체가 없다.

#### 오버로딩을 지원하지 않는다
 C++에서는 함수명은 같지만 함수파라미터는 다른 오버로딩을 지원하지 않는다.


## Malloc과 new의 차이점
 - malloc()는 C의 <stdlib.h>헤더파일에서 지원하는 함수로, 인자로 할당할 size를 받는다. 기본적으로 void* 형태의 포인터를
  return하기 때문에 할당할 포인터 자료형에 맞게 casting이 필요하다. 메모리 해제는 free()를 사용한다.

 - new는 C++ 자체에서 지원하는 키워드로 따로 헤더파일이 필요없다. malloc과 다르게 할당할 객체의 크기를 알아서 계산하여
  heap에 할당하기 때문에 size를 입력할 필요가 없다. 또한 객체 생성시 생성자를 자동으로 호출하기 때문에 생성과 초기화가 동시에 이루어진다.
  메모리 해제는 delete 키워드를 사용한다.

 - 차이점
   - malloc()는 함수이고, new는 연산자이다.
   - malloc은 헤더파일이 필요하지만 new는 그렇지 않다
   - malloc은 size를 인자로 받고 자료형 포인터에 따라 캐스팅을 필요하지만, new는 할당할 타입과 같은 포인터 변수로 받기만 하면 된다.
   - malloc과 달리 new는 생성자를 자동으로 호출해 생성과 동시에 초기화 되지만 malloc은 메모리 공간확보만 할 뿐 초기화는 되지않는다.

 - 총정리
  malloc()은 함수이고, new는 연산자이다. 발생하는 가장 큰 차이는 생성자의 유무이다.
  malloc()은 시스템 함수로 함수 안에서 메모리를 할당하지만 new는 연산자로 바로
  메모리에 할당하는 것이 아니라 생성자를 호출하여 메모리를 할당하여서 생성 시 초기화가 가능한 장점이 있다.

 - 추가질문(+)
   초기화 하는 방법

```c++
class Car{
private:
	int 바퀴;
	int 엔진;
	int 기름;
public:
	Car(){
		바퀴=4;
		엔진=1;
		기름=0;
	}  // 생성 후 초기화

	Car() : 바퀴(4), 엔진(1), 기름(0) {
		
	} // 생성 시 초기화

}
```

생성 시 초기화가 효율이 좋다.!! 생성 후 초기화는 생성->초기화 2가지 흐름으로 처리되기 때문이다.


## STL map
 STL의 map은 키를 기준으로 만든 이진 탐색 트리에 key/value 쌍을 보관한다. 충돌을 처리할 필요가 없고, 트리의 균형이 유지되므로 삽입 및 탐색시간이 O(log N)이 보장된다.

## 가상함수(Virtual)의 동작원리
 런타임 시점에서 동적바인딩을 통해 호출될 함수의 주소를 결정하는 경우에
 virtual 키워드를 사용한다.

 1. 어떤 클래스의 함수가 virtual로 선언되어 있다면, 해당 클래스의 
   가상 함수 주소를 보관하는 vtable이 만들어진다.
   가상 함수 테이블은 객체가 아닌, 클래스에 생성된다.

 2. 객체를 생성하게 되면 가상함수 테이블을 가리키는 포인터 변수가 생성된다.
   이 포인터는 각 클래스에 해당하는 가상함수 테이블을 가리키기 때문에, 포인터를 통해
   각 타입에 맞는 함수의 주소를 동적으로 찾을 수 있다.

### 예제로 virtual 이해하기

```c++
#include <iostream>
using namespace std;
class Base {
public:
	Base() {
		cout << "Base 생성자" << endl;
	}
	virtual ~Base() {
		cout << "Base 소멸자" << endl;
	}
	virtual void Do() {
		cout << "Base 행동" << endl;
	}
};

class Derived : public Base {
public:
	Derived() {
		cout << "Derived 생성자" << endl;
	}
	virtual ~Derived() {
		cout << "Derived 소멸자" << endl;
	}
	virtual void Do() {
		cout << "Derived 행동" << endl;
	}
};

int main() {
	Base* b = new Base();
	Base* d = new Derived();

	b->Do();
	d->Do();

	delete d;
	delete b;
}
```

```c++
Base 생성자
Base 생성자
Derived 생성자
Base 행동
Derived 행동
Derived 소멸자
Base 소멸자
Base 소멸자
```


똑같이 Base* 타입으로 받고 Do() 함수를 호출했지만, virtual 키워드를 사용하였더니
 같은 Base* 타입으로도 서로 다른 객체의 행동을 불러올 수 있다.

 이렇게 하나의 Base* 타입으로 받는 이유는 좋은 설계를 위해서이다.

#### 다형성을 가진 관계에서 소멸자에 virtual 키워드를 붙여야할까?
 붙이지 않는다면 하위 클래스의 소멸자는 불러오지 않아지고 이는 무한디버깅의 원인이 되는 메모리 누수의 원인이 된다.

이처럼 다형성을 가진 행동을 하기 위해서 상속 관계 안에서 각각의 객체를 구분하기 위해 함수에 virtual을 붙인 것처럼

소멸자도 상속 관계 안에서 자신의 객체를 소멸시키기 위해 소멸자에도 virtual 키워드를 꼭 붙여야 한다!


결론: 다형성을 가진 상속관계에서는 반드시 소멸자에 virtual 키워드를 붙여야 한다.


## 얕은 복사(Shallow copy) vs 깊은 복사 (Deep copy)
 객체는 다른 객체를 참조할 수 있는데 이럴 경우에 객체의 복사본을 핸들링 할 경우 깊은 복사인가, 얕은 복사인가에 따라 결과값이 달라질 수 있다.

 - 얕은 복사는 한 객체의 모든 멤버 변수의 값을 복사한다. 만약 객체가 참조타입의 멤버를 가지고 있다면 참조값만 복사가 된다.

```c++
Person(const char myname[], int myage) {
        int len = strlen(myname) + 1;
        name = new char[len];
        strcpy_s(name, len, myname);  // CodeBlock 일 경우에는  strcpy(name, myname)을 사용
        age = myage;
} //생성자만 있음
...
int main() {
	Person man1("Park", 24);
	Person man2 = man1; // 얕은 복사 
}
```

 - 깊은 복사의 경우에는 포인터 변수가 가리키는 모든 객체에 대해서도 시행한다.
   얕은 복사와 달리 객체가 가진 모든 멤버(값과 참조형식 모두)를 복사한다. 
   즉, 참조값의 복사가 아닌 참조된 객체 자체가 복사되는 것이다.

```c++
Person(const char myname[], int myage) {
        int len = strlen(myname) + 1;
        name = new char[len];
        strcpy_s(name, len, myname);
        age = myage;
} //생성자
Person(const Person& copy) : age(copy.age) {
        int len = strlen(copy.name) + 1;
        name = new char[len];
        strcpy_s(name, len, copy.name);
} // copy생성자
...
int main() {
	Person man1("Park", 24);
	Person man2 = man1; // 깊은 복사 
}
```


( 출처: [다다님의 tistory](https://jjoreg.tistory.com/entry/C-%EA%B3%BC-C%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)  
, [힐무새님의 tistory](https://geekhub.tistory.com/68)  
, <https://makefortune2.tistory.com/243?category=695910> 
, <https://rednine.tistory.com/195> 
, <https://wonjayk.tistory.com/256>
, <https://naldo627.github.io/2019/06/03/techinterview-prepare-wm/> )
