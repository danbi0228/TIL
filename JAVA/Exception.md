# 예외처리

## 1. 예외처리(exception handling)

### 1.1 프로그램 오류
- **컴파일 에러** : 컴파일 시에 발생하는 에러
- **런타임 에러** : 실행 시에 발생하는 에러
- **논리적 에러** : 실행은 되지만, 의도와 다르게 동작하는 것<br><br>

- **에러(error)** : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
- **예외(exception)** : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

### 1.2 예외 클래스의 계층구조
- **Exception클래스들** : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
  - 존재하지 않는 파일의 이름 입력(FileNotFoundException)
  - 클래스의 이름을 잘못 작성(ClassNotFoundException)
  - 입력한 데이터 형식이 잘못된 경우(DataFormatException)
- **RuntimeException클래스들** : 프로그래머의 실수로 발생하는 예외
  - 배열의 범위를 벗어남(ArrayIndexOutOfBoundsException)
  - 값이 null인 참조변수의 멤버를 호출함(NullPointerException)
  - 클래스간 형변환을 잘못함(ClassCastException)
  - 정수를 0으로 나눈 경우(ArithmeticException)

### 1.3 예외처리하기 - try-catch문
- **예외처리**
  - 정의 : 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성
  - 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지
- 발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되고 처리되지 못한 예외는 JVM의 예외처리기가 받아서 예외의 원인을 화면에 출력한다

```
try {
    // 예외가 발생할 가능성이 있는 문장을 넣는다
} catch (Exception e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장 작성
} catch (Exception e2) {
    // Exception2이 발생했을 경우, 이를 처리하기 위한 문장 작성
} catch (Exception eN) {
    // ExceptionN이 발생했을 경우, 이를 처리하기 위한 문장 작성
}
```

### 1.4 try-catch문에서의 흐름
- try블럭 내에서 예외가 발생한 경우
  1. 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
  2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속 수행한다.
  <br> 만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다.
- try 블럭 내에서 예외가 발생하지 않는 경우
  1. catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다

### 1.5 예외의 발생과 catch블럭
: 예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.

#### printStackTrace()와 getMessage()
- **printStackTrace()** : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
- **getMessage()** : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

#### 멀티 catch블럭
: '|' 기호를 이용해서 여러 catch블럭을 하나의 catch블럭으로 합칠 수 있다. '|'기호로 연결할 수 있는 예외 클래스의 개수에는 제한이 없다.
```
try {
    ...
} catch (ExceptionA | ExceptionB e) {
    e.printStackTrace();
}
```

### 1.6 예외 발생시키기
1. 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든다
   - Exception e = new Exception("고의로 발생시켰음");
2. 키워드 throw를 이용해서 예외를 발생시킨다
   - throw e;

### 1.7 메서드에 예외 선언하기
: 메서드에 예외를 선언하려면, 메서드 선언부에 **throws**를 사용해서 메서드 내에서 발생할 수 있는 예외를 작성하면 된다.
<br> 예외가 여러 개일 경우에는 쉼표(,)로 구분한다.

```
void method() throws Exception1, Exception2, ... ExceptionN {
    // 메서드의 내용
}
```

### 1.8 finally블럭
: finally블럭은 예외의 발생여부에 상관없이 실행되어야 할 코드를 포함시킬 목적으로 사용된다
<br> try-catch문의 끝에 선택적으로 붙여서 사용할 수 있으며, try-catch-finally의 순서로 구성된다
<br> 예외가 발생하지 않는 경우에는 'try → finally'의 순으로 실행된다

```
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception e1) {
    // 예외처리를 위한 문장을 적는다.
} finally {
    // 예외의 발생여부에 관계없이 항상 수행되어야 하는 문장들을 넣는다.
    // finally블럭은 try-catch문의 맨 마지막에 위치해야한다.
}
```