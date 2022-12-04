# 객체지향 프로그래밍1 <br>(Object-oriented Programming)

## 1. 객체지향언어
1. **코드의 재사용성이 높다.**
<br> : 새로운 코드를 작성할 때 기존의 코드를 이용하여 쉽게 작성할 수 있다.
2. **코드의 관리가 용이하다.**
<br> : 코드 간의 관계를 이용해서 적은 노력으로 쉽게 코드를 변경할 수 있다.
3. **신뢰성이 높은 프로그래밍을 가능하게 한다.**
<br> : 제어자와 메서드를 이용해서 데이터를 보호하고 올바른 값을 유지하도록 하며,
<br> 코드의 중복을 제거하여 코드의 불일치로 인한 오동작을 방지할 수 있다.

## 2. 클래스와 객체

### 2.1 클래스와 객체의 정의와 용도
> **클래스의 정의** : 클래스란 객체를 정의해 놓은 것
> <br>
> **클래스의 용도** : 클래스는 객체를 생성하는데 사용

> **객체의 정의** : 실제로 존재하는 것, 사물 또는 개념
> <br>
> **객체의 용도** : 객체가 가지고 있는 기능과 속성에 따라 다름
> <br><br>
> **유형의 객체** : 책상, 의자, 자동차, TV와 같은 사물
> <br>
> **무형의 객체** : 수학공식, 프로그램 에러와 같은 논리나 개념


| 클래스     | 객체   |
|-----------|-------|
| 제품 설계도 | 제품   |
| TV 설계도  | TV    |
| 붕어빵 기계 | 붕어빵 |


### 2.2 객체와 인스턴스
: 클래스로부터 객체를 만드는 과정을 클래스의 **인스턴스화(instantiate)**, 어떤 클래스로부터 만들어진 객체를 그 클래스의 **인스턴스(instance)** 라고 한다.
<br> **인스턴스와 객체**는 같은 의미, **객체**는 모든 인스턴스를 대표하는 포괄적인 의미, **인스턴스**는 어떤 클래스로부터 만들어진 것인지를 강조하는 구체적인 의미를 갖는다.

### 2.3 객체의 구성요소 - 속성과 기능
: 객체는 속성과 기능, 두 종류의 구성요소. 일반적으로 객체는 다수의 속성과 다수의 기능을 갖는다.
<br> 즉, 객체는 속성과 기능의 집합이라고 할 수 있다.
- **속성(property)** : 멤버변수, 특성, 필드, 상태
- **기능(function)** : 메서드, 함수, 행위

| 속성                       | 기능                                     |
|--------------------------|----------------------------------------|
| 크기, 길이, 높이,<br/> 색상, 볼륨, 채널 등 | 켜기, 끄기, 볼륨 높이기,<br/> 볼륨 낮추기, 채널 변경하기 등 |

```
// 변수
String color;   // 색깔
boolean power;  // 전원상태
int channel;    // 채널
---------------------------------
// 메서드
void power()        { power = !power; }
void channelUp()    { channel++; }
void channelDown()  { channel--; }
```

### 2.4 인스턴스의 생성과 사용
```
클래스명 변수명;           // 클래스의 객체를 참조하기 위한 참조변수를 선언
변수명 = new 클래스명();   // 클래스의 객체를 생성 후, 객체의 주소를 참조변수에 저장

Tv t;                   // Tv클래스 타입의 참조변수 t를 선언
t = new Tv();           // Tv인스턴스를 생성한 후, 생성된 Tv인스턴스의 주소를 t에 저장
```

**인스턴스는 참조변수를 통해서만 다룰 수 있으며, 참조변수의 타입은 인스턴스의 타입과 일치해야 한다.**

### 2.5 클래스의 또 다른 정의
: 프로그래밍적인 관점에서의 클래스의 정의와 의미
- **클래스 - 데이터와 함수의 결합**
> 1. **변수** : 하나의 데이터를 저장할 수 있는 공간
> 2. **배열** : 같은 종류의 여러 데이터를 하나의 집합으로 저장할 수 있는 공간
> 3. **구조체** : 서로 관련된 여러 데이터를 종류에 관계없이 하나의 집합으로 저장할 수 있는 공간
> 4. **클래스** : 데이터와 함수의 결합(구조체 + 함수)

- **클래스 - 사용자 정의 타입(user-defined type)**
<br>: 프로그래밍언어에서 제공하는 자료형 외에 프로그래머가 서로 관련된 변수들을 묶어서 하나의 타입으로 새로 추가하는 것
<br> 자바와 같은 객체지향언어에서는 클래스가 곧 사용자 정의 타입이다.


## 3. 변수와 메서드

### 3.1 선언위치에 따른 변수의 종류
1. **인스턴스변수(instance variable)**
<br> : 클래스 영역에 선언되며, 클래스의 인스턴스를 생성할 때 만들어진다. 그렇기 때문에 인스턴스 변수의 값을 읽어오거나 저장하기 위해서는 먼저 인스턴스를 생성해야 한다.
<br> 인스턴스는 독립적인 저장공간을 가지므로 **서로 다른 값을 가질 수 없다**.
2. **클래스변수(class variable)**
<br> : 클래스 변수는 인스턴스변수 앞에 **static**을 붙이기만 하면 된다. 클래스변수는 모든 인스턴스가 **공통된 저장공간(변수)를 공유**하게 된다.
<br> 클래스변수는 인스턴스를 생성하지 않고도 언제라도 바로 사용할 수 있으며, '클래스이름.클래스변수'와 같은 형식으로 사용.
<br> public을 붙이면 같은 프로그램 내에서 어디서나 접근할 수 있는 **'전역변수(global variable)'** 의 성격을 가진다.
3. **지역변수(local variable)**
<br> : 메서드 내에 선언되어 **메서드 내에서만 사용 가능**, 메서드가 종료되면 소멸되어 사용할 수 없다.


```
{   
    // 클래스 영역
    int iv;         // 지역변수
    static int cv   // 클래스변수(static변수, 공유변수)
    
    void method()
    {   
        // 메서드 영역
        int lv = 0; // 지역변수
    }
}
```

### 3.2 클래스변수와 인스턴스변수
> **인스턴스변수**는 인스턴스가 생성될 때마다 생성되므로 인스턴스마다 각기 다른 값을 유지할 수 있지만, 
> **클래스변수**는 모든 인스턴스가 하나의 저장공간을 공유하므로, 항상 공통된 값을 갖는다.


### 3.3 메서드
: **'메서드(method)'** 는 특정 작업을 수행하는 일련의 문장들을 하나로 묶은 것.

#### 메서드를 사용하는 이유
1. **높은 재사용성(reusablility)**
<br> : Java API에서 제공하는 메서드를 사용하면서 메서드는 몇 번이고 호출 가능, 다른 프로그램에서도 사용 가능.
2. **중복된 코드의 제거**
<br> : 반복되는 문장들을 묶어서 하나의 메서드로 작성해 놓으면, 반복되는 문장들 대신 메서드를 호출하는 한 문장으로 대체 가능.
<br> 코드의 중복이 제거되고, 변경사항이 발생했을 때 메서드만 수정하면 되므로 관리도 쉽고 오류 발생 가능성도 낮아진다.
3. **프로그램의 구조화**

### 3.4 메서드의 선언과 구현
: 메서드는 크게 '선언부(header, 머리)'와 '구현부(body, 몸통)'로 이루어져 있다.

#### 메서드 선언부(method declarartion, method header)
: 메서드 선언부는 '**메서드의 이름**'과 '**매개변수 선언**', '**반환타입**'으로 구성되어 있으며, 메서드가 작업을 수행하기 위해 어떤 값들을 필요로 하고 작업의 결과로 어떤 타입의 값을 반환하는지에 대한 정보를 제공
```
// int : 반환타입(출력)
// add : 메서드 이름
// int x, int y : 매개변수선언(입력)
int add (int x, int y) {
    int result = x + y;
    
    return result   // 결과를 반환
}
```

##### 메서드의 이름(method name)
: 메서드의 이름도 변수의 명명규칙대로 작성하면 된다. 메서드는 특정 작업을 수행하므로 메서드의 이름은 'add'처럼 동사인 경우가 많다.

##### 반환타입(return type)
: 메서드의 작업수행 결과(출력)인 '**반환값(return value)**'의 타입을 적는다. 반환값이 없는 경우 반환타입으로 '**void**'를 적어야 한다.
```
// 메서드 'print99danAll'은 구구단 전체를 출력하는데,
// 작업 수행 중 필요한 값도, 결과인 반환값도 없기 때문에 반환타입 'void'
void print99danAll() {
    for(int i=1; i<=9; i++) {
        for(int j=2; j<=9; j++) {
            System.out.print(j + "*" + i + " = " + (j*i) + " ");
        }
        System.out.println();
    }
}
```

#### 메서드의 구현부(method body, 메서드 몸통)
: 메서드의 선언부 다음에 오는 괄호{}를 '메서드의 구현부', 여기에 메서드를 호출했을 때 수행될 문장들을 적는다.

##### return문
: 메서드의 반환타입이 'void'가 아닌 경우, 구현부{}안에 'return 반환값;'이 반드시 포함되어 있어야 한다.
<br> 이 문장은 작업을 수행한 결과인 반환값을 호출한 메서드로 전달하는데, 이 값의 타입은 반환타입과 일치하거나 적어도 **자동 형변환이 가능**한 것이어야 한다.
<br> 매개변수와 달리 return문은 단 **하나의 값만 반환**할 수 있다.


### 3.5 메서드의 호출
```
// 메서드를 호출하는 방법
메서드이름(값1, 값2, ...);

-----------
print99danAll();        // void print99danAll()을 호출
int result = add(3, 5); // int add(int x, int y)를 호출하고 결과를 result에 저장
```

### 3.6 return문
: **return문**은 현재 실행중인 메서드를 종료하고 호출한 메서드로 되돌아간다. 
<br>반환값의 유무에 관계없이 모든 메서드에는 적어도 하나의 **return문**이 있어야 한다.

```
void printGugudan(int dan){
    for(int i=1; i<=9; i++){
        System.out.println("%d * %d = %d%n", dan, i, dan * i);
    }
    return; // 반환 타입이 void이므로 생략 가능. 컴파일러가 자동 추가
}
------------------------------------
int multiply(int x, int y){
    int result = x * y;
    
    return result;      // 반환 타입이 void가 아니므로 생략 불가
}
```

### 3.7 JVM의 메모리 구조
1. **메서드 영역(method area)**
<br> : 프로그램 실행 중 어떤 클래스가 사용되면, JVM은 해당 클래스의 클래스파일(*.class)을 읽어서 분석하여 클래스에 대한 정보(클래스 데이터)를 이곳에 저장한다. 이 때, 그 클래스의 클래스변수도 이 영역에 함께 생성된다. 
2. **힙(heap)**
<br> : 인스턴스가 생성되는 공간. 프로그램 실행 중 생성되는 인스턴스는 모두 이곳에 생성된다. 즉, 인스턴스변수들이 생성되는 공간.
3. **호출스택(call stack)**
<br> : 호출스택은 메서드의 작업에 필요한 메모리 공간을 제공한다. 메서드가 호출되면, 호출스택에 호출된 메서드를 위한 메모리가 할당되며, 이 메모리는 메서드가 작업을 수행하는 동안 지역변수(매개변수 포함)들과 연산의 중간결과 등을 저장하는데 사용된다. 그리고 메서드가 작업을 마치면 할당되었던 메모리공간은 반환되어 비워짐.

> - 메서드가 호출되면 수행에 필요한 만큼의 메모리를 스택에 할당받는다.
> - 메서드가 수행을 마치고 나면 사용했던 메모리를 반환하고 스택에서 제거된다.
> - 호출스택의 제일 위에 있는 메서드가 현재 실행중인 메서드이다.
> - 아래에 있는 메서드가 바로 위의 메서드를 호출한 메서드이다.

### 3.8 기본형 매개변수와 참조형 매개변수
- **기본형 매개변수** : 변수의 값을 읽기만 할 수 있다. (read only)
- **참조형 매개변수** : 변수의 값을 읽고 변경할 수 있다. (read & write)


### 3.9 재귀호출(recursive call)
: 메서드 내부에서 메서드 자신을 다시 호출하는 것을 '**재귀호출**' 이라 하고, 재귀호출을 하는 메서드를 '**재귀 메서드**'라 한다.
<br> 재귀호출도 조건문이 필수적으로 따라다닌다.
```
void method() {
    method();   // 재귀호출. 메서드 자신을 호출한다.
}
---------------------
void method(int n) {
    while(n != 0) {
        System.out.println(n--);
    }
}
```

### 3.10 클래스 메서드(static 메서드)와 인스턴스 메서드
- **인스턴스 메서드** : 인스턴스 변수와 관련된 작업을 하는, 즉 메서드의 작업을 수행하는데 인스턴스 변수를 필요로 하는 메서드
- **클래스 메서드(static 메서드)** : 메서드 중에서 인스턴스와 관계없는(인스턴스 변수나 인스턴스 메서드를 사용하지 않는) 메서드

1. **클래스를 설계할 때, 멤버변수 중 모든 인스턴스에 공통으로 사용하는 것에 static을 붙인다.**
<br> : 생성된 인스턴스는 서로 독립적이기 때문에 각 인스턴스의 변수(iv)는 서로 다른 값을 유지.
<br> 그러나 모든 인스턴스에서 같은 값이 유지되어야 하는 변수는 static을 붙여서 클래스변수로 정의해야 한다.
2. **클래스 변수(static 변수)는 인스턴스를 생성하지 않아도 사용할 수 있다.**
<br>  : static이 붙은 변수(클래스 변수)는 클래스가 메모리에 올라갈 때 이미 자동적으로 생성됨
3. **클래스 메서드(static 메서드)는 인스턴스 변수를 사용할 수 없다.**
<br> : 인스턴스변수는 인스턴스가 반드시 존재해야만 사용할 수 있는데,
<br> 클래스메서드(static이 붙은 메서드)는 인스턴스 생성없이 호출 가능하므로 클래스 메서드가 호출되었을 때 인스턴스가 존재하지 않을 수도 있다.
<br> 그래서 클래스 메서드에서 인스턴스변수의 사용을 금한다.
4. **메서드 내에서 인스턴스 변수를 사용하지 않는다면, static을 붙이는 것을 고려한다.**
<br> : 메서드의 작업 내용 중에서 인스턴스변수를 필요로 한다면, static을 붙일 수 없다.
<br> 반대로 인스턴스 변수를 필요로 하지 않는다면 static을 붙인다.

> - **클래스의 멤버변수 중 모든 인스턴스에 공통된 값을 유지해야하는 것이 있는지 살펴보고 있으면, static을 붙여준다.**
> - **작성한 메서드 중에서 인스턴스 변수나 인스턴스 메서드를 사용하지 않는 메서드에 static을 붙일 것을 고려한다.**

### 3.11 클래스 멤버와 인스턴스 멤버간의 참조와 호출
: 같은 클래스에 속한 멤버들 간에는 별도의 인스턴스를 생성하지 않고도 서로 참조 또는 호출이 가능하다.
<br> (단, 클래스멤버가 인스턴스 멤버를 참조, 호출하고자 하는 경우에는 인스턴스를 생성)
<br> → **인스턴스 멤버가 존재하는 시점에 클래스 멤버는 항상 존재하지만, 클래스멤버가 존재하는 시점에 인스턴스 멤버가 존재하지 않을 수도 있기 때문.**

```
class TestClass {
    void instanceMethod() {}        // 인스턴스 메서드
    static void staticMethod() {}   // static 메서드
    
    void instanceMethod2() {        // 인스턴스 메서드
        instanceMethod();           // 다른 인스턴스 메서드 호출
        staticMethod();             // static 메서드 호출
    }
    
    static void staticMethod2() {   // static 메서드
        instanceMethod();           // * 에러, 인스턴스 메서드를 호출할 수 없다.
        staticMethod();             // static 메서드는 호출 가능.
    }
}
```

> 클래스멤버(클래스 변수와 메서드)는 언제나 참조 또는 호출이 가능하기 때문에 인스턴스 멤버가 클래스 멤버를 사용하는 것은 문제 X
> <br> 그러나, 인스턴스멤버(인스턴스 변수와 메서드)는 반드시 객체를 생성한 후에만 참조 또는 호출이 가능하기 때문에 클래스멤버가 인스턴스멤버를 참조, 호출하기 위해서는 **객체를 생성해야 한다.**
> <br> 하지만, 인스턴스 멤버간의 호출에는 아무런 문제 X, 하나의 인스턴스멤버가 존재한다는 것은 **인스턴스가 이미 생성되었다는 것을 의미**하며 즉 **다른 인스턴스멤버들도 모두 존재**하기 때문이다.


<br><br><br><br><br><br><br><br><br><br><br><br><br>