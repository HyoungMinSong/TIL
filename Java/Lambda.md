# Lambda
>부족했던 자바8을 다시 공부하면서 정리.    
남궁성님의 자바의 정석 서적을 보고 짧게 요약 정리하였다. 

</br>

## 람다식
함수를 간단한 식으로 표현하는 방법.   

반환타입과 이름을 지운 후, 화살표(->)를 넣어서 작성한다.

익명함수라고도 한다.(사실 익명 객체)

</br>

## 람다식 작성 방법
반환타입과 이름을 지우고 화살표(->)

return문 생략 가능. 

끝에 ; 안 붙임.


매개변수의 타입이 추론이 가능하면 타입은 생략 가능하다. (보통 제네릭스를 통해 추론)

</br>

## 주의사항
매개변수가 하나인 경우 괄호() 생략 가능.

블록 안의 문장이 하나뿐 일 때, 괄호{} 생략가능. 

</br>

## 함수형 인터페이스
 단 하나의 추상 메서드만 선언된 인터페이스  

람다식(익명 객체)을 다루기 위한 참조변수의 타입은 함수형 인터페이스로 한다.

람다식을 참조변수로 다룰 수 있다는 것은 메서드를 통해 람다식을 주고받을 수 있다는 것을 의미.

```java
///ex)
@FunctionalInterface ///함수형 인터페이스 어노테이션
public interface Comparator<T> {
    int	compare(T o1, T o2)
}
```

</br>

## java.util.function패키지
### 자바에서 제공하는, 자주 사용되는 다양한 함수형 인터페이스를 제공.
* Runnable : 매개변수 x 반환값 x 
* >[ void run() ]
* Supplier<T> : 매개변수 x 반환값 o (공급자)
* > [ T get() -> T ] 
* Consumer<T> : 매개변수 o 반환값 x (소비자)
* > [ T -> void accept(T t) ]
* Function<T, R> : 매개변수 o 반환값 o (일반적인 함수)
* >[ T -> R apply(T t) -> R]
* Predicate<T> : 매개변수 o 반환  (조건식) 타입 boolean 
* >[ T -> boolean test(T t) -> boolean ]

### Bi 매개변수가 두개인 함수형 인터페이스
* BiConsumer<T,U> : 두 개의 매개변수 반환값 x 
* >[ T,U -> void accept(T t, U u) ]
* BiPredicate<T,U> :두 개의 매개변수 둘, 반환값 boolean 
* >[ T,U -> boolean test(T t, U u) -> boolean ]
* BiFunction<T,U,R> : 두 개의 매개변수, 하나의 반환값
* > [ T,U -> R apply(T t, U u) -> R ]

### 매개변수 타입과 반환타입 일치하는 함수형 인터페이스
* UnaryOperator<T> : Function의 자손, Function과 달리 매개변수의 결과의 타입이 같음 
* > [ T -> T apply(T t) -> T ]
* BinaryOperator<T> : BiFunction의 자손, BiFunction과 달리 매개변수와 결과의 타입이 같음  (입력이 두개)
* >[ T, T -> T apply(T t, T t) -> T ] 

</br>

## 함수형인터페이스를 사용하는 컬렉션 프레임웍
|인터페이스|메서드|설명
|:---:|:---:|:---:|
|Collection|boolean removeIf(Predicate<T> filter)| 조건의 element 제거
|List|void replaceAll(UnaryOperator<T> operator)|모든 element를 변환하여 대체
|Iterable|void forEach(Consumer<T> action)|모든 element에 대해 action 수행
|Map| 	V compute(K key, BiFunction<K, V, V> f)|지정된 key에 대해 f 수행
|Map|V computeIfAbsent(K key, Function<K, V> f)|key가 없을 경우 f 수행 후 추가
|Map|V computeIfPresent(K key, BiFunction<K, V, V> f)|지정된 key가 있을 때 f 수행
|Map|V merge(K key, V value, BiFunction<V, V, V> f)|모든 element에 대해 f 병합작업 수행
|Map|void forEach(BiConsumer<K, V> action)|모든 element에 대해 작업 action 수행
|Map|void replaceAll(BiFunction<K, V, V> f)|모든 element에 대해 치환작업 f 수행

</br>



## 메서드 참조
람다식을 더 간단히 작성할 수 있다. 

클래스이름 :: 메서드  (특정 객체 인스턴스메서드 참조는 잘 안써서 생략.)

이렇게 쓸 수 있는 이유는 함수형 인터페이스에 (입력,출력)정보가 있기 때문이다. 

```java
Function<String, Integer> s = (String s) -> Integer.parseInt(s);

Function<String, Integer> s = Integer :: parseInt
```
