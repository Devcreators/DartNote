# Dart 문법 노트
## 01 키워드
### 내장 식별자
|  **abstract**  | **implements** |    **as**    |  **import**  |
| :------------: | :------------: | :----------: | :----------: |
| **convariant** | **interface**  | **deferred** | **library**  |
|  **dynamic**   |   **mixin**    |  **export**  | **operator** |
|  **external**  |    **part**    | **factory**  |   **set**    |
|  **Function**  |   **static**   |   **get**    | **typedef**  |
### 문맥 키워드
| **sync** | **async** | **hide** | **on** | **show** |
| :------: | :-------: | :------: | :----: | :------: |
### 예약어
| **await** | **yield** |
| :-------: | :-------: |
### 기타 예약어
|  **assert**  |  **break**  |  **case**   |
| :----------: | :---------: | :---------: |
|  **catch**   |  **class**  |  **const**  |
| **continue** | **default** |   **do**    |
|   **else**   |  **enum**   | **extends** |
|  **false**   |  **final**  | **finally** |
|   **for**    |   **if**    |   **in**    |
|    **is**    |   **new**   |  **null**   |
| **rethrow**  | **return**  |  **super**  |
|  **switch**  |  **this**   |  **throw**  |
|   **true**   |   **try**   |   **var**   |
|   **void**   |  **while**  |  **with**   |

## 02 주석, 타입, 변수, 상수
### 주석
``` dart
  main() {
    // 주석 1

    /*
    * 주석 2
    * 이것은 주석입니다.
    * 
    */

    /// 이것도 주석입니다.
    /// [ClassName]
    /// dartdoc 으로 HTML 문서 생성 가능한 주석

    var numberA = 10;
    var numberB = 20;
    var result = add(numberA, numberB);
    print('The number is $result.');
  }
  
  add(int a, int b) {
    return a + b;
  }

```
### 타입
|  **타입**   | **비고**                              |
| :---------: | :------------------------------------ |
|   **num**   | int와 double의 supertype              |
|   **int**   | 정수                                  |
| **double**  | 실수                                  |
| **string**  | 문자열                                |
|  **bool**   | true 또는 false를 가지는 Boolean type |
|   **var**   | 타입 미지정 및 타입 변경 불가         |
| **dynamic** | 타입 미지정 및 타입 변경 가능         |
|  **list**   | dart의 array는 list로 대체            |
|   **set**   | 순서가 없고 중복 없는 collection      |
|   **map**   | key, value 형태를 가지는 collection   |
### 변수
``` dart
  main() {
    var number = 10;
    Object balanceA = 1000;
    dynamic balanceB = 2000;
    print('The number is $number.');
    print('The balanceA is $balanceA.');
    print('The balanceB is $balanceB.');    
    
    balanceA = '천';
    balanceB = false;
    print('The balanceA is $balanceA.');
    print('The balanceB is $balanceB.');    
  }
```
### 상수
> **상수**는 값을 바꿀 수 없는 변수이다.
> **final**와 **const**의 차이는 다음과 같다.
> **final**은 **런타임**에 상수가 되고 **const**는 **컴파일 시점**에 상수가 된다.

``` dart
  main() {
    final int NUMBER = 1;
    const int PRICE = 1000;
    
    // 타입 생략 가능
    final NAME = 'kim';
    const COLOR = 'Red';
    
    print('The NUMBER is $NUMBER.');
    print('The PRICE is $PRICE.');
      
    print('The NAME is $NAME.');
    print('The COLOR is $COLOR.');
  }
```

> **final**은 런타임에 상수화되기 때문에 실행하면 get() 함수에서 가져온 값으로 할당이 가능하다.
> 하지만 **const**는 이미 컴파일 시점에 상수화되었기 때문에 런타임 시 get() 함수에서 가져온 값을 할당하려고 하면 에러가 발생한다.
> 상수에 값을 할당하려는 것과 동일한 시도이기 때문이다.
``` dart
  main() {
    // final와 const 차이
    final int NUMBER = get();
    const int PRICE = get(); // error
    
    // 타입 생략 가능
    final NAME = 'kim';
    const COLOR = 'Red';
    
    print('The NUMBER is $NUMBER.');
    print('The PRICE is $PRICE.');
      
    print('The NAME is $NAME.');
    print('The COLOR is $COLOR.');
  }
  
  get() {
    return 100;
  }
```


<!-- 2일차 03~05 -->

## 03 함수
### 다트의 함수 특징
> 1. 변수가 함수 참조 가능
> 2. 함수의 인자로 함수 전달 가능
> 3. 이름 있는 선택 매개변수
> 4. 위치적 선택 매개변수
> 5. 익명 함수 및 람다식
### 1. 변수가 함수 참조 가능
**[기본 형태]**
```
타입 변수명 = 함수() { }
ex) var name = getName() { }
```

``` Dart
  main() {
    int a = 10;
    int b = 5;
    var name = getName();

    print('Name is $name.');
    print('$a + $b = $(add(a, b))');
    print('$a - $b = $(sub(a, b))');
  }

  getName() {
    return 'Kim';
  }

  int add(int a, int b) {
    return a + b;
  }

  int sub(a, b) { // 매개변수 타입 생략 가능
    return a - b; 
  }
```
### 2. 함수의 인자로 다른 함수 전달 가능
**[기본 형태]**
```
함수A(함수B(), 함수C()) { }
ex) getName(getFirsName(), getLastName() { }
```

``` dart
  main() {
    int a = 10;
    int b = 5;
    
    print('${a + b} * ${a - b} = ${multi(add(a, b), sub(a, b))}');
  }
  
  int add(int a, int b) {
    return a + b;
  }
  
  int sub(int a, int b) {
    return a - b;
  }
  
  int multi(int a, int b) {
    return a * b;
  }
```
### 3. 이름 있는 선택 매개변수
> 함수 호출 시 매개변수에 인자 값을 넘겨줄 때 매개변수명을 이용하여 인자 값을 넘겨줄 수 있다.
> 매개변수명으로 인자 값을 넘겨줄 매개변수는 { }로 감싸줘야 한다.
> 주의할 점은 필수 매개변수는 꼭 인자 값을 줘야 하고 매개변수 위치를 꼭 고려해야 한다.

```
getAddress (String 매개변수명1, {String 매개변수명2, String 매개변수명3}) { }
ex) getAddress ('seoul', {매개변수명2: 'gangnem', 매개변수명3: '123'}) { }
```

``` dart
  main() {
    print('${getName('서울', district: '강남')}'); 
    print('${getName('서울', district: '강남', zipCode: '012345')}'); 
    // print('${getName(district: '강남', zipCode: '012345')}'); // error
  }
  
  String getAddress(String city, {String district, String zipCode='111222'}) {
    return '$city시 $district구 $zipCode';
  }
```
### 4. 위치적 선택 매개변수
> **위치적 선택 매개변수**는 미리 초깃값을 지정해놓고 함수 호출 시 해당 매개변수에 인자 값을 넘겨주지 않으면 초깃값을 사용하는 것이다.
> **선언 방법**은 선택 매개변수 지정을 { } 대신 [ ]로 하는 것이 차이점이다.
> **주의할 점**은 필수 매개변수는 꼭 인자 값을 줘야 하고 매개변수 위치를 꼭 고려해야 한다.

``` dart
함수(매개변수, [매개변수 = 초깃값, 매개변수 = 초깃값]) { }
ex) getAddress (city, [distrct = '강남', zipCode = '111222']) { }
```

``` dart
  main() {
    print('${getAddress('서울')}');
    print('${getAddress('서울', '서초')}');
  }
  
  String getAddress(String city, [String district = '강남', String zipCode = '111222']) {
    return '$city시 $district구 $zipCode';
  }
```
### 5. 익명 함수 및 람다식
**[익명 함수의 기본 형태]**
``` dart
(매개변수명) { 표현식; };
ex) (a, b) { a + b; };
```

**[람다식의 기본 형태]**
``` dart
(매개변수명) => 표현식;
ex) (a, b) => a - b;
```

``` dart
  main() {
    int a = 10;
    int b = 5;
  
    print('$a + $b = ${add(a, b)}');
    print('$a * $b = ${multi(a, b)}');
    print('$a - $b = ${sub(a, b)}');
  }
  
  int add(int a, int b) {
    return a + b;
  }
  
  // anonymous function
  var multi = (_a, _b) {
    return _a * _b;
  }
  
  // lambda
  // int sub(int _a, int _b) => _a - _b;
  sub(_a, _b) => _a - _b;
```

## 04 연산자
### 1. 산술 연산자
> [종류] +, -, *, /, ~/, %, ++, --
>> ~ / 연산자는 정수를 결괏값으로 가지며, 몫을 구할 때 사용한다.

```dart
  double divide(_a, _b) {
    return _a / _b;
  }
  
  int divideQuotient(_a, _b) {
    return _a ~/ _b;
  }
  
  int divideModulo(_a, _b) {
    return _a % _b;
  }
  
  main() {
    int a = 11;
    int b = 5;
  
    print('$a / $b = ${divide(a, b)}');
    print('$a ~/ $b = ${divideQuotient(a, b)}');
    print('$a % $b = ${divideModulo(a, b)}');
  }
```
### 2. 할당 연산자
> [종류] =, +=, -=, /=, ~/=
``` dart
  main() {
    int a = 10;
    int b = 5;
    a = a + 5;
    b += 5;
  
    print('a = $a');
    print('b = $b');
  }
```
### 3. 관계 연산자 (비교 연산자) 
> [종류] ==, -=, >, <, >=, <=

```dart
  main() {
    int a = 10;
    int b = 5;

    if(a == b) {
      print('a == b');
    } else {
      print('a != b');
    }
  }
```
### 4. 비트 & 시프트 연산자
> [종류] &, |, ^, ~, <<, >>
>> '&', '|', '^', '~'는 각각 AND, OR, XOR, NOT을 의미한다.

``` dart
  mian() {
    int a = 0x03; // 0011
    int b = 0x01; // 0001
    print('a = $a'); // 0011
    print('a << 1 = ${a << 1}'); // 0110
    print('a >> 1 = ${a >> 1}'); // 0001
    print('a & 1 = ${a & 1}'); // 0011 & 0001 = 0001
  }
```
### 5. 타입 검사 연산자
> as : 형 변환 (상위 타입으로 변환할 수 있음)
> is : 객체가 특정 타입이면 true
> is! : 객체가 특정 타입이면 false, 즉 특정타입이 아니면 true

``` dart
  main() {
    Employee employee = Employee();
  
    Person person = employee as Person;
    print('person.name = ${person.name}');
    
    print('(employee as Person).name = ${(employee as Person).name}');
  
    if(employee is Employee) {
      print('employee is Employee');
    } else {
      print('employee is not Employee');
    }
  } 
  
  class Person {
    var name = 'person';
  }
  
  class Employee extends Person {
    var name = 'employee';
  }

```
### 6. 조건 표현식
**[삼항 연산자]**
``` dart
  조건 ? 표현식1(true) : 표현식2(false);
  ex) (a > 0) ? '양수' : '음수';
```
**[조건적 멤버 전근 연산자]**
null 체크를 편하게 해주는 연산자이다.
```dart
  좌항?.우항
  ex) employee?.name
```
**[?? 연산자]**
좌항이 null이 아니면 좌항 값을 리턴하고 null이면 우항 값을 리턴한다.
```dart
  좌항 ?? 우항
  ex) employee.name ?? 'new name'
```
### 7. 캐스케이드 표현법
캐스케이드 표기법(..)은 한 객체로 해당 객체의 속성이나 멤버 함수를 연속으로 호출할 때 유용하다.
``` dart
  ex)
  Employee employee = Employee()
  ..name = 'Kim'
  ..setAge(25)
  ..showInfo();
```

``` dart
  main() {
    Employee employee = Employee()
    ..name = 'Kim'
    ..setAge(25)
    ..showInfo();
  
    employee.name = 'Park';
    employee.setAge = 30;
    employee.showInfo();
  }
  
  class Employee {
    var name = 'employee';
    int age;
  
    setAge(int age) {
      this.age = age;
    }
  
    showInfo() {
      print('$name is $age');
    }
  } 
```

## 05 조건문
### 1. if문, if ~ else문
```dart
  if (조건) {
    조건식;
  }
  
  if (조건) {
    실행문;
  } else {
    실행문;
  }
```
### 2. switch문
```dart
  switch (변수) {
    case 값1:
    실행문;
    break;
  
    case 값2:
    실행문;
    break;
  
    default:
    실행문;
    break;
  }
```
### 3. assert
> **assert**는 조건식이 거짓이면 에러가 발생한다.
``` dart
  assert(조건식);
  ex) assert(a > 0);
```

## 06 반복문
### 1. for문
``` dart
  for (초기화; 조건식; 증감식) {
    실행문;
  }
  
  ex)
  for(int i = 1; i<8;i++) {
    print('i = $i');
  }
```
### 2. while문
``` dart
  while (조건식) {
    실행문;
  }
  
  ex)
  int a = 0;
  
  while (a < 5) {
    print('hello');
    a++;
  }
```
### 3. do~while문
``` dart
  do {
    실행문;
  } while (조건식);
  
  ex)
  int a = 0;
  
  do {
    print('hello');
    a++;
  } while (a < 5);
```

## 07 클래스
### 1. 객체, 멤버, 인스턴스
> **객체** : 저장 공간에 할당되어 값을 가지거나 식별자에 의해 참조되는 공간
> **멤버** : 클래스는 멤버를 가진다. 멤버는 멤버 함수(메서드)와 멤버 변수(인스턴스 변수)로 구성된다.
> **인스턴스화** : 객체를 메모리에 작성하는 것
### 2. 클래스 기본
``` dart
  class 클래스명 {
    멤버변수
    멤버함수
  }
  ex)
  class Person {
    String name;
    int age;
    
    void addOneYear() {
      age++;
    }
  }
```
객체 생성 예제
``` dart
    main() {
      var student = new Person();
      student.name = 'Kim';
      print('student name = ${student.getName()}');
    }
    
    class Person {
      String name;
      int age;
      
      void getName() {
        this.name = name;
      }
    } 
```
커스텀 타입 예제
``` dart
  main() {
    Person person = Person();
    person.name = 'Hong';
    print('person name = ${person.getName()}');
  }
  
  class Person {
    String name;
    int age;
  
    getName() {
      return name;
    }
  }
```

## 08 생성자
### 1. 기본 생성자
> 클래스를 구현할 때 생략하면 기본 생성자가 자동으로 제공된다.
> 기본 생성자는 클래스명과 동일하면서 인자가 없다.
> 자식 클래스는 부모 클래스의 기본 생성자를 상속받지 않는다.
``` dart
  class 클래스명 {
    클래스명() { }
  }
  
  ex)
  class Person {
    Person() { }
  }
```
### 2. 이름 있는 생성자
> 한 클래스 내에 많은 생성자를 생성하거나 생성자를 명확히 하기 위해서 사용할 수 있다.
> 이름 없는 생성자를 2개 선언했을 경우 중복 선언 에러가 발생한다.
> 이름 있는 생성자 선언했을 경우 기본 생성자 생략이 불가능하다.

``` dart
  class 클래스명 {
    클래스명.생성자명() { }
  }
  
  ex)
  class Person {
    Person.init() { }
  }
```
### 3. 초기화 리스트
> 초기화 리스트를 사용하면 생성자의 구현부가 실행되기 전에 인스턴스 변수를 초기화 할 수 있다. 
> 초기화 리스트는 생성자 옆에 :(콜론)으로 선언할 수 있다.

``` dart
  생성자 : 초기화 리스트 { }
  
  ex)
  Person() : name = 'Kim' { }
```
### 4. 리다이렉팅 생성자
> 초기화 리스트를 약간 응용하면 단순히 라다이렉팅을 위한 생성자를 만들 수 있다.
> 이러한 생성자는 본체가 비어 있고 메인 생성자에게 위임하는 역할을 한다.

``` dart
  main() {
    var person = Person.initName('Kim');
  }
  
  class Person {
    String name;
    int age;
  
    Person(this.name, this.age) {
      print('This is Person($name, $age) Constructor!');
    }
  
    Person.initName(String name) : this(name, 20);
  }
```
### 5. 상수 생성자
> 상수 생성자는 생성자를 상수처럼 만들어 준다.
> 해당 클래스가 상수처럼 변하지 않는 객체를 생성한다는 것이다.
> 상수 생성자를 만들기 위해서는 인스턴스 변수가 모두 final 이어야 하고, 
> 생성자는 const 키워드를 붙여야 한다.
``` dart
  main() {
    Person person1 = const Person('Kim', 20);
    Person person2 = const Person('Kim', 20);
    Person person3 = new Person('Kim', 20);
    Person person4 = new Person('Kim', 20);
  
    print(identical(person1, person2));
    print(identical(person2, person3));
    print(identical(person3, person4));
  }
  
  class Person {
    final String name;
    final num age;
  
    const Person(this.name, this.age);
  }
```
### 6. 팩토리 생성자
``` dart
  main() {
    Person student = Person('Student');
    Person employee = Person('Employee');
  
    print('type = ${stud.getType()}');
    print('type = ${emp.getType()}');
  }
  
  class Person {
    Person.init();
  
    factory Person(String type) {
      switch (type) {
        case 'Student':
            return Student();
        case 'Employee':
            return Employee();
      }
    }
  
    String getType() {
      return 'Person';
    }
  }
  
  class Student extends Person {
    Student() : super.init();
  
    @override
    String getType() {
      return 'Student';
    }
  }
  
  class Employee extends Person {
    Employee() : super.init();
  
    @override
    String getType() {
      return 'Employee';
    }
  }
```

## 09 상속
> 상속받는 쪽(자식 클래스)은 extends 키워드를 통해서 상속받고자 하는 부모 클래스를 지정한다.
> @override 어노테이션은 부모 클래스의 메서드를 재정의하고 싶을 때 사용한다.
> 재정의한다는 의미는 기본 메서드에서 구현한 내용 대신 다른 동작을 하는 코드를 구현한다는 것이다.
``` dart
    [기본형태]
    class 부모 클래스명 {
      멤버변수;
      멤버함수() { }
    }
    
    class 자식 클래스명 extends 부모 클래스명 {
      @override
      멤버함수() { }
    }
    
    ex)
    class Person {
      Person.init();
    
      factory Person(String type) {
        switch (type) {
          case 'Student':
              return Student();
          case 'Employee':
              return Employee();
        }
      }
    
      String getType() {
        return 'Person';
      }
    }
    
    class Student extends Person {
      Student() : super.init();
    
      @override
      String getType() {
        return 'Student';
      }
    }
    
    class Employee extends Person {
      Employee() : super.init();
    
      @override
      String getType() {
        return 'Employee';
      }
    }
```

## 10 접근지정자
### 1. 캡슐화
> **캡슐화**는 어떤 객체가 어떤 목적을 수행하기 위한 데이터(멤버 변수)와 기능(메서드)을 적절하게 모으는 것을 말한다.
``` dart
  class Developer {
    String name;
    int age;
    eat() {
      print('eat'); 
    }
    sleep() {
      print('sleap');
    }
    coding() {
      print("This is not bug. It's just feature");
    }
  }
```
### 2. 추상화
> **추상화**는 어떤 객체의 공통된 데이터와 메서드를 묶어서 이름(클래스명)을 부여하는 것이다.
``` dart
  class Person {
    String name;
    int age;
    eat() { }
    sleep() { }
  }
```
### 3. 접근지정자
> 다트의 접근지정자의 종류는 두 가지이다. private와 public이다.
> **private 접근지정자**는 동일 클래스 내에서만 접근이 가능하다. _(밑줄)을 붙인다.
> **public 접근지정자**는 접근 범위에 상관없이 모든 곳에서 접근이 가능하다. _(밑줄)을 안 붙인다.
``` dart
  class Person {
    String name;
    int _age;
    eat() {
      print('eat'); 
    }
    _sleep() {
      print('sleap');
    }
  }
```
