---
title: "JPA query creation"
categories: jpa
comments: true
---

## 1) 표준 API
 Java 언어를 배운다는 것은, 먼저 Java 문법을 배우고, 그 다음 Java 표준 라이브러리의 클래스들을 배우는 것을 말한다. 라이브러리의 API 라는 것은, 그 라이브러리의 public 클래스 목록과 public 메소드 목록을 말한다. 두 라이브러리의 public 클래스 목록과 public 메소드 목록이 동일하면, 그 두 라이브러리의 사용법도 동일하다. 라이브러리들의 사용법을 통일하려면, 그 라이브러리들의 API를 통일해야 한다. 이렇게 사용법이 통일된 라이브러리들의 API를 표준 API 라고 부른다. 표준 API의  존재 이유는, 여러 라이브러리들의 사용법을 통일하는 것이다


## 2) JPA와 Hibernate
 JPA(Java Persistence API)는 ORM기술의 하나이다. Java Persistence = Java 객체를 DB에 저장하거나, DB 데이터를 Java 객체로 조회하는 기술 JPA = Java Perssitence API = Java Persistence를 구현한 라이브러리에 대한 표준 API JPA는 표준 API일 뿐, 제품은 아니다. 
 JPA는 Java Persistence 기능을 구현한 라이브러리들의 API를 통일하기 위해 만든 표준 API 이다. 표준 API = 라이브러리의 public 클래스 목록 + public 메소드 목록 JPA 표준 API를 구현한 대표적인 오픈소스 라이브러리 = Hibernate 를 사용하게 될 것이다.

## 3) Spring Data JPA
 Spring Data는 Spring 프로젝트에서 사용되는 데이터베이스 라이브러리 개발 프로젝트이다. http://projects.spring.io/spring-data/
 Spring Data 라이브러리에는 JDBC 데이터베이스 프로그래밍을 편하게 할 수 있도록 도와주는 클래스들이 포함되어있다. 이 클래스들의 그룹을 Spring Data JPA라고 부른다. 하지만 mybatis는 지원하지 않는다.
 사용하고있는 JPARepository 인터페이스는 JPA에 포함된 것이 아니고 Spring Data JPA에 포함되어 있다.

## 4) JPA Repository 기본 메소드
 StudentRepository 인터페이스에는 아래 메소드가 기본적으로 포함되어 있다.
```
Optional<Student>  findById (int id);
List<Student>      findAll();
Student            save(Student student);
void               delete(Student student);
boolean            exists(int id);
```
 - findById: Student 테이블에서 기본키로 레코드 한 개를 조회하여 리턴한다. 리턴 타입은 `Option<Student>`
 - findAll: Student 테이블의 전체 레코드를 조회하여 리턴한다.
 - save: 파라미터 객체 student 를 Student 테이블에 저장한다. student 객체의 id 값이 0이면 INSERT 하고, id 값에 해당하는 레코드가 있으면 UPDATE 한다.
 - delete: student 객체의 id 값과 일치하는 레코드를 Student 테이블에서 삭제한다.
 - exists: id값과 일치하는 레코드가 Student 테이블에 있으면 true를 리턴한다

우리가 JPARepository 인터페이스를 상속하여 StudentRepository 인터페이스를 정의하면, Spring Data JPA가 다음과 같은 일들을 자동으로 해준다. 
 - StudentRepository 인터페이스를 구현한 클래스를 자동으로 구현한다.
 - 그 클래스에, JPA repository 기본 메소드(findByID, findAll, save, delete, exists)를 구현해 준다.
 - 그 클래스의 객체를 한 개 자동 생성해 준다.
 - 생성된 객체를 `@Autowired StudentRepository studentRepository;` 멤버 변수에 자동으로 대입해 준다.

## 5) 쿼리 메소드 자동 구현
 Spring Data JPA는 JPA reponsitory 기본 메소드 뿐만 아니라, reponsitory 인터페이스에 선언된 다른 메소드들도 자동으로 구현해준다.

ex)
```java
public interface StudentRepository extends JpaRepository<Student, Integer>  {

    List<Student> findByName(String name);
    List<Student> findByStudentNo(String studentNo);
    List<Student> findByNameStartsWith(String name);
    List<Student> findByDepartmentName(String name);
    List<Student> findByDepartmentNameStartsWith(String name);

    List<Student> findAllByOrderByName();
    List<Student> findAllByOrderByNameDesc();
    List<Student> findByDepartmentIdOrderByNameDesc(int id);
}
```

위 메소드 이름에서 Name, StudentNo, Name은 Student 엔터티 클래스의 속성 이름이다. DepartmentName과 DepartmentId의 Name과 Id는 Department 엔터티 클래스의 속성 이름이다. 외래키로 조인된것이다. 조인된 테이블의 속성임을 명시적으로 표시하기위해 밑에와 같이 _문자를 넣어서 표현하기도한다.
```java
List<Student> findByDepartment_Name(String name);
List<Student> findByDepartment_NameStartWith(String name);
List<Student> findByDepartment_IdOrderByNameDesc(int id);
```

 - ` List<Student> findByName(String name); `
 student 테이블에서 name 필드로 레코드를 조회하여 리턴한다. 파라미터 변수 name의 값과 필드 값 전체가 일치해야 한다.

 - `List<Student> findByStudentNo(String studentNo);`
 student 테이블에서 studentNo 필드로 레코드를 조회하여 리턴한다. 파라미터 변수 studentNo의 값과 필드 값 전체가 일치해야 한다.

 - `List<Student> findByNameStartsWith(String name);`
 student 테이블에서 name 필드로 레코드를 조회하여 리턴한다. 파라미터 변수 name의 값이 필드 값 앞 부분과 일치하면 된다. 예를 들어 파라미터 변수 name의 값이 "김" 이면, 성이 김씨인 레코드 전체를 조회한다.

 - `List<Student> findByDepartmentName(String name);`
 student 테이블과 department 테이블을 조회하고, 조인된 department 테이블의 name 필드로 조회하여 일치하는 student 테이블 레코드를 리턴한다. 파라미터 변수 name의 값과 department.name 필드 값 전체가 일치해야 한다.

 - `List<Student> findByDepartmentNameStartsWith(String name);`
 위 findByDepartmentName 메소드의 설명과 동일하다. 단 파라미터 변수 name의 값이, department.name 필드 값 앞 부분과 일치하면 된다. 

 - `List<Student> findAllByOrderByName();`
 student 테이블의 전체 레코드를, 필드의 오름차순으로 정렬하여 리턴한다.

 - `List<Student> findAllByOrderByNameDesc();`
 student 테이블의 전체 레코드를, 필드의 내림차순으로 정렬하여 리턴한다.

 - `List<Student> findByDepartmentIdOrderByNameDesc(int id);`
 student 테이블과 department 테이블을 조회하고, 조인된 department 테이블의 id 필드로 조회하여 일치하는 student 테이블 레코드를 리턴한다. 조회 결과를 student 테이블의 name 필드의 내림차순으로 정렬하여 리턴한다

## 6) Query Method Name Rule


```java
  KeyWord         |               Sample
And	          : findByLastNameAndFirstName(String lastName, String firstName)
Or	          : findByLastNameOrFirstName(String lastName, String firstName)
Between	          : findByStartDateBetween(Date date1, Date date2)
LessThan          : findByAgeLessThan(int age)
GreaterThan       : findByAgeGreaterThan(int age)
After	          : findByStartDateAfter(Date date1)
Before	          : findByStartDateBefore(Date date1)
IsNull	          : findByAgeIsNull()
IsNotNull,NotNull : findByAgeNotNull(), findByAgeIsNotNull()
Like	          : findByFirstNameLike(String pattern)
NotLike	          : findByFirstNameNotLike(String pattern)
StartingWith      : findByFirstNameStartingWith(String name)
EndingWith	  : findByFirstNameEndingWith(String name)
Containing	  : findByFirstNameContaining(String name)
OrderBy	          : findByAgeOrderByLastNameDesc(int age)
Not	          : findByLastNameNot(String name)
In	          : findByAgeIn(Collection<Integer> ages)
NotIn	          : findByAgeNotIn(Collection<Integer> age)
True	          : findByActiveTrue()
False	          : findByActiveFalse()
Top	          : findTop10ByOrderByAge()
```
 파라미터 변수의 이름은 중요하지 않다. 파라미터 변수순서가 중요하다!
ex)
```java
findByLastNameAndFirstName(String s1, String s2)
```
 파라미터 변수의 이름이 무엇이든 상관 없이 첫째 파라미터가 lastName이고 두번째 파라미터가 firstName이어야 한다.


 - 쿼리 메소드 이름규칙 Example
 이 표의 샘플 메소드를 SQL로 표현했다.
```
Person 테이블
   firstName    NVARCHAR(50)
   lastName     NVARCHAR(50)
   startDate    DATE
   age          int
   active       BOOLEAN
```

```
findByLastNameAndFirstName(String lastName, String firstName) 
    SELECT * FROM Person WHERE lastName = #{lastName} AND firstName = #{firstName}
 
findByLastNameOrFirstName(String lastName, String firstName)
    SELECT * FROM Person WHERE lastName = #{lastName} OR firstName = #{firstName}

findByStartDateBetween(Date date1, Date date2)
    SELECT * FROM Person WHERE startDate BETWEEN #{date1} AND #{date2}  

findByAgeLessThan(int age)
    SELECT * FROM Person WHERE age < #{age}

findByAgeGreaterThan(int age)
    SELECT * FROM Person WHERE age > #{age}

findByStartDateAfter(Date date1)
     SELECT * FROM Person WHERE startDate > #{date1}

findByStartDateBefore(Date date1)
     SELECT * FROM Person WHERE startDate < #{date1}

findByAgeIsNull()
     SELECT * FROM Person WHERE age IS NULL

findByAgeNotNull(), findByAgeIsNotNull()
     SELECT * FROM Person WHERE age IS NOT NULL


findByFirstNameLike(String pattern)
     SELECT * FROM Person WHERE firstName LIKE #{pattern}

findByFirstNameNotLike(String pattern)
     SELECT * FROM Person WHERE firstName NOT LIKE #{pattern}

findByFirstNameStartingWith(String name)
findByFirstNameStartsWith(String name)
     SELECT * FROM Person WHERE firstName LIKE #{name} + '%'
     예) SELECT * FROM Person WHERE firstName LIKE '김%'

findByFirstNameEndingWith(String name)
findByFirstNameEndsWith(String name)
     SELECT * FROM Person WHERE firstName '%' + LIKE #{name}
     예) SELECT * FROM Person WHERE firstName LIKE '%김'

findByFirstNameContaining(String name)
findByFirstNameContains(String name)
     SELECT * FROM Person WHERE firstName '%' + LIKE #{name} + '%'
     예) SELECT * FROM Person WHERE firstName LIKE '%김%'

findByAgeOrderByLastNameDesc(int age)
     SELECT * FROM Person WHERE age IS NULL

findByLastNameNot(String name)
     SELECT * FROM Person WHERE lastName <> #{name}

findByAgeIn(Collection<Age> ages)
     SELECT * FROM Person WHERE age IN #{ages}            
     /* 주의: mybatis는 Collection 타입 파라미터를 지원하지 않음 */

findByAgeNotIn(Collection<Age> age)
     SELECT * FROM Person WHERE age NOT IN #{ages}
     /* 주의: mybatis는 Collection 타입 파라미터를 지원하지 않음 */

findByActiveTrue()
     SELECT * FROM Person WHERE active 
     SELECT * FROM Person WHERE active = true

findByActiveFalse()
     SELECT * FROM Person WHERE NOT active 
     SELECT * FROM Person WHERE active = false

findTop10ByOrderByAge
     Top 10 
     조회 결과에서 선두 10 개 레코드만 리턴한다.
```

## 7) CountBy, DeleteBy, RemoveBy
 findBy 메소드 이름 규칙은 countBy, deleteBy, removeBy 메소드에도 적용된다.
countBy 메소드는 주어진 조건에 일치하는 레코드 수를 리턴한다. deleteBy, removeBy 메소드는 주어진 조건에 일치하는 레코드를 삭제한다.

 ex)
```java
public interface StudentRepository extends JpaRepository<Student, Integer>  {
    int countByName(String name);
    int countByNameStartsWith(String name);
    int countByStudentNoOrName(String studentNo, String name);

    void deleteByName(String name);
    void deleteByDepartmentId(int departmentId);
}
```
