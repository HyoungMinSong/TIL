# Recursion
> \-

</br>

## 재귀함수
* 자신이 자신을 호출하는 함수이다. 동적 프로그래밍 알고리즘에 기본이 된다.
* 일반적으로 반복문보다 느리다. 따라서 몇몇 유리한 문제 빼고는 반복문으로 풀 수 있으면 반복문으로 푸는게 좋다.
* 하지만 재귀함수를 쓰면 반복문을 쓰는 것보다 깔끔하게 코드를 작성할 수 있다.

</br>

### 주의사항
* 종료조건(기저사례)를 작성해야 한다.
* 문제의 정의를 작성해야 한다.
* 사이클이 있다면 쓰면 안된다.
  
```java
public class Main {
 
    public static void main(String[] args) {
 
        int result = factorial(5);
        System.out.println(result);
    }
    
    public static int factorial (int num) {
        if (num == 1)
            return 1; //종료조건을 작성한다.num이 1일 경우 num!= 1이다.
        
        return num * factorial ( num - 1 ); // 문제의 정의 작성한다.
        //팩토리얼의 수학적 정의 
    }
}

```
### 주요 문제
* 팩토리얼 ```num * factorial ( num - 1 )```
* 피보나치의 수열 ```fibo(n-1) + fibo(n-2)```
* 하노이탑

