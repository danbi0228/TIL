# 클래스

## 1. 유용한 클래스

### 1.1 java.util.Objects클래스
: Object클래스의 보조 클래스로 Math클래스처럼 모든 메서드가 'static'이다. 객체의 비교나 널체크(Null check)에 유용하다
<br>**isNull()** 은 해당 객체가 널인지 확인해서 null이면 true를 반환하고 아니면 false를 반환한다. **nonNull()** 은 isNull()과 정반대이다. 즉 !Object.isNull(obj)와 같다.
```
static boolean isNull(Object obj)
static boolean nonNull(Object obj)
```
**requireNonNull()** 은 해당 객체가 널이 아니어야 하는 경우에 사용. 만약 객체가 널이면 NullPointerException을 발생시킨다.
```
static <T> T requireNonNull(T obj)
static <T> T requireNonNull(T obj, String message)
static <T> T requireNonNull(T obj, Supplier<String> messageSupplier)
```
대소 비교를 위한 compare(), 두 비교대상이 같으면 0, 크면 양수, 작으면 음수를 반환한다
```
static int compare(Object a, Object b, Comparator c)

static boolean equals(Object a, Object b)
static boolean deepEquals(Object a, Object b)

----------

if(Objects.equals(a,b)){        // 매개변수의 값이 null인지 확인할 필요가 없다
    ...
}
```
**equals()** 내부에서 a와 b의 널 검사를 하기 때문에 따로 널 검사를 위한 조건식을 따로 넣지 않아도 된다.
<br> **deepEquals()** 는 객체를 재귀적으로 비교하기 때문에 다차원 배열의 비교도 가능하다.
```
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D1 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Objects.equals(str2D, str2D2));          // false
System.out.println(Objects.deepEquals(str2D, str2D2));      // true
```
toString()도 equals()처럼 내부적으로 널 검사를 한다.<br>
<br>hashCode()도 내부적으로 널 검사를 한 후에 Object클래스의 hashCode()를 호출한다. 단, 널일 때는 0을 반환한다.
```
static int hashCode(Object o)
static int hash(Object... values)
```

### 1.2 java.util.Random 클래스
: Random클래스를 사용하면 난수를 얻을 수 있다. Math.random()은 내부적으로 Random클래스의 인스턴스를 생성해서 사용하는 것이므로 둘 중에서 편한 것을 사용하면 된다.
```
double randNum = Math.random();
double randNum = new Random().nextDouble();     // 위의 문장과 동일
```
Math.random()과 Random클래스의 가장 큰 차이점은 종자값(seed)을 설정할 수 있다는 것이다. 종자값이 같은 Random인스턴스들은 항상 같은 난수를 같은 순서대로 반환한다.
<br>종자값은 난수를 만드는 공식에 사용되는 값으로 같은 공식에 같은 값을 넣으면 같은 결과를 얻는 것처럼 같은 종자값을 넣으면 같은 난수를 얻게 된다.

#### Random클래스의 생성자와 메서드
: 생성자 Random()은 종자값을 System.currentTimeMillis()로 하기 때문에 실행할 때마다 얻는 난수가 다르다.
```
public Random() {
    this(System.currentTimeMillis());       // Random(long seed)를 호출한다.
}
```

| 메서드                          | 설명                                                                  |
|------------------------------|---------------------------------------------------------------------|
| Random()                     | 현재시간(System.currentTimeMilis())을 종자값(seed)으로 이용하는 Random인스턴스를 생성한다. |
| Random(long seed)            | 매개변수seed를 종자값으로 하는 Random인스턴스를 생성한다.                                |
| boolean nextBoolean()        | boolean타입의 난수를 반환한다.                                                |
| void nextBytes(byte[] bytes) | bytes배열에 byte타입의 난수를 채워서 반환한다.                                      |
| double nextDouble()          | double타입의 난수를 반환한다. (0.0 <= x < 1.0)                                |
| float nextFloat()            | float타입의 난수를 반환한다. (0.0 <= x < 1.0)                                 |
| double nextGaussian()        | 평균은 0.0이고 표준편차는 1.0인 가우시안(Gaussian)분포에 따른 double형의 난수를 반환한다.        |
| int nextInt()                | int타입의 난수를 반환한다. (int의 범위)                                          |
| int nextInt(int n)           | 0 ~ n의 범위에 있는 int값을 반환한다.(n은 범위에 포함되지 않음)                           |
| long nextLong()              | long타입의 난수를 반환한다.(long의 범위)                                         |
| void setSeed(long seed)      | 종자값을 주어진 값(seed)으로 변경한다.                                            |

- int[] fillRand(int[] arr, int from, int to)
  <br> : 배열 arr을 from과 to범위의 값들로 채워서 반환한다.
- int[] fillRand(int[] arr, int[] data)
  <br> : 배열 arr을 배열 data에 있는 값들로 채워서 반환한다.
- int getRand(int from, int to)
  <br> : from과 to범위의 정수(int)값을 반환한다. from과 to 모두 범위에 포함한다.

### 1.3 정규식(Regular Expression) - java.util.regex패키지
: 정규식이란 텍스트 데이터 중에서 원하는 조건(패턴, pattern)과 일치하는 문자열을 찾아내기 위해 사용하는 것으로 미리 정의된 기호와 문자를 이용해서 작성한 문자열을 말한다.
<br> 정규식을 이용하면 많은 양의 텍스트 파일 중에서 원하는 데이터를 손쉽게 뽑아낼 수도 있고 입력된 데이터가 형식에 맞는지 체크할 수도 있다.

<br>정규식을 정의하고 데이터를 비교하는 과정을 단계별로 설명
1. 정규식을 매개변수로 Pattern클래스의 static메서드인 Pattern compile(String regex)을 호출하여 Pattern인스턴스를 얻는다.
    : Pattern p = Pattern.compile("c[a-z]*");
2. 정규식으로 비교할 대상을 매개변수로 Pattern클래스의 Matcher matcher(CharSequence input)를 호출해서 Matcher인스턴스를 얻는다.
    : Matcher m = p.matcher(data[i]);
3. Matcher인스턴스에 boolean matches()를 호출해서 정규식에 부합하는지 확인한다.
    : if(m.matches())

| 정규식 패턴                            | 설명                                                                                       | 결과                                                     |
|-----------------------------------|------------------------------------------------------------------------------------------|--------------------------------------------------------|
| c[a-z]*                           | c로 시작하는 영단어                                                                              | c, ca, co, car, combat, count ...                      |
| c[a-z]                            | c로 시작하는 두 자리 영단어                                                                         | ca, co  ...                                            |
| c[a-zA-Z]                         | c로 시작하는 두 자리 영단어(a~z 또는 A~Z, 대소문자 구분X)                                                   | cA, ca, co ...                                         |
| c[a-zA-Z0-9]<br/>c\w              | c로 시작하고 숫자와 영어로 조합된 두 글자                                                                 | cA, ca, co, c0 ...                                     |
| .*                                | 모든 문자열                                                                                   | bat, baby, bonus, c, cA, co, c., c0, c#, ...           |
| c.                                | c로 시작하는 두 자리 문자열                                                                         | cA, ca, co, c., c0, c# ...                             |
| c.*                               | c로 시작하는 모든 문자열(기호 포함)                                                                    | cA, ca, co, c., c0, c#, car, combat ...                |
| c\.                               | c.와 일치하는 문자열'.'은 패턴작성에 사용되는 문자이므로 escape문자인 '\'를 사용해야 한다                                 | c.                                                     |
| c\d<br/>c[0-9]                    | c와 숫자로 구성된 두 자리 문자열                                                                      | c0  ...                                                |
| c.*t                              | c로 시작하고 t로 끝나는 모든 문자열                                                                    | combat, count ...                                      |
| [b│c].*<br/>[bc].*<br/>[b-c].*    | b 또는 c로 시작하는 문자열                                                                         | bat, baby, bonus, c, cA, ca, co, c., c0, c#, count ... |
| [^b│c].*<br/>[^bc].*<br/>[^b-c].* | b 또는 c로 시작하지 않는 문자열                                                                      | date, disc ...                                         |
| .*a.*                             | a를 포함한 모든 문자열<br/>* : 0 또는 그 이상의 문자                                                      | bat, baby, ca, car, combat, date ...                   |
| .*a.+                             | a를 포함하는 모든 문자열<br/>+ : 1 또는 그 이상의 문자. '+'는 '*'과 달리 반드시 하나 이상의 문자가 있어야 하므로 a로 끝나는 단어는 포함X | bat, baby, car, combat, date ...                       |
| [b│c].{2}                         | b 또는 c로 시작하는 세 자리 문자열(b 또는 c 다음에 두 자리이므로 모두 세자리)                                         | bat, car                                               |


정규식의 일부를 괄호로 나누어 묶어서 그룹화(grouping)할 수 있다. 
<br>그룹화된 부분은 하나의 단위로 묶이는 셈이 되어서 한 번 또는 그 이상의 반복을 의미하는 '+'나 '*'가 뒤에 오면 그룹화된 부분이 적용대상이 된다.
<br> 그리고 그룹화된 부분은 group(int i)를 이용해서 나누어 얻을 수 있다.

| 정규식 패턴    | 설명                              |
|-----------|---------------------------------|
| 0\\d{1,2} | 0으로 시작하는 최소 2자리 최대 3자리 숫자(0 포함) |
| \\d{3,4}  | 최소 3자리 최대 4자리의 숫자               |
| \\d{4}    | 4자리의 숫자                         |


### 1.4 java.util.Scanner클래스
: Scanner는 화면, 파일, 문자열과 같은 입력소스로부터 문자데이터를 읽어오는데 도움을 줄 목적을 가지고 있다. 다양한 입력소스로부터 데이터를 읽어올 수 있다.
```
Scanner(String source)
Scanner(File source)
Scanner(InputStream source)
Scanner(Readable source)
Scanner(RedableByteChannel source)
Scanner(Path source)
```

Scanner는 정규식 표현을 이용한 라인단위의 검색을 지원하며 구분자에도 정규식을 표현할 수 있어서 복잡한 형태의 구분자도 처리가 가능
```
Scanner useDelimiter(Pattern pattern)
Scanner useDelimiter(String pattern)
```

```
boolean nextBoolean()
byte nextByte()
short nextShort()
int nextInt()
long nextLong()
double nextDouble()
float nextFloat()
String nextString()
```

### 1.5 java.util.StringTokenizer클래스
: StringTokenizer는 긴 문자열을 지정된 구분자를 기준으로 토큰(token)이라는 여러 개의 문자열로 잘라내는 데 사용된다.
<br> Ex) "100,200,300,400"이라는 문자열이 있을 때 ','를 구분자로 잘라내면 "100","200","300","400"이라는 4개의 문자열(토큰)을 얻을 수 있다.
```
String[] result = "100,200,300,400".split(",");
Scanner sc2 = new Scanner("100,200,300,400").useDelimiter(",");     // 정규식 표현을 사용
```

StringTokenizer는 구분자로 단 하나의 문자밖에 사용하지 못하기 때문에 보다 복잡한 형태의 구분자로 문자열을 나누어야 할 때는 어쩔 수 없이 정규식을 사용하는 메서드를 사용해야 한다.

#### StringTokenizer의 생성자와 메서드
| 생성자 / 메서드                                                       | 설명                                                                                             |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| StringTokenizer(String str, String delim)                       | 문자열(str)을 지정된 구분자(delim)로 나누는 StringTokenizer를 생성한다.(구분자는 토큰으로 간주 X)                           |
| StringTokenizer(String str, String delim, boolean returnDelims) | 문자열(str)을 지정된 구분자(delim)로 나누는 StringTokenizer를 생성한다. returnDelims의 값을 true로 하면 구분자도 토큰으로 간주된다. |
| int countTokens()                                               | 전체 토큰의 수를 반환한다.                                                                                |
| boolean hasMoreTokens()                                         | 토큰이 남아있는지 알려준다.                                                                                |
| String nextToken()                                              | 다음 토큰을 반환한다.                                                                                   |


### 1.6 java.math.BigInteger클래스
: 가장 큰 정수형 타입인 long으로 표현할 수 있는 값인 (10진수) 19자리보다 더 큰 값을 다뤄야할 때 사용하면 좋은 클래스이다.
<br>BigInteger는 내부적으로 int배열을 사용해서 값을 다룬다. 그래서 long보다 훨씬 큰 값을 다룰 수 있다.
```
final int signum;       // 부호. 1(양수), 0, -1(음수) 셋 중의 하나
final int[] mag;        // 값(magitude)
```

BigInteger는 String처럼 불변이다. 그리고 모든 정수형이 그렇듯 BigInteger 또한 값을 '2의 보수'의 형태로 표현한다.


#### BigInteger의 생성
: 문자열을 숫자로 표현하는 것이 일반적이다. 정수형 리터럴로는 표현할 수 있는 값의 한계가 있기 때문이다.

```
BigInteger val;
val = new BigInteger("12345678901234567890")    // 문자열로 생성
val = new BigInteger("FFFF", 16);               // n진수(radix)의 문자열로 생성
val = BigInteger.valueOf(1234567890L);          // 숫자로 생성
```

#### 다른 타입으로의 변환
: BigInteger를 문자열 또는 byte배열로 변환하는 메서드
```
String toString()           // 문자열로 변환
String toString(int radix)  // 지정된 진법의 문자열로 변환
byte[] toByteArray()        // byte배열로 변환
```

BigInteger도 Number로부터 상속받은 기본형으로 변환하는 메서드를 가지고 있다.
```
int intValue()
long longValue()
float floatValue()
double doubleValue()

---
// 정수형으로 변환하는 메서드 중에서 이름 끝에 'Exact'가 붙은 것들은 변환한 결과가
// 변환한 타입의 범위에 속하지 않으면 ArithmeticException을 발생시킨다

byte byteValueExact()
int intValueExact()
long longValueExact()
```

#### BigInteger의 연산
: BigInteger에는 정수형을 사용할 수 있는 모든 연산자와 수학적인 계산을 쉽게 해주는 메서드들이 정의.
```
BigInteger add(BigInteger val)          // 덧셈(this + val)
BigInteger subtract(BigInteger val)     // 뺄셈(this - val)
BigInteger multiply(BigInteger val)     // 곱셈(this * val)
BigInteger divide(BigInteger val)       // 나눗셈(this / val)
BigInteger remainder(BigInteger val)    // 나머지(this % val)
```

#### 비트 연산 메서드
```
int bitCount()          // 2진수로 표현했을 때, 1개의 개수(음수는 0개의 개수)를 반환
int bitLength()         // 2진수로 표현했을 때, 값을 표현하는데 필요한 bit수
boolean testBit(int n)  // 우측에서 n+1번째 비트가 1이면 true, 0이면 false
BigInteger setBit(int n)    // 우측에서 n+1번째 비트를 1로 변경
BigInteger clearBit(int n)  // 우측에서 n+1번째 비트를 0으로 변경
BigInteger flipBit(int n)   // 우측에서 n+1번째 비트를 전환(1→0, 0→1)

```

### 1.7 java.math.BigDecimal클래스
: double타입으로 표현할 수 있는 값의 범위는 넓지만, 정밀도가 최대 13자리 밖에 되지 않고 오차를 피할 수 없다.
<br> BigDecimal은 실수형과 달리 정수를 이용해서 실수를 표현한다. 오차가 없는 2진 정수로 변환하여 다룬다. 실수를 정수와 10의 제곱의 곱으로 표현한다.
```
private final BigInteger intVal;        // 정수
private final int scale;                // 지수
private transient int precision;        // 정밀도 - 정수의 자릿수
```

#### BigDecimal의 생성
: 문자열로 숫자를 표현하는 것이 일반적이다. 기본형 리터럴로는 표현할 수 있는 값의 한계가 있기 때문

```
BigDecimal val;
val = new BigDecimal("123.4567890");            // 문자열로 생성
val = new BigDecimal(123.456);                  // double타입의 리터럴로 생성
val = new BigDecimal(123456);                   // int, long타입의 리터럴로 생성 가능
val = BigDecimal.valueOf(123.456);              // 생성자 대신 valueOf(double) 사용
val = BigDecimal.valueOf(123456);               // 생성자 대신 valueOf(int) 사용
```

#### 다른 타입으로의 변환
```
String toPlainString()      // 어떤 경우에도 다른 기호없이 숫자로만 표현
String toString()           // 필요하면 지수형태로 표현할 수도 있음.
```
```
int intValue()
long longValue()
float floatValue()
double doubleValue()

------

byte byteValueExact()
short shortValueExact()
int intValueExact()
long longValueExact()
BitInteger toBigIntegerExact()
```

#### BigDecimal의 연산
```
BigDecimal add(BigDecimal val)               // 덧셈(this + val)
BigDecimal subtract(BigDecimal val)          // 뺄셈(this - val)
BigDecimal multiply(BigDecimal val)          // 곱셈(this * val)
BigDecimal divide(BigDecimal val)            // 나눗셈(this / val)
BigDecimal remainder(BigDecimal val)         // 나머지(this % val)
```

#### 반올림 모드 - divide()와 setScale()
: 열거형 RoundingMode

| 상수          | 설명                                             |
|--------------|------------------------------------------------|
| CEILING     | 올림                                             |
| FLOOR       | 내림                                             |
| UP          | 양수일 때는 올림, 음수일 때는 내림                           |
| DOWN        | 양수일 때는 내림, 음수일 떄는 올림(UP과 반대)                   |
| HALF_UP     | 반올림(5이상 올림, 5미만 버림)                            |
| HALF_EVEN   | 반올림(반올림 자리의 값이 짝수면 HALF_DOWN, 홀수면 HALF_UP)     |
| HALF_DOWN   | 반올림(6이상 올림, 6미만 버림)                            |
| UNNECESSARY | 나눗셈의 결과가 딱 떨어지는 수가 아니면, ArithmeticException 발생 |


#### java.math.MathContext
: 반올림 모드와 정밀도를 하나로 묶어놓은 것.
<br> divide()에서는 scale이 소수점 이하의 자리수를 의미하는데, MathContext에서는 precision이 정수와 소수점 이하를 모두 포함한 자리수를 의미한다.

#### scale의 변경
: BigDecimal을 10으로 곱하거나 나누는 대신 scale의 값을 변경함으로써 같은 결과를 얻을 수 있다.
<br>BigDecimal의 scale을 변경하려면, setScale()을 이용하면 된다.
```
BigDecimal setScale(int newScale)
BigDecimal setScale(int newScale, int roundingMode)
BigDecimal setScale(int newScale, RoundingMode mode)
```
