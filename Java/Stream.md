# Stream
>스트림을  토이프로젝트에 적용하면서 공부한 내용 정리. 
남궁성님의 자바의 정석을 보고 짧게 요약 정리하였다. 

</br>

## 스트림
다양한 데이터 소스(컬력션,  배열)를 표준화된 방법(스트림을 만들 수 있다)으로 다루기 위한 것이다.  
* 스트림 만들기
* 중간연산 (0 ~ n번) 
* >연산결과가 스트림인 연산. 반복적으로 적용 가능
* 최종연산 (0 ~ 1번)
* >연산결과가 스트림이 아닌 연산. 스트림의 연소를 소모하기 때문에 단 한번만 적용 가능.

</br>

## 스트림의 특징
* 스트림은 데이터 소스(원본)으로부터 데이터를 읽기만 할 뿐 변경하지 않는다.
* 스트림은 iterator처럼 일회용이다.(필요하면 다시 스트림을 생성해야 한다.)
* 최종 연산 전까지 중간 연산이 수행되지 않는다. - 지연된 연산 
* 스트림은 작업을 내부 반복으로 처리한다.
* 스트림의 작업을 병렬로 처리할 수 있다. - 병렬스트림(멀티쓰레드)
* 오토박싱과 언박싱의 비효율을 제거하려 기본형 스트림을 제공한다. (IntStream, LongStream…)

</br>

## 스트림 만들기
### 컬렉션
Collection인터페이스(List, Set)에 ```stream()```으로 컬렉션을 스트림으로 변환 가능
### 배열
```
Stream<T> Stream.of(가변인자 혹은 배열)
Stream<T> Arrays.stream(배열 혹은 배열과 범위)
```
### 무한 스트림(난수)
Random클래스를 이용하여 무한 스트림을 만들 수 있다.
```java
IntStream intStream = new Random().ints();
```
따라서 중간연산의 ```limit()```등 을 사용하여 크기를 제한해 주어야 한다.

아니면 매개변수를 주어 유한스트림으로 생성할 수도 있다.
```java
IntStream intStream = new Random().ints(5);
```
그 외에 범위지정도 가능하다.
### 특정 범위의 정수
```java
IntStream intStream = IntStream.range(1,5) //1,2,3,4
IntStream intStream = IntStream.rangeClosed(1,5) //1,2,3,4,5
```
### 람다식
```java
static <T> Stream<T> iterate(T seed, UnaryOperator<T> f ) //초기값 필요, 이전 요소에 종속적
static <T> Stream<T> generate(Supplier <T> s) //이전 요소에 독립적
```

</br>

## 스트림의 중간연산
~~표~~

</br>

## 스트림의 최종연산
~~표~~

</br>

## Optional\<T>
T 타입 객체의 래퍼 클래스. 모든 종류의 객체 저장 가능하다.(null 포함) 결국, null을 간접적으로 다루기 위한 것이다.

</br>

## 옵셔널 객체 생성하기
```java
Optional<String> optVal = Optional.of("abc");
Optional<String> optVal = Optional.of(null); //NullPointerException발생
Optional<String> optVal = Optional.ofNullable(null);
Optional<String> optVal = Optional. empty(); //옵셔널 빈객체로 초기화
```

</br>

## 옵셔널 객체의 값 가져오기
```java
String str1 = optVal.get(); //널이면 예외발생
String str2 = optVal.orElse(""); //널일 때는 “” 반환
String str3 = optVal.orElseGet(String::new); //람다식 사용가능 (Supplier)
String str4 = optVal.orElseThrow(NullPointerException::new); //널이면 예외종류 지정가능
```
```isPresent()``` - Optional 객체의 값이 null이면 false, 아니면 true를 반환한다.
```java
if (Optional.ofNullable(str).isPresent()) { System.out.println(str); }
```
```ifPresent(Consumer)``` - null이 아닐 때만 작업 수행, null이면 아무 일도 일어나지 않는다.
```java
Optional.ofNullable(str).ifPresent(System.out::println);
```
```OptionalInt```와 같은 기본형 값을 감싸는 래퍼 클래스 또한 있다.

</br>

## 직렬스트림 수행
```sequential()``` - 디폴트값이라 생략 가능하다.

</br>

## 병렬스트림 수행
```parallel()``` - 순서가 보장되지 않아 순서 보장을 원하면 순서가 보장되는 최종 연산을 사용해야 한다.

</br>

## collect
```collect()```는 ```Collector```를 매겨변수로 하는 스트림의 최종 연산이다.

</br>

## Collector
```Collector```는 ```collect```에 필요한 메서드를 정의해 놓은 인터페이스이다.

</br>

## Collectors
```Collectors```는 ```Collector```를 구현한 클래스이다. (다양한 기능 제공)

</br>

## 스트림을 컬렉션으로 변환
```java
List<String> names = stuStream.map(Student::getName).collect(Collectors.toList());

ArrayList<String> list = names.stream().collect(Collectors.toCollection(ArrayList::new));

Map<String, Person> map = personStream.collect(Collectors.toMap(p->p.getRegId(), p->p));
```

</br>

## 스트림을 배열로 변환
```java
Student[] stuNames = studentStream.toArray(Student[]::new);
Object[] stuNames = studentStream.toArray();
```

</br>

## Collectors의 메소드
* 통계 - ```counting()```, ```summingint()```
* 리듀싱 - ```reducing()```
* 문자열로 결합 - ```joining()```
* 2분할 - ```partitioningBy()```
* n분할 - ```groupingBy()```
