## 애너테이션(annotation)

### 1.1 애너테이션이란?
: 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것.
애너테이션은 주석처럼 프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에 유용한 정보를 제공할 수 있다는 장점이 있다.

<br>

### 1.2 표준 애너테이션
(*) 붙은 것은 메타 애너테이션

| 애너테이션                | 설명                                 |
|----------------------|------------------------------------|
| @Override            | 컴파일러에게 오버라이딩하는 메서드라는 것을 알림         |
| @Deprecated          | 앞으로 사용하지 않을 것을 권장하는 대상에 붙임         |
| @SuppressWarnings    | 컴파일러의 특정 경고메시지가 나타나지 않게 해줌         |
| @SafeVarargs         | 지네릭스 타입의 가변인자에 사용 (JDK1.7)         |
| @FunctionalInterface | 함수형 인터페이스라는 것을 알림 (JDK1.8)         |
| @Native              | native메서드에 참조되는 상수 앞에 붙임 (JDK1.8)  |
| @Target*             | 애너테이션이 적용가능한 대상을 지정하는데 사용          |
| @Documented*         | 애너테이션 정보가 javadoc으로 작성된 문서에 포함되게 함 |
| @Inherited*          | 애너테이션이 자손 클래스에 상속되도록 함             |
| @Retention*          | 애너테이션이 유지되는 범위를 지정하는데 사용           |
| @Repeatable*         | 애너테이션을 반복해서 적용할 수 있게 함 (JDK1.8)    |



#### @Override
: 메서드 앞에만 붙일 수 있는 애너테이션으로, 조상의 메서드를 오버라이딩 하는 것이라는걸 컴파일러에게 알려주는 역할
오버라이딩 할 때 조상메서드의 이름을 잘못 써도 컴파일러는 이것이 잘못된 것인지 인지하지 못하는데,
메서드 앞에 '**@Override**'라고 애너테이션을 붙이면, 컴파일러가 같은 이름의 메서드가 조상에 있는지 확인하고
없으면 에러메시지를 출력한다.

#### @Deprecated
: 더 이상 사용되지 않는 필드나 메서드에 붙이는 애너테이션.
이 애너테이션이 붙은 대상은 다른 것으로 대체되었으니 더 이상 사용하지 않을 것을 권한다는 의미이다.

#### @FunctionalInterface
: '함수형 인터페이스'를 선언할 때, 이 애너테이션을 붙이면 컴파일러가 '함수형 인터페이스'를 올바르게 선언했는지
확인하고, 잘못된 경우 에러를 발생시킨다.

#### @SuppressWarnings
: 컴파일러가 보여주는 경고메시지가 나타나지 않게 억제해준다.

'**@SuppressWarnings**'로 억제할 수 있는 경고 메시지 : "deprecation", "unchecked", "rawtypes", "varargs" 등

#### @SafeVarargs
: 메서드에 선언된 가변인자의 타입이 non-reifiable타입일 경우, 해당 메서드를 선언하는 부분과
호출하느나 부분에서 "unchecked" 경고가 발생하는데, 해당 코드에 문제가 없다면 이 경고를 억제하기 위해
'@SafeVarargs'를 사용해야 한다.

이 애너테이션은 static이나 final이 붙은 메서드와 생성자에만 붙일 수 있다.
오버라이드 될 수 있는 메서드에는 사용 불가.

<br>

### 1.3 메타 애너테이션
: '애너테이션을 위한 애너테이션', 즉 애너테이션에 붙이는 애너테이션으로 애너테이션을 정의할 때 
에터테이션의 적용대상(target)이나 유지기간(retention)등을 지정하는데 사용됨

#### @Target
: 애너테이션이 적용가능한 대상을 지정하는데 사용된다.

| 대상 타입           | 의미                           |
|-----------------|------------------------------|
| ANNOTATION_TYPE | 애너테이션                        |
| CONSTRUCTOR     | 생성자                          |
| FIELD           | 필드(멤버변수, enum상수) - 기본형       |
| LOCAL_VARIABLE  | 지역변수                         |
| METHOD          | 메서드                          |
| PACKAGE         | 패키지                          |
| PARAMETER       | 매개변수                         |
| TYPE            | 타입(클래스, 인터페이스, enum)         |
| TYPE_PARAMETER  | 타입 매개변수(JDK 1.8)             |
| TYPE_USE        | 타입이 사용되는 모든 곳(JDK 1.8) - 참조형 |


#### @Retention
: 애너테이션이 유지(retention)되는 기간을 지정하는데 사용된다.

- 애너테이션 유지정책의 종류

| 유지 정책  | 의미                          |
|--------|-----------------------------|
| SOURCE | 소스 파일에만 존재. 클래스파일에는 존재하지 않음 |
| CLASS  | 클래스 파일에 존재. 실행시에 사용 불가. 기본값 |
| RUNTIME | 클래스 파일에 존재. 실행시에 사용 가능      |


#### @Documented
: 애너테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 한다.
자바에서 제공하는 기본 애너테이션 중에 '@Override'와 '@SuppressWarnings'를 제외하고 
모두 이 메타 애너테이션이 붙어있다.


#### @Inherited
: 애너테이션이 자손 클래스에 상속되도록 한다.
'@Inherited'가 붙은 애너테이션을 조상 클래스에 붙이면, 자손 클래스도 이 애너테이션이 붙은 것과 같이 인식된다.

#### @Repeatable
: 보통은 하나의 대상에 한 종류의 애너테이션을 붙이는데, '@Repeatable'이 붙은 애너테이션은 여러 번 붙일 수 있다.

일반적인 애너테이션과 달리 같은 이름의 애너테이션이 여러 개가 하나의 대상에 적용될 수 있기 때문에,
이 애너테이션들을 하나로 묶어서 다룰 수 있는 애너테이션도 추가로 정의해야 한다.


#### @Native
: 네이티브 메서드(Native method)에 의해 참조되는 '상수 필드(constant field)'에 붙이는 애너테이션.

<br>

### 1.4 애너테이션 타입 정의하기
```
@interface 애너테이션이름 {
    타입요소이름();       // 애너테이션의 요소를 선언한다.
    ...
}
```

'**@Override**'는 애너테이션이고 '**Override**'는 애너테이션의 타입이다.

#### 애너테이션의 요소
: 애너테이션 내에 선언된 메서드를 '애너테이션의 요소(element)'라고 한다.

```
@interface TestInfo {
    int count();
    String testedBy();
    String[] testTools();
    TestType testType();    // enum TestType { FIRST, FINAL }
    DateTime testDate();    // 자신이 아닌 다른 애너테이션(@DateTime)을 포함할 수 있다.
}

@interface DateTime {
    String yymmdd();
    String hhmmss();
}
```

애너테이션의 요소는 반환값이 있고 매개변수는 없는 추상 메서드의 형태를 가지며,
상속을 통해 구현하지 않아도 된다.
다만, 애너테이션을 적용할 때 요소들의 값을 빠짐없이 지정해주어야 한다. 이름도 같이 적어주므로 순서는 상관없다.

애너테이션의 각 요소는 기본값을 가질 수 있으며,
기본값이 있는 요소는 애너테이션을 적용할 때 값을 지정하지 않으면
기본값이 사용된다.

```
@TestInfo {
    count = 3, testedBy = "Kim",
    testTools = {"Junit", "AutoTester"},
    testType = TestType.FIRST,
    testDate = @DateTime(yymmdd = "160101", hhmmss = "235959")
}
public class NewClass { ... }

// ------------------------------------------------------------------

@interface TestInfo {
    int count() default 1;      // 기본값을 1로 지정
}

@TestInfo   // @TestInfo(count=1)과 동일
public class NewcClass{ ... }
```

요소의 타입이 배열인 경우, 괄호{}를 사용해서 여러 개의 값을 지정할 수 있다.
```
@interface TestInfo {
    String[] testTools();
}

@Test(testTools = {"JUnit", "AutoTester"})  // 값이 여러 개인 경우
@Test(testTools = "JUnit")                  // 값이 하나일 때는 괄호{} 생략 가능
@Test(testTools = {})                       // 값이 없을 때는 괄호{}가 반드시 필요
```

#### java.lang.annotation.Annotation
: 모든 애너테이션의 조상은 Annotation이다. 그러나 애너테이션은 상속이 허용되지 않으므로
Annotation을 조상으로 지정할 수 없다.

```
@interface TestInfo extends Annotation {    // 에러. 허용되지 않는 표현
    int count();
    String testedBy();
        ...
}
```

모든 애너테이션 객체에 대해 equals(), hashCode(), toString()과 같은 메서드를 호출하는 것이 가능.

#### 마커 애너테이션 Marker Annotation
: 값을 지정할 필요가 없는 경우, 애너테이션의 요소를 하나도 정의하지 않을 수 있다.
Serializable이나 Cloneable인터페이스처럼, 요소가 하나도 정의되지 않은 애너테이션을 마커 애너테이션이라고 한다.

#### 애너테이션 요소의 규칙
- 요소의 타입은 기본형, String, enum, 애너테이션, Class만 허용된다.
- ()안에 매개변수를 선언할 수 없다.
- 예외를 선언할 수 없다.
- 요소를 타입 매개변수로 정의할 수 없다.

```
@interface AnnoTest {
    int id = 100;                       // 가능. 상수 선언. static final int id = 100;
    String major(int i, int j);         // 에러. 매개변수를 선언할 수 없음
    String minor() throws Exception;    // 에러. 예외를 선언할 수 없음
    ArrayList<T> list();                // 에러. 요소의 타입에 타입 매개변수 사용 불가
}
```