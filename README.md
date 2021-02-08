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
**상수**는 값을 바꿀 수 없는 변수이다.
**final**와 **const**의 차이는 다음과 같다.
**final**은 **런타임**에 상수가 되고 **const**는 **컴파일 시점**에 상수가 된다.

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

**final**은 런타임에 상수화되기 때문에 실행하면 get() 함수에서 가져온 값으로 할당이 가능하다.
하지만 **const**는 이미 컴파일 시점에 상수화되었기 때문에 런타임 시 get() 함수에서 가져온 값을 할당하려고 하면 에러가 발생한다.
상수에 값을 할당하려는 것과 동일한 시도이기 때문이다.
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
