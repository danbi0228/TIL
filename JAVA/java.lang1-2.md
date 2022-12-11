# java.lang패키지 1-1

### 1.3 StringBuffer클래스와 StringBuilder클래스
: String클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없지만 StringBuffer클래스는 변경이 가능하다.

```
public final class StringBuffer implements java.io.Serializable {
    private char[] value;
        ...
}
```

#### StringBuffer의 생성자
: **StringBuffer클래스**의 인스턴스를 생성할 때, 적절한 길이의 char형 배열이 생성되고, 이 배열은 문자열을 저장하고 편집하기 위한 공간(buffer)으로 사용된다.
<br> **StringBuffer인스턴스**를 생성할 때는 생성자 StringBuffer(int length)를 사용해서 StringBuffer인스턴스에 저장될 문자열의 길이를 고려하여 충분히 여유있는 크기로 지정하는 것이 좋다.

```
public StringBuffer(int length) {
    value = new char[length];
    shared = false;
}

public StringBuffer() {
    this(16);           // Buffer의 크기를 지정하지 않으면 Buffer의 크기는 16이 된다.
}

public StrinfBuffer(String str){
    this(str.length() + 16);        // 지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성
    append(str);
}
```

#### StringBuffer의 비교
: String클래스에서는 equals메서드를 오버라이딩해서 문자열의 내용을 비교하도록 구현되었지만, **StringBuffer클래스**는 equals메서드를 오버라이딩 하지 않아서 StringBuffer클래스의 equals메서드를 사용해도 등가비교연산자(==)로 비교한 것과 같다.

```
StringBuffer sb = new StringBuffer("abc");
StringBuffer sb2 = new StringBuffer("abc");

System.out.println(sb == sb2);      // false
System.out.println(sb.equals(sb2)); // false
```

toString()은 오버라이딩이 되어 있어서 StringBuffer인스턴스에 **toString()**을 호출하면 담고있는 문자열을 String으로 반환한다.
<br>그래서 StringBuffer인스턴스에 담긴 문자열을 비교하기 위해서는 StringBuffer인스턴스에 toString()을 호출해서 String인스턴스를 얻은 다음, 여기에 equals메서드를 사용해서 비교해야 한다.
```
String s = sb.toString();
String s2 = sb2.toString();

System.out.println(s.equals(s2));   // true
```

#### StringBuffer클래스의 생성자와 메서드
| 메서드 / 설명                                                                                                                                                                                       | 예제 / 결과                                                                                                                                                                                                                                                                         |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StringBuffer()**<br/><br/>16문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스를 생성                                                                                                                         | StringBuffer sb = new StringBuffer();<hr>sb = ""                                                                                                                                                                                                                                |
| **StringBuffer(int length)**<br/><br/> 지정된 개수의 문자를 담을 수 있는 버퍼를 가진 StringBuffer인스턴스를 생성                                                                                                         | StringBuffer sb = new StringBuffer(10);<hr>sb = ""                                                                                                                                                                                                                              |
| **StringBuffer(String str)**<br/><br/> 지정된 문자열 값(str)을 갖는 StringBuffer 인스턴스를 생성                                                                                                                | StringBuffer sb = new StringBuffer("Hi");<hr>sb = "Hi"                                                                                                                                                                                                                          |
| **StringBuffer append(boolean b)**<br/>**StringBuffer append(int i)**<br/>**StringBuffer append(String str)**<br/><br/>매개변수로 입력된 값을 문자열로 변환하여 StringBuffer인스턴스가 저장하고 있는 문자열의 뒤에 덧붙인다           | StringBuffer sb = new StringBuffer("abc");<br/>StringBuffer sb2 = sb.append(true);<br/>sb.append('d').append(10.0f);<br/>StringBuffer sb3 = sb.append("ABC").append(123);<hr>sb = "abctrued10.0ABC123"<br/>sb2 = "abctrued10.0ABC123"<br/>sb3 = "abctrued10.0ABC123"            |
| **int capacity()**<br/><br/>StringBuffer인스턴스의 버퍼크기를 알려준다. length()는 버퍼에 담긴 문자열의 길이를 알려준다.                                                                                                      | String Buffer sb = new StringBuffer(100);<br/>sb.append("abcd");<br/>int bufferSize = sb.capacity();<br/>int stringSize = sb.length();<hr>bufferSize = 100<br/>StringSize = 4(sb에 담긴 문자열이 "abcd"이므로)                                                                            |
| **char charAt(int index)**<br/><br/>지정된 위치(index)에 있는 문자를 반환                                                                                                                                   | StringBuffer sb = new StringBuffer("abc");<br/>char c = sb.charAt(2); <hr> c = 'c'                                                                                                                                                                                              |
| **StringBuffer delete (int start, int end)**<br/><br/>시작 위치부터 끝위치 사이에 있는 문자를 제거. 단, 끝위치의 문자는 제외.                                                                                               | StringBuffer sb = new StringBuffer("0123456");<br/>StringBuffer sb2 = sb.delete(3,6); <hr> sb = "0126"<br/>sb2 = "0126"                                                                                                                                                         |
| **StringBuffer insert (int pos, boolean b)**<br/>**StringBuffer insert (int pos, int i)**<br/>**StringBuffer insert (int pos, String str)**<br/><br/>두번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치(pos)에 추가 | StringBuffer sb = new StringBuffer("0123456");<br/>sb.insert(4,'.'); <hr> sb = "0123.456"                                                                                                                                                                                       |
| **int length()**<br/><br/>StringBuffer인스턴스에 저장되어 있는 문자열의 길이를 반환                                                                                                                                | StringBuffer sb = new StringBuffer("0123456");<br/>int length = sb.length(); <hr> length = 7                                                                                                                                                                                    |
| **StringBuffer replace(int start, int end, String str)**<br/><br/>지정된 범위(start~end)의 문자들을 주어진 문자열로 바꾼다. end위치의 문자는 범위에 포함X                                                                     | StringBuffer sb = new StringBuffer("0123456");<br/>sb.replace(3, 6, "AB"); <hr> sb = "012AB6"                                                                                                                                                                                   |
| **StringBuffer reverse()**<br/><br/>StringBuffer인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열                                                                                                                  | StringBuffer sb = new StringBuffer("0123456");<br/>sb.reverse(); <hr> sb = "6543210"                                                                                                                                                                                            |
| **void setCharAt(int index, char ch)**<br/><br/>지정된 위치의 문자를 주어진 문자(ch)로 바꾼다.                                                                                                                   | StringBuffer sb = new StringBuffer("0123456");<br/>sb.setCharAt(5, 'o'); <hr> sb = "01234o6"                                                                                                                                                                                    |
| **void setLength(int newLength)**<br/><br/>지정된 길이로 문자열의 길이를 변경한다. 길이를 늘리는 경우에 나머지 빈공간을 널문자 '\u0000'로 채운다                                                                                       | StringBuffer sb = new StringBuffer("0123456");<br/>sb.setLength(5);<br/>StringBuffer sb2 = new StringBuffer("0123456");<br/>sb2.setLength(10);<br/>String str = sb2.toString().trim(); <hr> sb = "01234"<br/>sb2 = "0123456 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"<br/>sb3 = "0123456" |
| **String toString()**<br/><br/>StringBuffer인스턴스의 문자열을 String으로 변환                                                                                                                              | StringBuffer sb = new StringBuffer("0123456");<br/>String str = sb.toString(); <hr> str = "0123456"                                                                                                                                                                             |
| **String substring(int start)**<br/>**String substring(int start, int end)**<br/><br/>지정된 범위 내의 문자열을 String으로 뽑아서 반환. 시작위치(start)만 지정하면 시작위치부터 문자열 끝까지 뽑아서 반환                                  | StringBuffer sb = new StringBuffer("0123456");<br/>String str = sb.substring(3);<br/>String str2 = sb.substring(3, 5); <hr> str = "3456"<br/>str2 = "34"                                                                                                                         |


#### StringBuilder
: 멀티쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능만 떨어뜨린다.
<br>그래서 StringBuffer에서 쓰레드의 동기화만 뺀 **StringBuilder**가 새로 추가되었다. StringBuffer와 완전히 똑같은 기능이기 때문에 StringBuffer에서 StringBuilder를 사용하도록 바꾸기만 하면 된다.

```
StringBuffer sb = new StringBuffer();
sb.append("abc");
---
StringBuilder sb = new StringBuilder();
sb.append("abc");
```

### 1.4 Math클래스
: Math클래스는 기본적인 수학계산에 유용한 메서드로 구성되어 있다.
<br>Math클래스의 생성자는 접근제어자가 private이기 때문에 다른 클래스에서 Math인스턴스를 생성할 수 없도록 되어있다. → 클래스 내에 인스턴스변수가 하나도 없어서 인스턴스를 생성할 필요가 없기 때문

#### 올림, 버림, 반올림
1. 원래 값에 100을 곱한다. 90.7552 * 100 → 9075.52
2. 위의 결과에 Math.round()를 사용한다. Math.round(9075.52) → 9076
3. 위의 결과를 다시 100.0으로 나눈다. 9076/100.0 → 90.76, 9076/100 → 90

#### 예외를 발생시키는 메서드
```
int addExact(int x, int y)          // x + y
int subtractExact(int x, int y)     // x - y
int multiplyExact(int x, int y)     // x * y
int incrementExact(int a)           // a++
int decrementExact(int a)           // a--
int negateExact(int a)              // -a
int toIntExact(long value)          // (int)value - int로의 형변환
```

#### StrictMath클래스
| 메서드 / 설명                                                                                                                                             | 예제                                                                                                                                                       | 결과                                                          |
|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| **static double abs(double a)**<br/>**static float abs(float f)**<br/><br/>주어진 값의 절대값을 반환                                                            | int i = Math.abs(-10);<br/>double d = Math.abs(-10.0);                                                                                                   | i = 10<br/>d = 10.0                                         |
| **static double ceil(double a)**<br/><br/>주어진 값을 올림하여 반환                                                                                             | double d = Math.ceil(10.1);<br/>double d2 = Math.ceil(-10.1);<br/>double d3 = Math.ceil(10.000015)                                                       | d = 11.0<br/>d2 = -10.0<br/>d3 = 11.0                       |
| **static double floor(double a)**<br/><br/>주어진 값을 버림하여 반환                                                                                            | double d = Math.floor(10.8);<br/>double d2 = Math.floor(-10.8);                                                                                          | d = 10.0<br/>d2 = -11.0                                     |
| **static double max(double a, double b)**<br/>**static int max(int a, int b)**<br/><br/>주어진 두 값을 비교하여 큰 쪽을 반환                                        | double d = Math.max(9.5, 9.500001);<br/>int i = Math.max(0, -1);                                                                                         | d = 9.500001<br/>i = 0                                      |
| **static double min(double a, double b)**<br/>**static int min(int a, int b)**<br/><br/>주어진 두 값을 비교하여 작은 쪽을 반환                                       | double d = Math.min(9.5, 9.500001);<br/>int i = Math.min(0, -1);                                                                                         | d = 9.5<br/>i = -1                                          |
| **static double random()**<br/><br/>0.0~1.0범위의 임의의 double값을 반환 (1.0은 포함X)                                                                            | double d = Math.random();<br/>int i = (int)(Math.random() * 10) + 1                                                                                      | 0.0 <= d < 1.0<br/>1 <= i < 11                              |
| **static double rint(double a)**<br/><br/>주어진 double값과 가장 가까운 정수값을 double형으로 반환. 단, 두 정수의 정가운데 있는 값(1.5, 2.5, 3.5 등)은 짝수를 반환                         | double d = Math.rint(1.2);<br/>double d2 = Math.rint(2.6);<br/>double d3 = Math.rint(3.5);                                                               | d = 1.0<br/>d2 = 3.0<br/>d3 = 4.0                           |
| **static long round(double a)**<br/>**static long round(float a)**<br/><br/>소수점 첫째자리에서 반올림한 정수값(long)을 반환. 매개변수의 값이 음수인 경우, round()와 rint()의 결과가 다르다 | long l = Math.round(1.2);<br/>long l2 = Math.round(2.6);<br/>long l3 = Math.round(3.5);<br/>double d = 90.7552;<br/>double d2 = Math.round(d*100)/100.0; | l = 1<br/>l2 = 3<br/>l3 = -4<br/>d = 90.7552<br/>d2 = 90.76 |


### 1.5 래퍼(wrapper) 클래스
: 때로는 기본형변수도 어쩔 수 없이 객체로 다뤄야하는 경우가 있다. ex) 매개변수로 객체를 요구할 때, 기본형 값이 아닌 객체로 저장해야할 때, 객체간의 비교가 필요한 때 등등
<br> 기본형 값들을 객체로 변환하여 작업을 수행해야 하는데 이 때 사용되는 것이 **래퍼(wrapper)클래스**이다.
<br> char형과 int형을 제외한 나머지는 자료형 이름의 첫글자를 대문자로 한 것이 각 래퍼 클래스의 이름이다.
<br> 래퍼클래스의 생성자는 매개변수로 문자열이나 각 자료형의 값들을 인자로 받는다.

| 기본형     | 래퍼클래스         | 생성자                                                               | 예시                                                                                          | 
|---------|---------------|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| boolean | Boolean       | Boolean (boolean value)<br/>Boolean (String s)                    | Boolean b = new Boolean(true);<br/>Boolean b2 = new Boolean("true");                        |
| char    | **Character** | Character (char value)                                            | Character c = new Character('a');                                                           |
| byte    | Byte          | Byte (byte value)<br/>Byte (String s)                             | Byte b = new Byte(10);<br/>Byte b2 = new Byte("10");                                        |
| short   | Short         | Short (short value)<br/>Short (String s)                          | Short s = new Short(10);<br/>Short s2 = new Byte("10");                                     |
| int     | **Integer**   | Integer (int value)<br/>Integer (String s)                        | Integer i = new Integer(100);<br/>Integer i2 = new Integer("100");                          |
| long    | Long          | Long (long value)<br/>Long (String s)                             | Long l = new Long(100);<br/>Long l2 = new Long("100");                                      |
| float   | Float         | Float (double value)<br/>Float (float value)<br/>Float (String s) | Float f = new Float(1.0);<br/>Float f2 = new Float(1.0f);<br/>Float f3 = new Float("1.0f"); |
| double  | Double        | Double (double value)<br/>Double (String s) | Double d = new Double(1.0);<br/>Double f2 = new Double("1.0");                              |

#### 문자열을 숫자로 변환하기
```
int i = new Integer("100").intVale();       // floatValue(), longValue(), ...
int i2 = Integer.paresInt("100");           // 주로 이 방법을 많이 사용
int i3 = Integer.valueOf("100");
```

#### 오토박싱 & 언박싱(autoboxing & unboxing)
: 기본형 값을 래퍼클래스의 객체로 자동 변환해주는 것을 **오토박싱(autoboxing)** 이라고 하고,
<br>반대로 변환하는 것을 **언박싱(unboxing)** 이라고 한다.
```
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(10);               // 오토박싱. 10 → new Integer(10)

int value = list.get(0);    // 언박싱. new Integer(10) → 10
```