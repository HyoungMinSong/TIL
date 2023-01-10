# String Class
> 까먹을 만한 것 지속적으로 추가.

</br>

## split 메소드
* split 함수는 정규표현식 또는 특정 문자를 기준으로 문자열을 나누어 배열 에 저장하여 리턴. 

### split(String regex)
* split(String regex, int limit)
  * 구분자를 바탕으로 배열 형식으로 문자열을 자르지만 limit 수만큼 잘라준다.(최대 배열 크기)
 ```java
	String str = "가나다라-1234-abcd";
	String[] split = str.split("-");
	String a = split[0];
	String b = split[1];
	String c = split[2];

	System.out.println("a = "+a);
	System.out.println("b = "+b);
	System.out.println("c = "+c);

```
```
a = 가나다라
b = 1234
c = abcd
```

### 구분자를 하나가 아닌 여러 개를 사용하여 분리도 가능하다.
구분자의 사이에 | 를 사용하여 구분자|구분자|구분자 이렇게 여러 개의 구분자를 사용이 가능하다.

### 주의해야할 점
*  정규식 사용 시  ```String.split("\\.");```, ```String.split("\\s");``` 등 ```\```을 붙이며 사용.
*  속도를 더 빠르게 사용해야할 경우 StringTokenizer 사용. 


</br>

## indexOf 메소드
특정 문자나 문자열이 앞에서부터 처음 발견되는 인덱스를 반환. 만약 찾지 못했을 경우 -1을 반환.

### indexOf(String s)
* indexOf(int i)
* indexOf(String s, int startindex)  
	- int startindex는 시작할 위치
* indexOf(int i, int startindex)

첫 번째 매개변수 int 는 아스키 코드이다. 아스키코드로도 찾을 수 있다. 
```java
	String a = "hello world";
	System.out.println(a.indexOf("h")); //0
	System.out.println(a.indexOf("a")); //-1
	System.out.println(a.indexOf("o", 5)); //(5부터 시작) 7
``` 

### 주의해야할 점
 * 뒤에서 부터 찾는 lastIndexOf()도 있다. 뒤에서 부터 찾지만 인덱스 값은 왼쪽	부터 몇번째 위치하는지의 값이다.


<br>

## replaceAll 메소드
대상 문자열을 원하는 문자 값으로 변환하는 함수

### String replaceAll(String regex, String replacement)
 * String replace(CharSequence target, CharSequence replacement)
	- 사실상 replaceAll 메소드와 같다고 볼 수 있다. 하지만 replaceAll은 정규표현식을 쓸 수 있다.
 * String replaceFirst​(String regex, String replacement)
	- 처음으로 만난 기존 문자만 변경되고 나머지는 그대로이다.

```java
	String a = "hello hello world";
	System.out.println(a.replaceAll("hello", "hi")); //hi hi world
	System.out.println(a.replaceFirst("hello", "hi")); //hi hello world
```

<br>

## trim 메소드
문자열의 앞과 뒤의 공백을 제거

### String trim()
```java
	String a = "   hello hello world   ";
	System.out.println(a.trim()); //hello hello world
```

## substring 메소드
문자열 구간(부분)을 자르는데 사용.

### String substring(int startIndex)
  - startIndex(시작점)부터 끝까지의 문자열을 리턴.
  - String substring(int startIndex, int endIndex)
    - startIndex(시작점)부터 endIndex(불포함) 전까지의 문자열을 리턴.
```java
	String a = "hello world";
	System.out.println(a.substring(6)); //world
	System.out.println(a.substring(0,4)); //hell
```

