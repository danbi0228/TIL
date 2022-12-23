## 지네릭스(Generics) 1-2

### 1.5 와일드 카드

    <? extends T> : 와일드 카드의 상한 제한. T와 그 자손들만 가능
    <? super T> : 와일드 카드의 하한 제한. T와 그 조상들만 가능
    <?> : 제한 없음. 모든 타입이 가능. <? extends Object>와 동일

<br>

    Comparator<? super Apple> : Comparator<Apple>, Comparator<Fruit>, Comparator<Object>
    Comparator<? super Grape> : Comparator<Grape>, COmparator<Fruit>, Comparator<Object>

### 1.6 지네릭 메서드
: 메서드의 선언부에 지네릭 타입이 선언된 메서드를 지네릭 메서드라 한다.
Collections.sort()가 지네릭 메서드이며, 지네릭 타입의 선언 위치는 반환 타입 바로 앞이다.

```
static <T> void sort(List<T> list, Comparator<? super T> c)
```

지네릭 클래스에 정의된 타입 매개변수와 지네릭 메서드에 정의된 타입 매개변수는 전혀 별개의 것이다.
같은 타입 문자 T를 사용해도 같은 것이 아니라는 것에 주의해야 한다.

```
class FruitBox<T> {
        ...
    static <T> void sort(List<T> list, Comparator<? super T> c) {
        ...
    }
}
```

static멤버에는 타입 매개 변수를 사용할 수 없지만, 이처럼 메서드에 지네릭 타입을 선언하고 사용하는 것은 가능하다.

### 1.7 지네릭 타입의 형변환
: 지네릭 타입과 넌지네릭(non-generic) 타입간의 형변환은 항상 가능하지만, 경고가 발생한다.
```
Box box = null;
Box<Object> objBox = null;

box = (Box)objBox;          // 가능. 지네릭 타입 → 원시 타입. 경고 발생
objBox = (Box<Object>)box;  // 가능. 원시 타입 → 지네릭 타입. 경고 발생
```

대입된 타입이 다른 지네릭 타입 간에는 형변환이 불가능하다.

```
Box<Object> objBox = null;
Box<String> strBox = null;

objBox = (Box<Object>)strBox;   // 에러. Box<String> → Box<Object>
strBox = (Box<String>)objBox;   // 에러. Box<Object> → Box<String>
```

Box<String>이 Box<? extends Object>로 형변환 가능.
```
Box<? extends Object> wBox = new Box<String>();

-------

static Juice makeJuice(FruitBox<? extends Fruit> box) {  ...  }

FruitBox<? extends Fruit> box = new FruitBox<Fruit>();  // 가능 
FruitBox<? extends Fruit> box = new FruitBox<Apple>();  // 가능 
FruitBox<? extends Fruit> box = new FruitBox<Grape>();  // 가능 
```


### 1.8 지네릭 타입의 제거
: 컴파일러는 지네릭 타입을 이용해서 소스파일을 체크하고, 필요한 곳에 형변환을 넣어준다. 그리고 지네릭 타입을 제거한다.
즉, 컴파일된 파일(*.class)에는 지네릭 타입에 대한 정보가 없다.
→ 이렇게 하는 주된 이유 : 지네릭이 도입되기 이전의 소스 코드와의 호환성을 유지하기 위해서.

1. 지네릭 타입의 경계(bound)를 제거한다.<br>
    : 지네릭 타입dl <T extends Fruit>라면 T는 Fruit으로 치환된다. <T>인 경우는 T는 Object로 치환된다. 그리고 클래스 옆의 선언은 제거된다.
```
class Box<T extends Fruit> {
    void add(T t) {
        ...
    }
}

// ↓

class Box {
    void add(Fruit t) {
        ...
    }
}
```

2. 지네릭 타입을 제거한 후에 타입이 일치하지 않으면, 형변환을 추가한다.
    : List의 get()은 Object타입을 반환하므로 형변환이 필요하다.

```
T get(int i) {
    return list.get(i);
}

// ↓

Fruit get(int i) {
    return (Fruit)list.get(i);
}
```