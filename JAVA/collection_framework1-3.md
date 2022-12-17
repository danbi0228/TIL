# 컬렉션 프레임워크 1-3

### 1.5 Iterator, ListIterator, Enumeration
: Iterator, LIstIterator, Enumeration은 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스이다.
<br> Enumeration은 Iterator의 구버전이며, ListIterator는 Iterator의 기능을 향상 시킨 것

#### Iterator
: 컬렉션 프레임워크에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화하였다.
컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator인터페이슬르 정의하고,
Collection인터페이스에는 'Iterator(Iterator를 구현한 클래스의 인터페이스)'를 반환하는 iterator()를 정의하고 있다.

```
public interface Iterator {
    boolean hasNext();
    Object next();
    void remove();
}

public interface Collection {
    ...
    public Iterator iterator();
    ...
}
```

iterator()는 Collection인터페이스에 정의된 메서드이므로 Collection인터페이스의 자손인 List와 Set에도 포함되어 있다.
그래서 List나 Set인터페이스를 구현하는 컬렉션은 iterator()가 각 컬렉션의 특징에 알맞게 작성되어 있다.
컬렉션 클래스에 대해 iterator()를 호출하여 Iterator를 얻은 다음 반복문(주로 while문)을 사용해서 컬렉션 클래스의 요소들을 읽어올 수 있다.

| 메서드               | 설명                                                                      |
|-------------------|-------------------------------------------------------------------------|
| boolean hasNext() | 읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환                            |
| Object next()     | 다음 요소를 읽어온다. next()를 호출하기 전에 hasNext()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다. |
| void remove()     | next()로 읽어온 요소를 삭제한다. next()를 호출한 다음에 remove()를 호출해야한다. (선택적 가능)        |


#### ListIterator와 Enumeration
- Enumeration : Iterator의 구버전
- ListIterator : Iterator의 양방향 조회기능추가(List를 구현한 경우만 사용 가능)


Enumeration인터페이스의 메서드

| 메서드                       | 설명                                                                                                            |
|---------------------------|---------------------------------------------------------------------------------------------------------------|
| boolean hasMoreElements() | 읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환한다. Iterator의 hasNext()와 같다.                                      |
| Object nextElement()      | 다음 요소를 읽어온다. nextElement()를 호출하기 전에 hasMoreElements()를 호출해서 읽어올 요소가 남아있는지 확인하는 것이 안전하다. Iterator의 nest()와 같다. |

ListIterator인터페이스의 메서드

| 메서드                   | 설명                                                                                                        |
|-----------------------|-----------------------------------------------------------------------------------------------------------|
| void add(Object o)    | 컬렉션에 새로운 객체(o)를 추가한다.(선택적 가능)                                                                             |
| boolean hasNext()     | 읽어 올 다음 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환                                                           |
| boolean hasPrevious() | 읽어 올 이전 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환                                                           |
| Object next()         | 다음 요소를 읽어온다. next()를 호출하기 전에 hashNext()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다.                                  |
| Object Previous()     | 이전 요소를 읽어온다. next()를 호출하기 전에 hashPrevious()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전하다.                              |
| int nextIndex()       | 다음 요소의 index를 반환한다.                                                                                       |
| int previousIndex()   | 이전 요소의 index를 반환한다.                                                                                       |
| void remove()         | next() 또는 previous()로 읽어 온 요소를 삭제한다. 반드시 next()나 previous()를 먼저 호출한 다음에 이 메서드를 호출해야한다.(선택적 가능)            |
| void set(Object o)    | next() 또는 previous()로 읽어 온 요소를 지정된 객체(o)로 변경한다. 반드시 next()나 previous()를 먼저 호출한 다음에 이 메서드를 호출해야한다.(선택적 가능) |


### 1.6 Arrays
: Arrays클래스에는 배열을 다루는데 유용한 메서드가 정의되어 있다. 
Arrays에 정의된 메서드는 모두 static메서드이다.

#### 배열의 복사 - copyOf(), copyOfRange()
: copyOf()는 배열 전체를, copyOfRange()는 배열의 일부를 복사해서 새로운 배열을 만들어 반환.
copyOfRange()에 지정된 범위의 끝은 포함되지 않는다.
```
int[] arr = {0, 1, 2, 3, 4};
int[] arr2 = Arrays.copyOf(arr, arr.length);    // arr2 = [0,1,2,3,4]
int[] arr3 = Arrays.copyOf(arr, 3);             // arr3 = [0,1,2]
int[] arr4 = Arrays.copyOf(arr, 7);             // arr4 = [0,1,2,3,4,0,0]
int[] arr5 = Arrays.copyOfRange(arr, 2, 4);     // arr5 = [2,3] -> 4는 불포함
int[] arr6 = Arrays.copyOfRange(arr, 0, 7);     // arr6 = [0,1,2,3,4,0,0]
```

#### 배열 채우기 - fill(), setALl()
: **fill()** 은 배열의 모든 요소를 지정된 값으로 채운다. **setAll()** 은 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다.
<br> 이 메서드를 호출할 떄는 함수형 인터페이스를 구현한 객체를 매개변수로 지정하던가 아니면 람다식을 지정해야 한다.

```
int[] arr = new int[5];
Arrays.fill(arr, 9);    // arr = [9,9,9,9,9]
Arrays.setAll(arr, () -> (int)(Math.random() * 5) + 1);     // arr=[1,5,2,1,1]
```

#### 배열의 정렬과 검색 - sort(), binarySearch()
: sort()는 배열을 정렬할 떄, 그리고 배열에 저장된 요소를 검색할 때는 binarySearch()를 사용한다.
<br> binarySearch()는 배열에서 지정된 값이 저장된 위치(index)를 찾아서 반환하는데, 반드시 배열이 정렬된 상태여야 올바를 겨로가를 얻는다.
<br> 그리고 검색한 값과 일치하는 요소들이 여러 개 있다면, 이 중에서 어떤 것의 위치가 반환될지는 알 수 없다.

```
int[] arr = {3, 2, 0, 1, 4};
int idx = Arrays.binarySearch(arr, 2);      // idx = -5 <- 잘못된 결과

Arrays.sort(arr);       // 배열 arr을 정렬한다.
System.out.println(Arrays.toString(arr));   // [0, 1, 2, 3, 4]
int idx = Arrays.binarySearch(arr, 2);      // idx = 2 <- 올바른 결과
```

#### 배여의 비교와 출력 - equals(), toString()
: toString()배열의 모든 요소를 문자열로 편하게 출력할 수 있다. toString()은 일차원 배열에만 사용할 수 있으므로, 다차원 배열에는 deepToString()을 사용해야 한다.
<br> **deepToString()** 은 배열의 모든 요소를 재귀적으로 접근해서 문자열을 구성하므로 2차원뿐만 아니라 3차원 이상의 배열에도 동작한다.

```
int[] arr = {0,1,2,3,4};
int[][] arr2D = {{11,12}, {21,22}};

System.out.println(Arrays.toString(arr));       // [0, 1, 2, 3, 4]
System.out.println(Arrays.deepToString(arr));   // [[11, 12], [21, 22]]
```

equals()는 두 배열에 저장된 모든 요소를 비교해서 같으면 true, 다르면 false를 반환한다.
equals()도 일차원 배열에만 사용가능하므로, 다차원 배열의 비교에는 **deepEquals()** 를 사용
```
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Arrays.equals(str2D, str2D2));       // false
System.out.println(Arrays.deepEquals(str2D, str2D2));   // true
```

#### 배열을 List로 변환 - asList(Object... a)
: asList()는 배열을 List에 담아서 반환한다. 매개변수의 타입이 가변인수라서 배열 생성없이 저장할 요소들만 나열하는 것도 가능하다.
```
List list = Arrays.asList(new Integer[]{1,2,3,4,5});        // list = [1,2,3,4,5]
List list = Arrays.asList(1,2,3,4,5);                       // list = [1,2,3,4,5]
list.add(6);        // UnsupportedOperationException 예외 발생
```

asList()가 반환한 List의 크기는 변경할 수 없다. 추가 또는 삭제가 불가능하다.
저장된 내용의 변경은 가능하다.
```
// 크기를 변경할 수 있는 List가 필요할 때
List list = new ArrayList(Arrays.asList(1,2,3,4,5));
```

#### parallelXXX(), spliterator(), stream()
: 'parallel'로 시작하는 이름의 메서드들이 있는데, 이 메서드들은 보다 빠른 결과를 얻기 위해 여러 쓰레드가 작업을 나누어 처리하도록 한다.
<br> spliterator()는 여러 쓰레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 Spliterator를 반환하며, stream()은 컬렉션을 스트림으로 변환한다.
