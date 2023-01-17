# List와 Arraylist,Arrays
> .

## Arrays
배열을 다루기 편리한 Static 메소드 제공.
### asList()
* 배열을 List로 변환
### toString(), deepToString()
 * 배열 요소 출력
 * 다차원 배열일 경우 deepToString 사용
### equals(), deepEquals()
* 배열 비교
* 다차원 배열일 경우 deepEquals 사용
### copyOf(), copyOfRange()
* 배열 복사
### fill()
* 배열 채우기
### sort(), sort([], Comparator)
* 배열 정렬
### binarySearch()
* 배열 검색
* 사용 전 배열이 정렬되있어야 한다.

</br>

## String to char[] / char[] to String
```java
	String s = "Hello World";
	char[] chars = s.toCharArray();  //String to char[]

    String s1 = new String(chars); // char[] to String
```

</br>

## Array to List

### Arrays.asList(배열 혹은 단일객체);
* ```Arrays.asList()```는 Arrays의 private static 클래스인 ArrayList를 리턴한다.
즉, ```java.util.ArrayList``` 클래스와는 다른 클래스이다.
* ```java.util.Arrays.ArrayList``` 클래스는 읽기(변경) 전용 클래스이다.
  * set(), get(), contain() 메소드가 있다.
* ```Arrays.asList(배열)```는 새로운 배열 객체를 만드는 것이 아니라, 원본 배열의 주소값을 가져오기 때문에 내용을 수정하면 원본 배열도 함께 바뀌게 되고, 원본 배열을 수정하면 그 배열로 만들어뒀던 List 내용도 바뀌게 된다.
#### 주의점
* asList()를 사용할 때 기본형 타입 배열을 사용할 수 없다.
```java
        // int 배열
        int[] arr = { 1, 2, 3 };
 
        // Arrays.asList() 
        List<int[]> intList = Arrays.asList(arr);
```
이런식으로 int 배열 요소 하나 가지고 있는 List가 된다.
#### 해결방법 
```java
List<Integer> list = Arrays.asList(1,2,3);  
//혹은
Integer[] arr = new Integer[]{1, 2, 3};  
Arrays.asList(arr);
//혹은 for문을 통해 바로 ArrayList로 받기
int[] arr = { 1, 2, 3 };

List<Integer> list1 = new ArrayList<>();
for (int a : arr) {
list1.add(element);
}
//혹은 stream을 통해 받기
List<Integer> list2 
        = Arrays.stream(arr)
                .boxed() // Primitive Stream 값들을 Wrapper Class로 바꿔줌.
                .collect(Collectors.toList());
```

### ArrayList
ArrayList는 Collection 프레임워크의 일부이며 List 인터페이스에서 상속받아 사용이 된다.
### 생성자 
* ArrayList() - 크기가 10인 ArrayList를 생성
* ArrayList(Collection c) - 주어진 컬렉션이 저장된 ArrayList를 생성
```java
ArrayList<Integer> a = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4));
```
* ArrayList(int initialCapacity) - 지정된 초기용량을 갖는 ArrayList생성
### add(Object) 
* ArrayList의 마지막에 데이터를 추가
### add(int index, Object) 
* ArrayList의 index에 데이터를 추가
### set(int index, Object)
* 값 변경
### remove(Object)
* 해당 ArrayList의 Object와 같은 값을 삭제
### remove(int index)
* ArrayList의 index에 해당하는 값을 삭제
### size()
* 크기 구하기
### get(int Index)
* 값 구하기
* 향상된 포문을 사용하면 get 생략 가능.
```java
for (String s : arraylist)
            System.out.print(s);
```
### contains(object)
* 값 여부 확인
### indexOf(object)
* 값의 인덱스를 반환. 없으면 -1 출력.
