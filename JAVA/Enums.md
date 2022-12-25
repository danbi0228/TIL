## 열거형(enums)

### 1.1 열거형이란?
: 열거형은 서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 때 사용하면 유용하다.

```
class Card {
    static final int CLOVER = 0;
    static final int HEART = 1;
    static final int DIAMOND = 2;
    static final int SPADE = 3;
    
    static final int TWO = 0;
    static final int THREE = 1;
    static final int FOUR = 2;
    
    final int kind;
    final int num;
}

// 열거형
class Card {
    enum Kind   {  CLOVER, HEART, DIAMOND, SPADE  }     // 열거형 Kind를 정의
    enum Value  {  TWO, THREE, FOUR  }
    
    final Kind kind;    // 타입이 int가 아닌 Kind 유의
    final Value value;
}
```

자바의 열거형은 '타입에 안전한 열거형(typesafe enum)'이라서 실제 값이 같아도 타입이 다르면 컴파일 에러 발생.
상수의 값이 바뀌었을 때 열거형 상수를 사용하면 기존의 소스를 다시 컴파일하지 않아도 된다.


### 1.2 열거형의 정의와 사용
괄호{}안에 상수의 이름을 나열하기만 하면 된다.
```
enum 열거형이름 {  상수명1, 상수명2, ...  }
```

```
enum Direction {  EAST, SOUTH, WEST, NORTH  }

class Unit {
    int x, y;       // 유닛의 위치
    Direction dir;  // 열거형을 인스턴스 변수로 선언
    
    void init() {
        dir = Direction.EAST;       // 유닛의 방향을 EAST로 초기화
    }
}
```

열거형 상수간의 비교에는 '**==**'를 사용할 수 있다. 하지만 '<', '>'와 같은 비교연산자는 사용할 수 없고
**compareTo()** 는 사용 가능하다. (같으면 0, 왼쪽이 크면 양수, 오른쪽이 크면 음수 반환)


#### 모든 열거형의 조상 - java.lang.Enum
```
Direction[] dArr = Direction.values();

for(Direction d : dArr)
    System.out.printf("%s=%d%n", d.name(), d.ordinal());

```

values()는 열거형의 모든 상수를 배열에 담아 반환한다.
이 메서드는 모든 열거형이 가지고 있는 것으로 컴파일러가 자동으로 추가해준다.

ordinal()은 모든 열거형의 조상인 java.lang.Enum클래스에 정의된 것으로,
열거형 상수가 정의된 순서(0부터 시작)를 정수로 반환한다.

| 메서드                                       | 설명                              |
|-------------------------------------------|---------------------------------|
| Class<E> getDeclaringClass()              | 열거형의 Class객체를 반환                |
| String name()                             | 열거형 상수의 이름을 문자열로 반환             |
| int ordinal()                             | 열거형 상수가 정의된 순서를 반환(0부터 시작)      |
| T valueOf(Class<T> enumType, String name) | 지정된 열거형에서 name과 일치하는 열거형 상수를 반환 |


### 1.3 열거형에 멤버 추가하기
: Enum클래스에 정의된 ordinal()이 열거형 상수가 정의된 순서를 반환하지만,
이 값을 열거형 상수의 값으로 사용하지 않는 것이 좋다. 이 값은 내부적인 용도로만 사용되기 위한 것이기 때문.

열거형 상수의 값이 불연속적인 경우에는 열거형 상수의 이름 옆에 원하는 값을 괄호()와 함께 적어준다.

그리고 지정된 값을 저장할 수 있는 인스턴스 변수와 생성자를 새로 추가해주어야 한다.
이 때 주의할 점, 먼저 열거형 상수를 모두 정의한 다음에 다른 멤버들을 추가해야한다.
```
enum Direction {  EAST(1), SOUTH(5), WEST(-1), NORTH(10)  }

enum Direction {
    EAST(1), SOUTH(5), WEST(-1), NORTH(10);     // 끝에 ';'를 추가해야한다.
    
    private final int value;    // 정수를 저장할 필드(인스턴스 변수)를 추가
    Direction(int value)    {  this.value = value;  }   // 생성자를 추가
    
    public int getValue()   {  return value;  }
}
```

### 1.4 열거형의 이해
열거형 상수 하나하나가 Direction객체이다.
```
enum Direction {  EAST, SOUTH, WEST, NORTH  }

// 위 문장을 클래스로 정의

class Direction {
    static final Direction EAST = new Direction("EAST");
    static final Direction SOUTH = new Direction("SOUTH");
    static final Direction WEST = new Direction("WEST");
    static final Direction NORTH = new Direction("NORTH");
    
    private String name;
    
    private Direction(String name) {
        this.name = name;
    }
}
```
Direction클래스의 static상수 EAST, SOUTH, WEST, NORTH의 값은 객체의 주소이고,
이 값은 바뀌지 않는 값이므로 **'=='로 비교 가능**.<br>
모든 열거형은 추상 클래스 Enum의 자손이다.

객체가 생성될 때마다 번호를 붙여서 인스턴스변수 ordinal에 저장.
그리고 **Comparable인터페이스를 구현해서 열거형 상수간의 비교가 가능**.<br>
구현 내용 : 두 열거형 상수의 ordinal값을 서로 빼주기만 하면 된다.

추상메서드를 새로 추가하면, 클래스 앞에도 '**abstract**'를 붙여줘야 하고,
각 static상수들도 추상 메서드를 구현해주어야 한다.