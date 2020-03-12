---
title: "JPA의 설정"
categories: jpa
comments: true
---

1) Single-valued association
 department와 employee관계의 구현은 외래키(departmentId)가 포함된 employee객체에 department멤버 변수를 구현하는 것이 기본


- @ManyToOne 어노테이션

```java
@Entity
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    int id;
    String name;

    @ManyToOne
    @JoinColumn(name = "departmentId")
    Department department;
}
```

직원(employee) 여러명이 부서(department) 한개에 소속된다. 즉 employee는 여러개, department는 한개이다. 소속 부서를 의미하는 department멤버 변수는 department 객체 한개이다. 여기서 departmentId는 외래키로 표현된다.

***

2) Collection-Valued Association
 구현은 필수가 아니고 선택이다.
- @OneToMany 어노테이션

```java
@Entity
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    int id;
    String name;
    @OneToMany(mappedBy="department")
    List<Employee> employees;
}
```

mappedBy="department"에서 department는 Employee 엔터티 클래스에서 @JoinColumn 어노테이션이 붙은 멤버 변수를 가리킨다. 
department테이블의 레코드를 조회할 때의 절차
 1. department 테이블에서 레코드가 하나 조회된다
 2. Department 객체가 한 개 생성된다.
 3. 조회된 레코드가 Department 객체에 채워진다.
 4. 조회된 department 레코드의 id 값과, departmentId 값이 일치하는 employee 레코드들이 조회된다
 5. Employee 객체가 여러 개 생성된다.
 6. 조회된 employee 레코드들이 Employee 객체에 채워진다.
 7. Employee 객체들이 List<Employee> 객체에 add 된다.
 8. List<Employee> 객체가, Department 객체의 employees 멤버 변수에 대입된다.

 위 절차에서 1-3 부분은 department테이블의 레코드를 조회하자마자 즉시 실행되지만, 4-8부분은 나중에 실행될 수도 있다.
 - Eager Loading: 1-3부분과 동시에 4-8부분도 실행하는 정책
 - Lazy Loading: 4-8부분의 실행을 최대한 뒤로 미루어서 꼭 필요할 때에만 실행하는 정책

***

3) Eager Loading/ Lazy Loading
 - Eager Loading
   - 장점: DB에 연결해서 명령을 실행하고 데이터를 받아오고 있으나, 필요할 것 같은 데이터들은 한 번에 다 가져오는 것이 좋다.
   - 단점: 가져온 데이터를 결국 사용하지 않게 되면 자원낭비가 된다.

 - Lazy Loading
   - 장점: 불필요하게 많은 데이터를 조회하는 자원낭비를 줄일 수 있다.
   - 단점: DB에서 가져오지 않은 데이터가 필요하다는 것을 나중에 알게 되면 또 DB에 연결해서 데이터를 받아와야 한다. 한 번 DB에 연결해서 필요한 것을 다 받아오는 것보다, 하나씩 따로 받아오는 것이 비효율적이고 시간이 많이 걸린다.

 - 정책 설정

```java
@ManytoOne(fetch = FetchType.EAGER)
```
```java
@OneToMany(fetch = FetchType.LAZY)
```
```java
@OneToOne(fetch = FetchType.EAGER)
```


```
@JsonIgnore
```
JSON 포맷으로 변환될 때, @JsonIgnore을 붙인 변수는 생략된다.



