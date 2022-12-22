## 지네릭스(Generics) 1-1

### 1.1 지네릭스란?
: **지네릭스**는 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에
**컴파일 시의 타입체크**(compile-time type check)를 해주는 기능
객체의 타입을 컴파일 시에 체크하기 때문에 **객체의 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.**

타입의 안정성을 높인다는 → 의도하지 않은 타입의 객체가 저장되는 것을 막고,
저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄여준다는 뜻.

- **지네릭스**의 장점
  - 타입 안정성을 제공한다.
  - 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.

### 1.2 지네릭 클래스의 선언
: 지네릭 타입은 클래스와 메서드에 선언할 수 있다.

```
class Box {
    Object item;
    
    void setItem(Object item) {  this.item = item;  }
    Object getItem() {  return item;  }
}

// 이 클래스를 지네릭 클래스로 변경하려면
// 클래스 옆에 '<T>'를 붙이고 'Object'를 모두 'T'로 바꿔주면 된다.

class Box<T> {  // 지네릭 타입 T를 선언
    T item;
    void setItem(T item) {  this.item = item;  }
    T getItem() {  return item;  }
}
```

Box<T>에서 T를 '타입변수(type variable)'라고 하며, Type의 첫 글자에서 따온 것이다. T가 아닌 다른 것을 사용해도 된다.
타입 변수가 여러 개인 경우에는 Map<K, V>와 같이 콤마 ','를 구분자로 나열하면 된다.

이들은 **기호의 종류만 다를 뿐 '임의의 참조형 타입'을 의미한다는 것은 모두 같다.**
ex) 'f(x, y) = x + y'와 'f(k, v) = k + v'와 다르지 않은 것처럼

```
Box<String> b = new Box<String>();    // 타입 T 대신, 실제 타입을 지정
b.setItem(new Object());              // 에러. String이외의 타입은 지정 불가
b.setItem("ABC");                     // 가능. String타입이므로 가능하다
String item = b.getItem();            // 형변환이 필요없음

// 타입 T 대신 String타입을 지정해줬으므로,
// 지네릭 클래스 Box<T>는 다음과 같이 정의된 것과 같다.

class Box {   // 지네릭 타입을 String타입으로 지정
    String item;
    
    void setItem(String item) {  this.item = item;  }
    String getItem() {  return item;  }
}
```

#### 지네릭스의 용어
> class Box<T> {}

- Box<T> : 지네릭 클래스. 'T의 Box' 또는 'T Box'라고 읽는다.
- T : 타입 변수 또는 타입 매개변수(T는 타입 문자)
- Box : 원시 타입(raw type)

#### 지네릭스의 제한
: 지네릭 클래스의 Box의 객체를 생성할 때, 객체별로 다른 타입을 지정하는 것은 적절하다.

```
Box<Apple> appleBox = new Box<Apple>();   // 가능. Apple객체만 저장 가능
Box<Grape> grapeBox = new Box<Grape>();   // 가능. Grape객체만 저장 가능
```

그러나 모든 객체에 대해 동일하게 동작해야하는 static멤버에 타입 변수 T를 사용할 수 없다.
T는 인스턴스변수로 간주되기 때문. static멤버는 인스턴스변수를 참조할 수 없다.

```
class Box<T> {
    static T item;    // 에러
    static int compare(T t1, T t2) {  ...  }  // 에러
}
```

static 멤버는 타입변수에 지정된 타입, 즉 대입된 타입의 종류에 관계없이 동일한 것이어야 한다.
'Box<Apple>.item'과 'Box<Grape>.item'이 다른 것이어서는 안된다.
그리고 지네릭 타입의 배열을 생성하는 것도 허용되지 않는다.
지네릭 배열 타입의 참조변수를 선언하는 것은 가능하지만, 'new T[10]'과 같이 배열을 생성하는 것은 안된다.

```
class Box<T> {
    T[] itemArr;    // 가능. T타입의 배열을 위한 참조변수
      ...
    T[] toArray() {
        T[] tmpArr = new T[itemArr.length];   // 에러. 지네릭 배열 생성불가
        ...
        return tmpArr;
    }
        ...
}
```

instanceof연산자도 new연산자와 같은 이유로 T를 피연산자로 사용할 수 없다.

꼭 지네릭 배열을 생성해야할 필요가 있을 때는, new연산자 대신 'Reflection API'의 newInstacne()와 같이
동적으로 객체를 생성하는 메서드로 배열을 생성하거나, Object배열을 생성해서 복사한 다음에 'T[]'로 형변환하는 방법 등을 사용한다.


### 1.3 지네릭 클래스의 객체 생성과 사용
: 지네릭 클래스 Box<T>가 다음과 같이 정의되어 있을때, 이 Box<T>의 객체에는 한 가지 종류, T타입의 객체만 저장할 수 있다.
전과 달리 ArrayList를 이용해서 여러 객체를 저장할 수 있도록 하였다.

```
class Box<T> {
    ArrayList<T> list = new ArrayList<T> ();
    
    void add(T item)          {  list.add(item);          }
    T get(int i)              {  return list.get(i);      }
    ArrayList<T> getList()    {  return list;             }
    int size()                {  return list.size();      }
    public String toString()  {  return list.toString();  }
}
```

Box<T>의 객체를 생성할 때는 다음과 같이 한다.
참조변수와 생성자에 대입된 타입(매개변수화된 타입)이 일치해야 한다. 일치하지 않으면 에러 발생

```
Box<Apple> appleBox = new Box<Apple> ();  // 가능
Box<Apple> appleBox = new Box<Grape> ();  // 에러
```

두 타입이 상속관계에 있어도 마찬가지이다. Apple이 Fruit의 자손이라고 가정했을때

```
Box<Fruit> appleBox = new Box<Apple> ();   // 에러. 대입된 타입이 다르다.
```

단, 두 지네릭 클래스의 타입이 상속관계에 있고, 대입된 타입이 같은 것은 괜찮다. 
FruitBox는 Box의 자손이라고 가정했을때

```
Box<Apple> appleBox = new FruitBox<Apple> ();   // 가능. 다형성
```

추정이 가능한 경우 타입을 생략할 수 있다. 
참조변수의 타입으로부터 Box가 Apple타입의 객체만 저장한다는 것을 알 수 있기 때문에,
생성자에 반복해서 타입을 지정해주지 않아도 된다.

```
Box<Apple> appleBox = new Box<Apple> ();
Box<Apple> appleBox = new Box<> ();       // 위 문장과 동일한 문장.
```

### 1.4 제한된 지네릭 클래스
: 타입 문자로 사용할 타입을 명시하면 한 종류의 타입만 저장할 수 있도록 제한할 수 있지만,
그래도 여전히 모든 종류의 타입을 지정할 수 있다는 것에는 변함이 없다.

지네릭 타입에 'extends'를 사용하면, 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.
```
FruitBox<Toy> fruitBox = new FruitBox<Toy> ();
fruitBox.add(new Toy()); 

class FruitBox<T extends Fruit> {   // Fruit의 자손만 타입으로 지정가능
    ArrayList<T> list = new ArrayList<T> ();
}

-------
FruitBox<Apple> appleBox = new FruitBox<Apple> ();  // 가능
FruitBox<Toy> toyBox = new FruitBox<Toy> ();    // 에러. Toy는 Fruit의 자손이 아님

FruitBox<Fruit> fruitBox = new FruitBox<Fruit> ();
fruitBox.add(new Apple());    // 가능. Apple이 Fruit의 자손
fruitBox.add(new Grape());    // 가능. Grape가 Fruit의 자손
```