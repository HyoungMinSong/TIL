# Comparator와 Comparable
> \-

</br>

## Comparator와 Comparable
* 객체 정렬에 필요한 메서드(정렬기준 제공)를 정의한 인터페이스

</br>

### Comparable
기본 정렬기준을 구현하는데 사용.
```java
Interface Comparable<T> {
	int	compareTo(T o) // 주어진 객체를 자신과 비교
}
```
### Comparator
기본 정렬기준 외에 다른 기준으로 정렬하고자할 때 사용
```java
Interface Comparator<T> {
	int	compare(T o1, T o2) //o1, o2 두 객체를 비교
}
```
#### compareTo()와 compare()는 두 객체의 비교결과를 반환하도록 작성.
 * 같으면 0, 오른쪽이 크면 음수(-), 작으면 양수
>  오름차순을 기준으로 양수면 자리 바꾸기(왼쪽 객체가 큼), 음수(오른쪽 객체가 큼) 혹은 0(같음)이면 자리를 바꾸지 않는다. 

</br>

### 정렬은 ①두 대상을 비교하고 ②자리를 바꾸는 로직을 가지고 있다
* ```Arrays.sort()```에 하나의 객체 배열을 넣는 경우 ex) ```sort(String[] a)``` 객체 배열에 저장된 Comparble에 의한 정렬이 일어난다.
* ```Arrays.sort()```에 객체 배열과 Comparator를 넣는 경우 ex) ```sort(String[] a, Compartor c)``` Comparator에 의한 정렬이 일어난다.

</br>

#### String이나 Integer, Float 와 같은 비교가 가능한 클래스는 이미 Comparable 기본 정렬 기준이 제공된다.(인터페이스 구현) 

