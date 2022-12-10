# java.lang패키지 1-1

## 1. java.lang패키지
: java.lang패키지는 자바프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다. 그렇기 때문에 import문 없이도 사용 가능하다

### 1.1 Object클래스

| Object클래스의 메서드                                                                                      | 설명                                                                                                                        |
|-----------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| **protected** Object clone()                                                                            | 객체 자신의 복사본을 반환한다                                                                                                          |
| public boolean equals(Object obj)                                                                   | 객체 자신과 객체 obj가 같은 객체인지 알려준다(같으면 true)                                                                                     |
| **protected** void finalize()                                                                           | 객체가 소멸될 때 가비지 커렉터에 의해 자동적으로 호출된다<br/>이 때 수행되어야하는 코드가 있을 때 오버라이딩한다.(거의 사용X)                                                |
| public Class getClass()                                                                             | 객체 자신의 클래스 정보를 담고 있는 Class인스턴스를 반환한다                                                                                      |
| public int hashCode()                                                                               | 객체 자신의 해시코드를 반환한다.                                                                                                        |
| public String toString()                                                                            | 객체 자신의 정보를 문자열로 반환한다.                                                                                                     |
| public void notify()                                                                                | 객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨운다.                                                                                           |
| public void notifyAll()                                                                             | 객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.                                                                                            |
| public void wait()<br/>public void wati(long timeout)<br/>public void wati(long timeout, int nanos) | 다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 지정된 시간(timeout,nanos)동안 기다리게 한다.(timeout은 천 분의 1초, nanos는 10⁹분의 1초) |


#### equals(Object obj)
: 매개변수로 객체의 참조변수를 받아서 비교하여 그 결과를 boolean값으로 알려준다.
```
public boolean equals(Object obj) {
    return (this==obj);
}
```

#### hashCode()
: 해싱(hashing)기법에 사용되는 '해시함수'를 구현한 것이다. 해싱은 데이터관리기법 중 하나인데 다량의 데이터를 저장하고 검색하는데 유용하다
<br> 해시함수는 찾고자하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시코드(hashcode)를 반환한다.

#### toString()
: 인스턴스에 대한 정보를 문자열(String)로 제공할 목적으로 정의한 메서드.
<br> 인스턴스의 정보를 제공한다는 것은 대부분의 경우 인스턴스 변수에 저장된 값들과 문자열로 표현한다는 뜻이다.
```
public String toString() {
    return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

#### clone()
: 자신을 복제하여 새로운 인스턴스를 생성하는 메서드.
<br> Object클래스에 정의된 clone()은 단순히 인스턴스변수의 값만 복사하기 때문에 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않는다.

#### 공변 반환타입
: 오버라이딩 할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용하는 것.
<br> 공변 반환타입을 사용하면 조상의 타입이 아닌 실제로 반환되는 자손 개게의 타입으로 반환할 수 있어 번거로운 형변환이 줄어든다는 장점이 있다.
```
public Point clone() {      // 1. 반환타입을 Object에서 Point로 변경
    Object obj = null;
    try {
        obj = super.clone();
    } catch(CloneNotSupportedException e) {}
    return (Point)obj;      // 2. Point타입으로 형변환한다.
}

----------
Point copy = (Point)original.clone();
-
Point copy = original.clone();
```

#### 얕은 복사와 깊은 복사
: clone()은 단순히 객체에 저장된 값을 그대로 복제할 뿐, 객체가 참조하고 있는 객체까지 복제하지 않는다.
<br> 객체배열을 clone()으로 복제하는 경우 원본과 복제본이 같은 객체를 공유하므로 완전한 복제라고 보기 어렵다. 이런 복제를 얕은 복사(shallow copy)라고 한다. 얕은 복사에서 원본을 변경하면 복사본도 영향을 미친다.
<br> 반면, 원본이 참조하고 있는 객체까지 복제하는 것을 깊은 복사(deep copy)라고 하며, 깊은 복사에서는 원본과 복사본이 서로 다른 객체를 참조하므로 원본의 변경이 복사본에 영향을 미치지 않는다.

#### getClass()
: 자신이 속한 클래스의 Class객체를 반환하는 메서드인데, Class객체는 이름이 'Class'인 클래스의 객체이다.
```
public final class Class implements ... {   // Class클래스
    ...
}
```

<br> Class 객체는 클래스의 모든 정보를 담고 있으며, 클래스당 1개만 존재한다. 그리고 클래스 파일이 '클래스 로더'에 의해서 메모리에 올라갈 떄, 자동으로 생성된다.
<br> 클래스 로더는 실행 시에 필요한 클래스를 동적으로 메모리에 로드하는 역할을 한다. 기존에 생성된 클래스 객체가 메모리에 존재하는지 확인하고, 있으면 객체의 참조를 반환하고 없으면 클래스 패스에 지정된 경로를 따라서 클래스 파일을 찾는다.
<br> 못 찾으면 ClassNotFoundException이 발생하고, 찾으면 해당 클래스 파일을 읽어서 Class객체로 변환한다.

#### Class객체를 얻는 방법
: 클래스의 정보가 필요할 때, 먼저 Class객체에 대한 참조를 얻어와야 한다.
```
Class cObj = new Card().getClass();     // 생성된 객체로부터 얻는 방법
Class cObj = Card.Class;                // 클래스 리터럴(*.class)로부터 얻는 방법
Class cObj = Class.forName("Card");     // 클래스 이름으로부터 얻는 방법

Card c = new Card();                    // new연산자를 이용해서 객체 생성
Card c = Card.Class.newInstance();      // Class객체를 이용해서 객체 생성
```

### 1.2 String클래스
: String클래스는 문자열을 저장하고 이룰 다루는데 필요한 메서드를 함께 제공한다.

#### 변경 불가능한(immutable)클래스
: String클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[]) value를 인스턴스 변수로 정의해놓고 있다.
<br> 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스변수(value)에 문자형 배열(char[])로 저장되는 것이다.

```
public final class String implements java.io.Serializable, Comparable {
    private char[] value;
        ...
}
```

한 번 생성된 String인스턴스가 갖고 있는 문자열은 읽어올 수만 있고, 변경할 수는 없다.
<br> 문자열간의 결합이나 추출 등 문자열을 다루는 작업이 많이 필요한 경우에는 Stirng클래스 대신 StringBuffer클래스를 사용하는 것이 좋다.
<br> StringBuffer인스턴스에 저장된 문자열은 변경이 가능하므로 하나의 StringBuffer인스턴스만으로도 문자열을 다루는 것이 가능하다.


#### 문자열의 비교
: 문자열을 만들 때는 문자열 리터럴을 지정하는 방법과 String클래스의 생성자를 사용해서 만드는 방법이 있다.

```
String str1 = "abc";        // 문자열 리터럴 "abc"의 주소가 str1에 저장
String str2 = "abc";        // 문자열 리터럴 "abc"의 주소가 str2에 저장
String str3 = new String("abc");    // 새로운 String인스턴스를 생성
String str4 = new String("abc");    // 새로운 String인스턴스를 생성
```

문자열 리터럴은 이미 존재하는 것을 재사용하고, String클래스의 생성자를 이용한 경우에는 새로운 String인스턴스가 생성된다.
<br> equals()를 사용했을 때 두 문자열의 내용("abc")을 비교하기 때문에 두 경우 모두 결과가 true로 나오지만, 각 String인스턴스의 주소를 등가비교연산자 '=='로 비교했을 때는 결과가 다르다.


#### 문자열 리터럴
: 자바 소스파일에 포함된 모든 문자열 리터럴은 컴파일 시에 클래스 파일에 저장된다. 이 때 같은 내용의 문자열 리터럴은 한번만 저장된다.
<br> 문자열 리터럴도 String인스턴스이고, 한번 생성하면 내용을 변경할 수 없으니 하나의 인스턴스를 공유하면 되기 때문.

#### 빈 문자열(empty string)
: 'String s = "";'과 같은 문장이 있을 때, 참조변수 s가 참조하고 있는 String인스턴스는 내부에 'new char[0]'과 같이 길이가 0인 char형 배열을 저장하고 있는 것.

```
char[] chArr = new char[0];     // 길이가 0인 char배열
int[] iArr = {};                // 길이가 0인 int배열
```

char형 변수에는 반드시 하나의 문자를 지정해야 한다. 'char c = '';'(X)

#### String클래스의 생성자와 메서드

| 메서드 / 설명                                                                                                                                        | 예제                                                                                                                                                                                           | 결과                                                                            |
|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **String(String s)**<br/>주어진 문자열(s)을 갖는 String인스턴스를 생성                                                                                              | String s = new String("Hello");                                                                                                                                                              | s = "Hello"                                                                   |
| **String(char[] value)**<br/>주어진 문자열(value)을 갖는 String인스턴스를 생성                                                                                      | char[] c = {'H', 'e', 'l', 'l', 'o'};<br/>String s = new String(c);                                                                                                                          | s = "Hello"                                                                   |
| **String(StringBuffer buf)**<br/>StringBuffer인스턴스가 갖고 있는 문자열과 같은 내용의 String인스턴스를 생성                                                                 | StringBuffer sb = new StringBuffer("Hello");<br/>String s = new String(sb);                                                                                                                  | s = "Hello"                                                                   |
| **char charAt(int index)**<br/>지정된 위치(index)에 있는 문자를 알려준다(0부터 시작)                                                                                   | String s = new String("Hello");<br/>String n = "0123456";<br/>char c = s.charAt(1);<br/>char c2 = n.charAt(1);                                                                               | c = 'e'<br/>c2 = '1'                                                          |
| **int compareTo(String str)**<br/>문자열(str)과 사전순서로 비교한다. 같으면 0, 사전순으로 이전이면 음수, 이후면 양수 반환                                                             | int i = "aaa".compareTo("aaa");<br/>int i2 = "aaa".compareTo("bbb");<br/>int i3 = "bbb".compareTo("aaa");                                                                                    | i = 0<br/>i2 = -1<br/>i3 = 1                                                  |
| **String concat(String str)**<br/>문자열(str)을 뒤에 덧붙인다                                                                                                 | String s = "Hello";<br/>String s2 = s.concat(" World");                                                                                                                                      | s2 = "Hello World"                                                            |
| **boolean contains(CharSequence s)**<br/>지정된 문자열(s)이 포함되었는지 검사                                                                                      | String s = "abcdefg";<br/>boolean b = s.contains("bc");                                                                                                                                      | b = true                                                                      |
| **boolean endsWith(String suffix)**<br/>지정된 문자열(suffix)로 끝나는지 검사                                                                                    | String file = "Hello.txt";<br/>boolean b = file.endsWith("txt");                                                                                                                             | b = true                                                                      |
| **boolean equals(Object obj)**<br/>매개변수로 받은 문자열(obj)과 String인스턴스의 문자열을 비교. obj가 String이 아니거나 문자열이 다르면 false를 반환                                     | String s = "Hello";<br/>boolean b = s.equals("Hello");<br/>boolean b2 = s.equals("hello");                                                                                                   | b = true<br/>b2 = false                                                       |
| **boolean equalsIgnoreCase(String str)**<br/>문자열과 String인스턴스의 문자열을 대소문자 구분없이 비교                                                                     | String s = "Hello";<br/>boolean b = s.equalsIgnoreCase("HELLO");<br/>boolean b2 = s.equalsIgnoreCase("hello");                                                                               | b = true<br/>b2 = true                                                        |
| **int indexOf(int ch)**<br/>주어진 문자(ch)가 문자열에 존재하는지 확인하여 위치(index)를 알려준다. 못 찾으면 -1을 반환                                                               | String s = "Hello";<br/>int idx1 = s.indexOf('o');<br/>int idx2 = s.indexOf('k');                                                                                                            | idx1 = 4<br/>idx2 = -1                                                        |
| **int indexOf(int ch, int pos)**<br/>주어진 문자(ch)가 문자열에 존재하는지 지정된 위치(pos)를 확인하여 위치(index)를 알려준다. 못 찾으면 -1을 반환                                         | String s = "Hello";<br/>int idx1 = s.indexOf('e', 0);<br/>int idx2 = s.indexOf('e', 2);                                                                                                      | idx1 = 1<br/>idx2 = -1                                                        |
| **int indexOf(String str)**<br/>주어진 문자열이 존재하는지 확인하여 그 위치(index)를 알려준다.<br/>없으면 -1을 반환                                                               | String s = "ABCDEFG";<br/>int idx = s.indexOf("CD");                                                                                                                                         | idx = 2                                                                       |
| **String intern()**<br/>문자열을 상수풀(constant pool)에 등록한다. 이미 상수풀에 같은 내용의 문자열이 있을 경우 그 문자열의 주소값을 반환함                                                    | String s = new String("abc");<br/>String s2 = new String("abc");<br/>boolean b = (s==s2);<br/>boolean b2 = s.equals(s2);<br/>boolean b3 = (s.intern() == s2.intern());                       | b = false<br/>b2 = true<br/>b3 = true                                         |
| **int lastIndexOf(int ch)**<br/>지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서부터 찾아서 위치(index)를 알려준다. 못 찾으면 -1 반환                                                     | String s = "java.lang.Object";<br/>int idx1 = s.lastIndexOf('.');<br/>int idx2 = s.IndexOf('.');                                                                                             | idx1 = 9<br/>idx2 = 4                                                         |
| **int lastIndexOf(String str)**<br/>지정된 문자열을 인스턴스의 문자열 끝에서부터 찾아서 위치(index)를 알려준다. 못 찾으면 -1 반환                                                       | String s = "java.lang.java";<br/>int idx1 = s.lastIndexOf("java");<br/>int idx2 = s.IndexOf("java");                                                                                         | idx1 = 10<br/>idx2 = 0                                                        |
| **int length()**<br/>문자열의 길이를 알려준다                                                                                                                  | String s = "Hello";<br/>int length = s.length();                                                                                                                                             | length = 5                                                                    |
| **String replace(char old, char nw)**<br/>문자열 중의 문자(old)를 새로운 문자(nw)로 바꾼 문자열을 반환                                                                    | String s = "Hello";<br/>String s1 = s.replace('H', 'C');                                                                                                                                     | s1 = "Cello"                                                                  |
| **String replace**(CharSequence old, CharSequence nw)<br/>문자열 중의 문자열(old)을 새로운 문자열(nw)로 모두 바꾼 문자열을 반환                                               | String s = "Hellollo";<br/>String s1 = s.replace("l"l, "LL");                                                                                                                                | s1 = "HeLLoLLo"                                                               |
| **String replaceAll(String regex, String replacement)**<br/>문자열 중에서 지정된 문자열(regex)과 일치하는 것을 새로운 문자열(replacement)로 모두 변경                             | String ab = "AABBAABB";<br/>String r = ab.replaceAll("BB", "bb");                                                                                                                            | r = "AAbbAAbb"                                                                |
| **String replaceFirst(String regex, String replacement)**<br/>문자열 중에서 지정된 문자열(regex)과 일치하는 것 중, 첫번째 것만 새로운 문자열로 변경                                  | String ab = "AABBAABB";<br/>String r = ab.replaceFirst("BB", "bb");                                                                                                                          | r = "AAbbAABB"                                                                |
| **String[] split(String regex)**<br/>문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환                                                                         | String animals = "dog,cat,bear";<br/>String[] arr = animals.split(",");                                                                                                                      | arr[0] = "dog"<br/>arr[1] = "cat"<br/>arr[2] = "bear"                         |
| **String[] split(String regex, int limit)**<br/>문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환. 단, 문자열 전체를 지정된 수(limit)로 자른다                                | String animals = "dog,cat,bear";<br/>String[] arr = animals.split(",", 2);                                                                                                                   | arr[0] = "dog"<br/>arr[1] = "cat,bear"                                        |
| **boolean startsWith(String prefix)**<br/>주어진 문자열(prefix)로 시작하는지 검사                                                                                 | String s = "java.lang.Object";<br/>boolean b = s.startWith("java");<br/>boolean b2 = s.startWith("lang");                                                                                    | b = true<br/>b2 = false                                                       |
| **String substring(int begin)** / **String substring(int begin, int end)**<br/>주어진 시작위치(begin)부터 끝 위치(end) 범위에 포함된 문자열을 얻는다. 시작위치의 문자는 범위에 포함되지만, 끝 위치의 문자는 포함X | String s = "java.lang.Object";<br/>String c = s.substring(10);<br/>String p = s.substring(5, 9);                                                                                             | c = "Object"<br/>p = "lang"                                                   |
| **String toLowerCase()**<br/>String인스턴스에 저장되어 있는 모든 문자열을 소문자로 변환하여 반환                                                                               | String s = "Hello";<br/>String s1 = s.toLowerCase();                                                                                                                                         | s1 = "hello";                                                                 |
| **String toUpperCase()**<br/>String인스턴스에 저장되어 있는 모든 문자열을 대문자로 변환하여 반환                                                                               | String s = "Hello";<br/>String s1 = s.toUpperCase();                                                                                                                                         | s1 = "HELLO";                                                                 |
| **String toString()**<br/>String인스턴스에 저장되어 있는 문자열을 반환                                                                                               | String s = "Hello";<br/>String s1 = s.toString();                                                                                                                                            | s1 = "Hello";                                                                 |
| **String trim()**<br/>문자열의 왼쪽 끝과 오른쪽 끝에 있는 공백을 없앤 결과를 반환. 문자열 중간에 있는 공백은 제거되지 않음                                                                    | String s = "&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Hello World&nbsp;&nbsp;&nbsp;";<br/>String s1 = s.trim();                                                                                          | s1 = "Hello World";                                                           |
| **static String valueOf(boolean b)**<br/>지정된 값을 문자열로 변환하여 반환.<br/>참조변수의 경우, toString()을 호출한 결과를 반환                                                  | String b = String.valueOf(true);<br/>String c = String.valueOf('a');<br/>String i = String.valueOf(100);<br/>java.util.Date dd = new java.util.Date();<br/>String date = String.valueOf(dd); | b = "true"<br/>c = "a"<br/>i = "100"<br/>date = "Wed Jan 27 21:26:29 KST 2022" |

#### join()과 StringJoiner
: join()은 여러 문자열 사이에 구분자를 넣어서 결합. 문자열을 자르는 split()과 반대

```
String animals = "dog,cat,bear";
String[] arr = animals.split(",");  // 문자열을 ',' 구분자로 나눠서 배열에 저장
String str = String.join("-", arr); // 배열의 문자열을 '-'로 구분해서 결합
System.out.println(str);            // dog-cat-bear
```

#### String.format()
: format()은 형식화된 문자열을 만들어내는 간단한 방법. pritf()와 사용방법이 같다.
```
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3+5);
System.out.pritnln(str);        // 3 더하기 5는 8입니다.
```

#### 기본형 값을 String으로 변환
: 숫자로 이루어진 문자열을 숫자로 변환할 때, 숫자에 빈 문자열 ""을 더해주면 된다. valueOf()를 사용하는 방법도 있다.
```
int i = 100;
String str1 = i + "";               // "100"
String str2 = String.valueOf(i);    // "100"
```

#### String을 기본형 값으로 변환
: String을 기본형으로 변환하는 방법은 valueOf()를 쓰거나 parseInt()를 사용하면 된다.
```
int i = Integer.parseInt("100");        // 100
int i2 = Integer.ValueOf("100");        // 100
```
| 기본형 → 문자열                        | 문자열 → 기본형                                                                                                                                                                                                                           |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| String String.valueOf(boolean b)<br/>String String.valueOf(char c)<br/>String String.valueOf(int i)<br/>String String.valueOf(long l)<br/>String String.valueOf(float f)<br/>String String.valueOf(double d) | boolean Boolean.parseBoolean(String s)<br/>byte Byte.parseByte(String s)<br/>short Short.parseShort(String s)<br/>int  Integer.parseInteger(String s)<br/>long Long.parseLong(String s)<br/>float Float.parseFloat(String s)<br/> double Double.parseDouble(String s) |