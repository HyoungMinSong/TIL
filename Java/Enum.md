# Enum
>남궁성님의 자바의 정석과 생활코딩을 보고 짧게 요약 정리하였다. 

</br>

## enum
enum은 열거형(enumerated type)이라고 부른다. 열거형은 서로 연관된 상수들의 집합이라고 할 수 있다. 
```java
enum Fruit{
APPLE, PEACH, BANANA;
}

```

하지만 enum은 사실상 class이다. 편의를 위해서 enum만을 위한 문법적 형식을 가지고 있기 때문에 구분하기 위해서 enum이라는 키워드를 사용하는 것이다. 위의 코드는 아래 코드와 사실상 같다.
```java
class Fruit{
public static final Fruit APPLE = new Fruit();
public static final Fruit PEACH = new Fruit();
public static final Fruit BANANA = new Fruit();
private Fruit(){}
}
```

</br>

## 열거형의 사용
열거형이름.상수명이다. 클래스의 스태틱 변수를 참조하는 것과 동일하다.

</br>

## 열거형의 조상 - java.lang.Enum
|메서드|설명|
|:---:|:---:|
|String name( )|열거형 상수의 이름을 문자열로 반환함.
|int ordinal()|열거형 상수가 정의된 순서를 반환한다.(0부터 시작)
|T valueOf (Class<T> enumType, String name)|지정된 열거형에서 name과 일치하는 열거형 상수를 반환한다.
|Class<E> getDeclaringClass()|열거형의 Class 객체를 반환한다.

</br>

## 그 외 컴파일러가 자동적으로 추가해주는 메서드
|메서드|설명|
|:---:|:---:|
|static E[] values( )|열거형의 모든 상수를 배열에 담아 반환한다.
|static E valueOf(String name)|열거형 상수의 이름으로 문자열 상수에 대한 참조를 얻을 수 있게 한다.

</br>

## 열거형에 멤버 추가하기
```java
enum Direction {
    EAST(1, ">"), SOUTH(5, "V"), WEST(-1, "<"), NORTH(10, "^"); //생성자를 통한 필드에 값을 추가한다고 보면 된다.
    
    private final int value; // 정수를 저장할 필드(인스턴스 변수) 추가
    private final String symbol;
    Direction(int value, String symbol) { // 당연히 생성자 추가해야함.
        this.value = value;
        this.symbol = symbol;
    }
    
    public int getValue() {return value;}
    public String getSymbol() {return symbol;}

```
