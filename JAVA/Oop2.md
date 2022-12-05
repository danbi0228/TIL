# 객체지향 프로그래밍2 <br>(Object-oriented Programming)

## 4. 오버로딩(overloading)
: 한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것을 **메서드 오버로딩, 오버로딩**이라고 한다.

### 4.1 오버로딩의 조건
1. **메서드 이름이 같아야 한다.**
2. **매개변수의 개수 또는 타입이 같아야 한다.**

### 4.2 오버로딩의 예
: **println메서드**
```
void println()
void println(boolean x)
void println(char x)
void println(char[] x)
void println(double x)
void println(float x)
void println(int x)
void println(long x)
void println(Object x)
void println(String x)
```
**println메서드**를 호출할 때 매개변수로 넘겨주는 값의 타입에 따라서 위의 오버로딩된 메서드들 중의 하나가 선택되어 실행되는 것

```
int add(int a, int b) { return a+b; }
int add(int x, int y) { return x+y; }
// 매개변수의 이름만 다르고 매개변수의 타입이 같기 때문에 오버로딩 성립 X
--------------------
int add(int a, int b) { return a+b; }
long add(int a, int b) { return (long)(a+b); }
// 리턴타입만 다르고 매개변수의 타입과 개수가 일치하기 때문에 오버로딩 성립 X
--------------------
long add(int a, int b) { return a+b; }
long add(long a, long b) { return a+b; }
// 오버로딩 O
--------------------
int add(int a, int b) { return a+b; }
long add(long a, long b) { return a+b; }
long add(int[] a) {
    long result = 0;
    
    for(int i=0; i<a.length; i++) {
        result += a[i];
    }
    return result;
}
// 오버로딩 O
```

### 4.4 오버로딩의 장점
: 오버로딩을 통해 여러 메서드들이 println이라는 하나의 이름으로 정의될 수 있다면, println이라는 이름만 기억하며 되므로 **기억하기도 쉽고 이름도 짧게 할 수 있어서 오류의 가능성을 줄일 수 있다**.
<br> 또한, **메서드의 이름을 절약할 수 있다**. 

### 4.5 가변인자(varargs)와 오버로딩
: 기존에 메서드의 매개변수 개수는 고정적이었으나, JDK 1.5부터 동적으로 지정 가능, 이 기능을 '**가변인자**'라고 한다.
<br> 가변인자는 '**타입... 변수명**'과 같은 형식으로 선언, PrintStream클래스의 printf()가 대표적인 예
```
public PrintStream printf(String format, Object... args) { ... }
--------------
String concatenate (String... str) { ... }
--------------
String concatenate (String[] str) { ... }

String result = concatenate (new String[0]);    // 인자로 배열을 지정
String result = concatenate (null);             // 인자로 null을 지정
String result = concatenate ();                 // 에러. 인자가 필요함

// 매개변수의 타입을 배열로 하면, 반드시 인자를 지정해줘야 하기 때문에 인자를 생략할 수 없다.
// 그래서 null이나 길이가 0인 배열을 인자로 지정해줘야 하는 불편함이 있다.
```

<br>

## 5. 생성자(Constructor)

### 5.1 생성자란
: **생성자**는 인스턴스가 생성될 때 호출되는 '**인스턴스 초기화 메서드**(인스턴스변수들을 초기화)'이다. 따라서 인스턴스 변수의 초기화 작업에 주로 사용.
<br> 생성자 역시 메서드처럼 클래스 내에 선언, 구조도 메서드와 유사하지만 리턴값이 없다는 점이 다르다.

1. **생성자의 이름은 클래스의 이름과 같아야 한다.**
2. **생성자는 리턴값이 없다.**

```
클래스이름 (타입 변수명, 타입 변수명, ...) {
    // 인스턴스 생성 시 수행될 코드,
    // 주로 인스턴스 변수의 초기화 코드를 적는다.
}

class Card() {
    Card() {        // 매개변수가 없는 생성자.
        ...
    }
    
    Card(String k, int num) {   // 매개변수가 있는 생성자
        ...
    }
}
```

> Card c = new Card();
> 1. **연산자 new에 의해서 메모리(heap)에 Card클래스의 인스턴스가 생성된다.**
> 2. **생성자 Card()가 호출되어 수행된다.**
> 3. **연산자 new의 결과로, 생성된 Card인스턴스의 주소가 반환되어 참조변수 c에 저장된다.**

### 5.2 기본 생성자
: 모든 클래스에는 반드시 하나 이상의 생성자가 정의되어야 한다.
<br> 그러나 클래스에 생성자를 정의하지 않고도 인스턴스를 생성할 수 있었던 이유는 컴파일러가 기본생성자를 제공하기 때문이다.

### 5.3 매개변수가 있는 생성자
```
class Car {
    String color;       // 색상
    String gearType;    // 변속기 종류 - auto(자동), manual(수동)
    int door;           // 문의 개수
    
    Car() {}    // 생성자
    Car(String c, String g, ind d) {    // 생성자
        color = c;
        gearType = g;
        door = d;
    }
}

class CarTest {
    public static void main(String[] args) {
        Car c1 = new Car();
        c1.door = "white";
        c1.gearType = "auto";
        c1.door = 4;
        
        Car c2 = new Car("white", "auto", 4);
    }

}
```

### 5.4 생성자에서 다른 생성자 호출하기 - this(), this
: 생성자 간에 서로 호출이 가능하다. 단, 다음 두 조건으르 만족시켜야 한다.
1. **생성자의 이름으로 클래스 이름 대신 this를 사용한다.**
2. **한 생성자에서 다른 생성자를 호출할 때는 반드시 첫 줄에서만 호출이 가능하다.**
```
class Car {
    String color;       // 색상
    String gearType;    // 변속기 종류 - auto(자동), manual(수동)
    int door;           // 문의 개수
    
    Car() {
        this("white", "auto", 4);
    }
    
    Car(String color) {
        this(color, "auto", 4);
    }
    
    Car(String color, String gearType, int door) {
        this.color = color;
        this.gearType = gearType;
        this.door = door;
    }
}
```

'this'는 참조변수로 인스턴스 자신을 가리킨다. 참조변수를 통해 인스턴스의 멤버에 접근할 수 있는 것처럼, 'this'로 인스턴스변수에 접근할 수 있다.
<br>하지만, 'this'를 사용할 수 있는 것은 인스턴스 멤버뿐이다. static메서드(클래스 메서드)에서는 인스턴스 멤버들을 사용할 수 없는 것처럼, 'this' 역시 사용할 수 없다.
<br>왜냐하면 static메서드는 인스턴스를 생성하지 않고도 호출될 수 있으므로 static 메서드가 호출된 시점에 인스턴스가 존재하지 않을 수도 있기 때문.

> **this** : 인스턴스 자신을 가리키는 참조변수, 인스턴스의 주소가 저장되어 있다. 모든 인스턴스 메서드에 지역변수로 숨겨진 채로 존재한다.
> <br>
> **this(), this(매개변수)** : 생성자, 같은 클래스의 다른 생성자를 호출할 때 사용한다.

<br>

## 6. 변수의 초기화

### 6.1 변수의 초기화
: 변수를 선언하고 처음으로 값을 저장하는 것을 '**변수의 초기화**'라고 한다.
<br> 지역변수는 사용하기 전에 반드시 초기화해야 한다.

```
class InitTest {
    int x;              // 인스턴스변수
    int y = x;          // 인스턴스변수
    
    void method1() {
        int i;          // 지역변수
        int j = 1;      // 에러. 지역변수를 초기화하지 않고 사용
    }
}
```

> 멤버변수의 초기화 방법
> 1. 명시적 초기화
> 2. 생성자
> 3. 초기화 블럭 
>    - 인스턴스 초기화 블럭 : 인스턴스 변수를 초기화 하는데 사용.
>    - 클래스 초기화 블럭 : 클래스 변수를 초기화 하는데 사용.


### 6.2 명시적 초기화
: 변수를 선언과 동시에 초기화하는 것을 명시적 초기화라고 한다.

### 6.3 초기화 블럭
- **클래스 초기화 블럭** : 클래스변수의 복잡한 초기화에 사용. 클래스가 메모리에 처음 로딩될 때 한번만 수행.
- **인스턴스 초기화 블럭** : 인스턴스변수의 복잡한 초기화에 사용. 인스턴스를 생성할 때마다 수행.

### 6.4 멤버변수의 초기화 시기와 순서
- 클래스 변수의 초기화 시점 : 클래스가 처음 로딩될 때 단 한 번 초기화 된다.
- 인스턴스 변수의 초기화 시점 : 인스턴스가 생성될 때마다 각 인스턴스별로 초기화가 이루어진다.
- 클래스 변수의 초기화 순서 : 기본값 → 명시적초기화 → 클래스 초기화 블럭
- 인스턴스 변수의 초기화 순서 : 기본값 → 명시적초기화 → 인스턴스 초기화 블럭 → 생성자

```
class InitTest {
    // 명시적 초기화
    static int cv = 1;
    int iv = 1;
    
    static {  cv = 2;  }    // 클래스 초기화 블럭
    
    {    iv = 2;   }        // 인스턴스 초기화 블럭
    
    InitTest() {
        iv = 3;
    }
}

```