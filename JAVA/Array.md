# 배열

## 1. 배열(array)
: 배열은 <b>같은 타입의 여러 변수를 하나의 묶음으로 다루는 것</b>

### 1.1 배열의 선언과 생성

| 선언 방법      | 선언 예                             |
|------------|----------------------------------|
| 타입[] 변수이름; | int[] score; <br/>String[] name; |
| 타입 변수이름[]; | int score[]; <br/>String name[]; |

#### 배열의 생성
: 배열을 선언한 다음에는 배열을 생성해야 한다. 배열을 생성해야 값을 저장할 수 있는 공간이 만들어지는 것.
```
타입[] 변수이름;               // 배열을 선언
변수이름 = new 타입[길이];      // 배열을 생성

-----------------------
int[] score;
score = new int[5];

int[] score = new int[5];
```

### 1.2 배열의 길이와 인덱스
: 생성된 배열의 각 저장공간을 배열의 요소라고 하며, <b>인덱스(index)는 배열의 요소마다 붙여진 일련번호</b>로 각 요소를 구별하는데 사용된다.
<br> 인덱스의 범위는 <b>0부터 '배열길이 -1'까지</b> 이다. ex) 길이가 5인 배열 → 범위:0,1,2,3,4

#### 배열의 길이
: 배열의 길이는 배열의 요소의 개수, 즉 <b>값을 저장할 수 있는 공간의 개수</b>
<br> 배열의 길이는 int 범위의 양의 정수(0도 포함)이어야 한다.

#### 배열이름.length
: 배열은 한번 생성하면 길이를 변경할 수 없기 때문에 이미 생성된 배열의 길이는 변하지 않는다.<br>
 → '배열이름.length'는 상수다. 값을 읽을수만 있을 뿐 변경할 수 없다.

#### 배열의 길이 변경하기
1. 더 큰 배열을 <b>새로 생성</b>한다.
2. 기존 배열의 내용을 <b>새로운 배열에 복사</b>한다.

### 1.3 배열의 초기화
: 배열은 생성과 동시에 자동적으로 기본값으로 초기화되므로 따로 초기화를 해주지 않아도 되지만, 원하는 값을 저장하려면 각 요소마다 값을 지정
<br> 배열의 길이가 큰 경우에는 값을 지정하는 것보다 for문을 사용하는 것이 좋다.(일정한 규칙이 있을때만 가능)
```
int[] score = new int[5];   // 길이가 5인 int 배열 생성
score[0] = 50;              // 각 요소에 직접 값 저장
score[1] = 60;
score[2] = 70;
score[3] = 80;
score[4] = 90;

-----------------

int[] score = new int[5];       // 길이가 5인 int 배열 생성

for (int i=0; i<score.length; i++) {
    score[i] = i * 10 + 50;
}

-----------------

int[] score = new int[]{50,60,70,80,90};    // 배열의 생성과 초기화 동시에
int[] score = {50,60,70,80,90}              // new int[] 생략 가능 

```

#### 배열의 출력
: 배열을 초기화할 때 for문을 사용하듯이, 저장된 값을 확인할 때도 for문을 사용한다.

```
int[] iArr = {100,95,80,70,60};

for(int i=0; i<iArr.length; i++) {
    System.out.println(iArr[i]);
}
```

더 간단한 방법은 <b>'Arrays.toString(배열이름)'</b> 메서드를 사용. 이 메서드는 배열의 모든 요소를 '[첫번째, 두번째, ..]' 와 같은 형식의 문자열로 반환

```
int[] iArr = {100,95,80,70,60};

System.out.println(Arrays.toString(iArr));
// iArr이 참조변수라서 '타입@주소' 형식으로 출력됨.

--------------------------------

char[] chArr = {'a','b','c','d'};

System.out.println(chArr);  // abcd가 출력된다.
```

### 1.4 배열의 복사
1. for문을 이용한 배열을 복사
```
int[] arr = new int[5];

int[] tmp = new int[arr.length*2];  // 길이가 기본 배열의 2배인 배열 생성

for(int i=0; i<arr.length; i++) {
    tmp[i] = arr[i];    // arr[i]의 값을 tmp[i]에 저장
}

arr = tmp;              // 참조변수 arr이 새로운 배열을 가리키게 한다.

```

2. <b>System.arraycopy()</b>를 이용한 배열의 복사
: for문 대신 System클래스의 arraycopy()를 사용하면 보다 간단하고 빠르게 배열을 복사할 수 있다.
<br> arraycopy()는 지정된 범위의 값들을 한 번에 통째로 복사한다.
```
for(int i=0; i<num.length; i++){ newNum[i] = num[i]; }

----------

System.arraycopy(num, 0, newNum, 0, num.length);
// num[0]에서 newNum[0]으로 num.length개의 데이터를 복사

```

## 2. String배열

### 2.1 String배열의 선언과 생성
```
String[] name = new String[3];
```

### 2.2 String배열의 초기화
```
String[] name = new String[3];
name[0] = "Kim";
name[1] = "Park";
name[2] = "Lee";

--------------------

String[] name = new String[] {"Kim","Park","Lee"};
String[] name = {"Kim","Park","Lee"};   // new String[] 생략 가능

```

### 2.3 char배열과 String클래스
: String클래스는 char배열에 기능(메서드)를 추가한 것이다.

#### String클래스의 주요 메서드

| 메서드                                | 설명                                                     |
|------------------------------------|--------------------------------------------------------|
| char charAt(int index)             | 문자열에서 해당 위치(index)에 있는 문자를 반환한다.                       |
| int length()                       | 문자열의 길이를 반환한다.                                         |
| String substring(int from, int to) | 문자열에서 해당 범위(from~to)에 있는 문자열을 반환한다. <br/>(to는 범위에 포함X) |
| boolean equals(Object obj)         | 문자열의 내용이 obj와 같은지 확인한다. 같으면 true, 다르면 false.           |
| char[] toCharArray()               | 문자열을 문자배열(char[])로 변환해서 반환한다.                          |


```
String str = "ABCDE";
char ch = str.charAt(3);    // 문자열 str의 4번째 문자 'D'를 ch에 저장
-----------
String str = "012345";
String tmp = str.substring(1,4);    // str에서 index범위 1~4의 문자들을 반환
System.out.println(tmp);            // "123"반환, 4는 포함X
-----------
String str = "abc";
if (str.equals("abc"))  {       // str와 "abc"가 같은지 확인
    ...
}
```

#### char배열과 String클래스의 변환
```
char[] chArr = {"A", "B", "C"};
String str = new String(chArr);     // char배열 → String
char[] tmp = str.toCharArray();     // String → char배열
```

## 3. 다차원 배열

### 3.1 2차원 배열의 선언과 인덱스

| 선언 방법        | 선언 예           |
|--------------|----------------|
| 타입[][] 변수이름; | int[][] score; |
| 타입 변수이름[][]; | int score[][]; |
| 타입[] 변수이름[]; | int[] score[]; |

```
int[][] score = new int[4][3]   // 4행 3열의 2차원 배열을 생성
```

| **score[0][0]** | **score[0][1]** | **score[0][2]** |
|-----------------|-----------------|-----------------|
| **score[1][0]** | **score[1][1]** | **score[1][2]** |
| **score[2][0]** | **score[2][1]** | **score[2][2]** |
| **score[3][0]** | **score[3][1]** | **score[3][2]** |


### 3.2 2차원 배열의 초기화
```
int[][] arr = new int[][]{ {1, 2, 3}, {4, 5, 6} };
int[][] arr = { {1, 2, 3}, {4, 5, 6} };
```
