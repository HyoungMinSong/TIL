# Queue & Stack
> 스택과 큐는 배열에서 발전된 형태의 자료구조.

</br>

## Queue
* 삽입과 삭제 연산이 선입선출(FIFO)로 이뤄지는 자료구조. 먼저 들어온 데이터가 먼저 나간다. 따라서 삽입과 삭제가 양방향에서 이루어짐.

</br>

### 큐 구현
Queue 인터페이스를 직간접적으로 구현한 클래스는 상당히 많다. 보통 LinkedList 클래스를 통해 구현.
```java
import java.util.LinkedList; 
import java.util.Queue; 
Queue<Integer> queue = new LinkedList<>();
```
### 큐 주요 함수
|메서드|설명
|:---:|:---:|
|boolean add(E e)|해당 큐의 맨 뒤에 전달된 요소를 삽입. 만약 삽입 성공시 true를 반환, 큐에 여유 공간이 없어 삽입에 실패하면 IllegalStateException을 발생.
|E peek()|해당 큐의 맨 앞에 있는(제일 먼저 저장된) 요소를 반환. 큐가 비어있으면 null을 반환.
|E poll()|해당 큐의 맨 앞에 있는(제일 먼저 저장된) 요소를 반환하고, 해당 요소를 큐에서 제거. 만약 큐가 비어있으면 null을 반환.

</br>

## PriorityQueue
 * 우선순위 큐는 값이 들어간 순서와 상관 없이 우선순위가 높은 데이터가 먼저 나오는 자료구조. 
 * 큐 설정에 따라 front에 항상 최댓값 또는 최솟값이 위치. 
 * 일반적으로 힙을 이용해 구현.

</br>

### 우선순위 큐 구현
```java
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(); ///오름차순 우선순위

PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());	///매개변수에 Comprator를 넣으면 해당 정렬기준으로 우선순위가 결정된다. (현재 내림차순 우선순위)

PriorityQueue<Integer> pq = new PriorityQueue<>((o1, o2) -> {
    int a = Math.abs(o1);
    int b = Math.abs(o2);
    if (a == b)
        return o1>o2 ? 1: -1;
    else
        return a - b;
}); ///이런식으로 람다를 사용해서 커스텀 정렬기준 사용

```

### 우선순위 큐 주요함수
 * add(value) - 새로운 데이터 우선순위 큐에 더하기
 * poll() - 우선순위가 높은 데이터 반환, 큐에서 제거

</br>

## Stack
* 삽입과 삭제 연산이 후입선출(LIFO)로 이뤄지는 자료구조.
* 후입선출은 삽입과 삭제가 한 쪽에서만 일어나는 특징이 있다.

</br>

### 스택 구현
```java
import java.util.Stack;
Stack<Integer> stack = new Stack<>();
```
### 스택 주요 함수
|메서드|설명
|:---:|:---:|
|E push(E item)|해당 스택의 제일 상단에 전달된 요소를 삽입.
|E pop()|해당 스택의 제일 상단에 있는(제일 마지막으로 저장된) 요소를 반환하고, 해당 요소를 스택에서 제거.
|E peek()|해당 스택의 제일 상단에 있는(제일 마지막으로 저장된) 요소를 반환.

</br>

