# Thymeleaf
>토이프로젝트에 타임리프를 적용하면서 배웠던 내용 정리.
김영한님의 인프런 MVC강의를 보고 짧게 요약 정리하였다.

</br>

## 타임리프
타임리프는 순수 HTML을 최대한 유지하는 네츄럴 템플릿이다.  

</br>

## 타임리프 사용 선언
```html
<html xmlns:th="http://www.thymeleaf.org">
```

</br>

## 텍스트
HTML 태그의 속성에 th:text 넣어 사용한다. 
```html
<p th:text="${test}">
```
태그 바깥에다가 바로 넣고 싶으면
```[[${test}]]``` 를 넣으면 된다.

기본적으로 제공하는 이스케이프 기능을 사용하기 싫으면
```html
<p th:utext="${test}"> 
```
```[(${test})]``` 이렇게 사용하면 된다.

</br>

## 변수 표현식
타임리프에서 변수를 사용할 때 아래의 변수표현식을 사용하면 된다.

```${...}```

이 변수 표현식은 스프링 EL 표현식도 지원한다. (JSP에서 많이 쓰던)

</br>

## 지역 변수
th:with를 사용하여 타임리프 안에서 지역변수를 선언할 수 있다.
```html
<p th:with="test=${users[0]}">
```

## 타임리프 기본 객체와 편의 객체
### 타임리프 기본객체
* ${#request}
* ${#response} 
* ${#session}
* ${#servletContext}
* ${#locale}

### 편의객체
* HTTP 요청 파라미터 접근: param
* > ex) ${param.paramData}
* HTTP 세션 접근: session
* > ex) ${session.SessionData}
* 스프링 빈 접근: @
* > ex) ${@helloBean.hello('Spring!')}
</br>

## URL 링크
타임리프에서 URL을 생성할 때는 @{...}문법을 사용하면 된다.

### 단순한 url
>@{/hello} -> /hello

### 쿼리 파라미터
>@{/hello(param1=${param1}, param2=${param2})} 

-> /hello?param1=data&param2=data2
()에 있는 부분은 쿼리 파라미터로 처리된다.

### 경로변수
>@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}

 -> /hello/data1/data2
URL 경로상에 변수가 있으면 () 부분은 경로 변수로 처리된다.

### 경로 변수 + 쿼리 파라미터
>@{/hello/{param1}(param1=${param1}, param2=${param2})} -> /hello/data1?param2=data2

경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.

</br>

## 타임리프에서 리터럴 사용시 주의사항
타임리프에서 문자 리터럴은 항상 ' (작은 따옴표)로 감싸야 한다.
```html
<span th:text="'hello'">
```
그런데 문자를 항상 ' 로 감싸는 것은 너무 귀찮은 일이다. 공백 없이 쭉 이어진다면 하나의 의미있는
토큰으로 인지해서 다음과 같이 작은 따옴표를 생략할 수 있다.

```
A-Z , a-z , 0-9 , [] , . , - , _
```
```html
<span th:text="hello">
```
### 오류
```html
<span th:text="hello world!"></span>
```
문자 리터럴은 원칙상 ' 로 감싸야 한다. 중간에 공백이 있어서 하나의 의미있는 토큰으로도 인식되지
않는다.

### 수정
```html
<span th:text="'hello world!'"></span>
```
### 리터럴 대체
```html
<span th:text="|hello ${data}|">
```
리터럴 대체 문법을 사용하면 마치 템플릿을 사용하는 것 처럼 편리하다.

</br>

## 반복
타임리프에서 반복은 th:each 를 사용한다. 추가로 반복에서 사용할 수 있는 여러 상태 값을 지원한다.

### 반복 기능
```html
<tr th:each="user : ${users}">
```
반복시 오른쪽 컬렉션( ${users} )의 값을 하나씩 꺼내서 왼쪽 변수( user )에 담아서 태그를 반복
실행한다.

th:each 는 List 뿐만 아니라 배열, java.util.Iterable , java.util.Enumeration 을 구현한 모든
객체를 반복에 사용할 수 있습니다. Map 도 사용할 수 있는데 이 경우 변수에 담기는 값은 Map.Entry
이다.

### 반복 상태 유지
```html
<tr th:each="user, userStat : ${users}">
```
반복의 두번째 파라미터를 설정해서 반복의 상태를 확인 할 수 있다.
두번째 파라미터는 생략 가능한데, 생략하면 지정한 변수명( user ) + Stat 가 된다.

### 반복 상태 유지 기능
* index : 0부터 시작하는 값
* count : 1부터 시작하는 값
* size : 전체 사이즈
* even , odd : 홀수, 짝수 여부( boolean )
* first , last :처음, 마지막 여부( boolean )
* current : 현재 객체

</br>

## 연산
### 비교연산
HTML 엔티티를 사용해야 하는 부분을 주의
```
> (gt), < (lt), >= (ge), <= (le), ! (not), == (eq), != (neq, ne)
```
```html
<li>1 gt 10 = <span th:text="1 gt 10"></span></li>
```
### 조건식
```html
<li>(10 % 2 == 0)? '짝수':'홀수' = <span th:text="(10 % 2 == 0)?
'짝수':'홀수'"></span></li>
```
### Elvis 연산자
조건식의 편의 버전
```html
<li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?:
'데이터가 없습니다.'"></span></li>
```
### No-Operation
_ 인 경우 마치 타임리프가 실행되지 않는 것 처럼 동작한다. 이것을 잘 사용하면 HTML
의 내용 그대로 활용할 수 있다
```html
<li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가
없습니다.</span></li>
```

</br>

## 조건부 평가
### if, unless
타임리프는 해당 조건이 맞지 않으면 태그 자체를 렌더링하지 않는다.
만약 다음 조건이 false 인 경우 <span>...<span> 부분 자체가 렌더링 되지 않고 사라진다.
```html
<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>
```
### switch
```html
<td th:switch="${user.age}">
 <span th:case="10">10살</span>
 <span th:case="20">20살</span>
 <span th:case="*">기타</span>
</td>
 ```
*은 만족하는 조건이 없을 때 사용하는 디폴트이다.

</br>

## 템플릿 조각
```html
<footer th:fragment="copy">
 푸터 자리 입니다.
</footer>
<footer th:fragment="copyParam (param1, param2)">
 <p>파라미터 자리 입니다.</p>
 <p th:text="${param1}"></p>
 <p th:text="${param2}"></p>
</footer>
```
template/fragment/footer :: copy 는 template/fragment/footer.html 템플릿에 있는
th:fragment="copy" 라는 부분을 템플릿 조각으로 가져와서 사용한다는 의미이다.
```html
<div th:insert="~{template/fragment/footer :: copy}"></div>
```
th:insert 를 사용하면 현재 태그( div ) 내부에 추가한다.
```html
<div th:replace="~{template/fragment/footer :: copy}"></div>
```
th:replace 를 사용하면 현재 태그( div )를 대체한다.
```html
<div th:replace="~{template/fragment/footer :: copyParam ('데이터1', '데이터
2')}"></div>
```
다음과 같이 파라미터를 전달해서 동적으로 조각을 렌더링 할 수도 있다.

</br>

## 템플릿 레이아웃
```html
<head th:fragment="common_header(title,links)">
 <title th:replace="${title}">레이아웃 타이틀</title>
 <!-- 공통 -->
 <link rel="stylesheet" type="text/css" media="all" th:href="@{/css/
awesomeapp.css}">
 <link rel="shortcut icon" th:href="@{/images/favicon.ico}">
 <script type="text/javascript" th:src="@{/sh/scripts/codebase.js}"></
script>
 <!-- 추가 -->
 <th:block th:replace="${links}" />
</head>
```

```html
<head th:replace="template/layout/base :: common_header(~{::title},~{::link})">
 <title>메인 타이틀</title>
 <link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
 <link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">
</head>
```
이 방식은 사실 앞서 배운 코드 조각을 조금 더 적극적으로 사용하는 방식이다. 쉽게 이야기해서 레이아웃
개념을 두고, 그 레이아웃에 필요한 코드 조각을 전달해서 완성하는 것으로 이해하면 된다.


앞서 이야기한 개념을 ```<head>``` 정도에만 적용하는게 아니라 ```<html>``` 전체에 적용할 수도 있다.
