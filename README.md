# Dart 문법 노트
## 출처
> **참고한 교재** : [ebook 다트&플러터](https://ibit.ly/8khe)

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


## 11 getter와 setter
### 1. getter와 setter의 기본
getter는 멤버 변수의 값을 가져오는 역할을 한다.
setter는 값을 쓰는 역할을 한다.
다트에서는 getter와 setter 메서드를 각각 get과 set이라는 키워드로 사용한다.
``` dart
  class Person {
    String _name;
    String get name => _name;
    set name(String name) => _name = name;
  }
  
  main() {
    Person person = Person();
    person.setName = 'Kim';
    print(person.name);
  }
```
### 2. getter와 setter의 활용
만약 _name이 null이 되지 않아야 하는 경우라면 getter와 setter을 다음과 같이 활용할 수 있다.
``` dart
    main() {
      Person person = Person();
      print(person.name);
      pserson.name = null;
      print(person.name);
    }
    
    class Person {
      String _name;
    
      String get name => (_name == null) ? 'Lee' : _name;
    
      set name(String name) => (_name == null) ? name = 'Park' : _name = name;
    }
```
## 12 추상 클래스
### 1. 추상 클래스
추상 클래스는 추상 메서드를 가질 수 있는 클래스이다.
추상 메서드를 필수적으로 포함해야 하는 것은 아니다.
일반 클래스는 추상 메서드를 선언할 수 없다.
추상 클래스는 기존 클래스 앞에 abstract 키워드를 붙여서 표현한다.
``` dart
  abstract class Person {
    eat();
  }
```
추상 클래스는 객체를 생성할 수 없다.
추상 클래스를 사용하기 위해서는 일반 클래스의 implements 키워드로 임플리먼트한 후에 반드시 추상 메서드를 오버라이딩해야 한다.
``` dart
  main() {
    Person person = Developer();
    person.eat();
  }
  
  abstract class Person {
    eat();
  }
  
  class Developer implements Person {
    @override
    eat() {
      print('Developer eat a meal');
    }
  }
```
### 2. 일반 메서드를 포함하는 추상 클래스
다트는 일반 메서드를 정의할 수도 있고 일반 메서드만 존재할 수도 있다.
일반 메서드도 반드시 임플리먼트하는 클래스에서 재정의되어야 한다. 
그리고 추상 메서드든 일반 메서드든 임플리먼트 하는 클래스에서 @override 어노테이션 생략이 가능하다.
``` dart
  main() {
    Person person = Developer();
    person.eat();
  }
  
  abstract class Person {
    eat();
  
    sleep() {
      print('Person must sleep');
    }
  }
  
  class Developer implements Person {
    @override
    eat() {
      print('Person eat a meal');
    }
  
    sleep() {
      print('Person must sleep');
    }  
  }
```
### 3. 여러 추상 클래스의 임플리먼트
``` dart
  main() {
    Developer person = Developer();
    person.eat();
    person.sleep();
    person.work();
  }
  
  abstract class Person {
    eat();
  
    sleep() {
      print('Person must sleep');
    }
  }
  
  abstract class Junior {
    work() {
      print('work hard');
    }
  }
  
  class Developer implements Person, Junior {
    @override
    eat() {
      print('Developer eat a meal');
    }
    sleep() {
      print('Developer must sleep');
    }
    work() {
      print('Junior developer works hard');
    }
  }
```
### 4. 정리
> 추상 클래스와 추상 메서드는 abstract 키워드를 사용하다.
> 추상 클래스는 참조형 변수의 타입으로 사용할 수 있다.
> 추상 클래스는 임플리먼트 할 때 반드시 메서드를 오버라이딩해야 한다.
> 추상 클래스에 추상 메서드만 존재하는 것은 아니다.
> 메서드 오버라이딩 시 @override 어노테이션은 생략 가능하다.
## 13 컬렉션
다트에서는 세 가지 컬렉션을 제공한다.
> List : 데이터 순서가 있고 중복 허용된다.
> Set : 데이터 순서가 없고 중복 허용하지 않는다.
> Map : key와 value으로 구성되며, key는 중복이 허용되지 않고 value은 중복 허용된다.
### 1. List
List는 여러 개의 데이터를 담을 수 있는 자료구조이다.
List에 데이터를 담을 때 순서를 가지기 때문에 배열을 대체할 수 있고 데이터에 순차적으로 접근하기 쉽다.
``` dart
  List <데이터 타입> 변수명 = [데이터1, 데이터2, 데이터3, ....];
  또는
  List <데이터 타입> 변수명 = List();
  변수명.add(데이터1);
  변수명.add(데이터2);
  변수명.add(데이터3);
  
  ex)
  List<String> colors = ['red', 'orange', 'yellow'];
  또는
  List<String> colors = List();
  colors.add('red');
  colors.add('orange');
  colors.add('yellow');
```
**List 주요 메서드와 프로퍼티**
> indexOf(요소) : 요소의 인덱스 값
> add(데이터) : 데이터 추가
> addAll([데이터1, 데이터2]) : 여러 데이터 추가
> remove(요소) : 요소 삭제
> removeAt(인덱스) : 지정한 인덱스의 요소 삭제
> contains(요소) : 요소가 포함되었으면 true, 아니면 false
> clear() ; 리스트 요소 전체 삭제
> sort() : 리스트 요소 정렬
> first : 리스트 첫 번째 요소
> last : 리스트 마지막 요소
> reversed : 리스트 요소 역순
> isNotEmpty : 리스트가 비어 있지 않으면 true, 비었으면 false
> isEmpty : 리스트가 비었으면 true, 비어있지 않으면 false
> single : 리스트에 단 1개의 요소만 있다면 해당 요소 리턴
### 2. Set
Set은 List처럼 데이터를 여러 개 담을 수 있는 자료구조이다. 하지만 데이터의 순서가 없고 중복된 요소를 허용하지 않는다.
``` dart
  Set <데이터 타입> 변수명 = { 데이터1, 데이터2, 데이터3, .... };
  또는
  Set <데이터 타입> 변수명 = Set();
  변수명.add(데이터1);
  변수명.add(데이터2);
  변수명.add(데이터3);
  
  ex)
  Set<String> colors = { 'red', 'orange', 'yellow' };
  또는
  Set<String> colors = Set();
  colors.add('red');
  colors.add('orange');
  colors.add('yellow');
```
Set **주요 메서드와 프로퍼티**
> add(데이터) : 데이터 추가
> addAll([데이터1, 데이터2]) : 여러 데이터 추가
> remove(요소) : 요소 삭제
> contains(요소) : 요소가 포함되었으면  true, 아니면 false
> clear() ; Set 요소 전체 삭제
> first : Set 첫 번째 요소
> last : Set 마지막 요소
> isNotEmpty : Set가 비어 있지 않으면 true, 비었으면 false
> isEmpty : Set가 비었으면 true, 비어있지 않으면 false
> single : Set에 단 1개의 요소만 있다면 해당 요소 리턴
Set는 순서를 가지지 않기 때문에 순서와 관련된 메서드와 프로퍼티는 사용하지 못한다.
> indexOf()
> removeAt()
> sort()
> reversed()
### 3. Map
Map은 key와 value로 이뤄지고 key와 value는 한 쌍으로 이뤄진다. 
key에 대한 값이 1 대 1로 매칭되어 있어서 빠른 탐색이 가능하다.
Map은 순서를 가지지 않지만 key를 정수로 설정하면 순서를 가진 것처럼 사용할 수도 있다.
key는 중복이 불가능하고 value는 중복이 가능하다.
Map에서 key에 대한 value의 맵핑을 새로운 value로 변경하려면 update() 메서드를 이용하면 된다.
``` dart
  Map<key 타입, value 타입> 변수명 = {
    key1 : value1,
    key2 : value2,
    key3 : value3
  };
  또는
  Map<key 타입, value 타입> 변수명 = Map();
  변수명[1] = key1;
  변수명[2] = key2;
  변수명[3] = key3;
  
  ex)
  Map<int, String> testMap = {
    1 : 'value1',
    2 : 'value2',
    3 : 'value3'
  };
  
  testMap.update(1, (value) => 'NewValue1', ifAbsent: () => 'NewKey1');
  testMap.update(5, (value) => 'NewValue5', ifAbsent: () => 'NewKey5');
  
  또는
  Map<int, String> testMap = Map();
  testMap[1] = 'value1';
  testMap[2] = 'value2';
  testMap[3] = 'value3';
  
  testMap.update(1, (value) => 'NewValue1', ifAbsent: () => 'NewKey1');
  testMap.update(5, (value) => 'NewValue5', ifAbsent: () => 'NewKey5');
```
## 14 제네릭
제네릭은 타입 매개변수를 통해 다양한 타입에 대한 유연한 대처가 가능하다.
제네릭의 대표적인 예가 List, Set, Map이다.
### 1. 타입 매개변수
타입 매개변수는 인자 값을 전달하는 것이 아니라 타입을 전달한다.
``` dart
  abstract class List<E> implements EfficientLengthIterable<E> {
  // <E>의 E는 형식 타입 매개변수라고 한다.
  ...
  void add(E value);
  ...
  }
```
### 2. 매개변수화 타입을 제한하기
제네릭을 사용할 때 매개변수화 타입을 제한할 수도 있다.
타입 매개변수에 extends를 사용해서 특정 클래스를 지정하면 된다. 그러면 해당 특정 클래스와 그 클래스의 자식 클래스가 실제 타입 매개변수가 될 수 있는 것이다.
``` dart
  void main() {
    var manager1 = Manager<Person>();
    manager1.eat();
    var manager2 = Manager<Student>();
    manager2.eat();
    var manager3 = Manager();
    manager3.eat();
  }
  
  class Person {
    eat() {
      print('Person eats a food');
    }
  }
  
  class Student extends Person {
    eat() {
      print('Student eats a hambuger');
    }
  }
  
  class Mananger<T extends Person> {
    eat() {
      print('Manager eats a sandwich');
    }
  }
  
  class Dog {
    eat() {
      print('Dog eats a dog food');
    }
  }
```
### 3. 제너릭 메서드
제네릭은 클래스뿐만 아니라 메서드에도 사용할 수 있다.
메서드의 리턴 타입, 매개변수를 제네릭으로 지정할 수 있다.
``` dart
  void main() {
    var person = Person();
    print(person.getName<String>('Kim'));
  }
  
  class Person {
    T getName<T> (T param) {
      return param;
    }
  }
```
