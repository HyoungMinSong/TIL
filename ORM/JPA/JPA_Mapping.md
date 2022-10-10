# JPA - Mapping
>김영한님의 인프런 강의를 보고 요약 정리하였다. 

</br>

## 엔티티 매핑
* 객체와 테이블 매핑: @Entity, @Table
* 필드와 컬럼 매핑: @Column 
* 기본 키 매핑: @Id 
* 연관관계 매핑: @ManyToOne,@JoinColumn
### 객체와 테이블 매핑
@Entity가 붙은 클래스는 JPA가 관리, 엔티티라 한다.
* JPA를 사용해서 테이블과 매핑할 클래스는 @Entity 필수 
* 주의 
  * 기본 생성자 필수(파라미터가 없는 public 또는 protected 생성자)
  * final 클래스, enum, interface, inner 클래스 사용 X 
  * 저장할 필드에 final 사용 X
### 그 외 필드와 컬럼 매핑
#### @Enumerated
enum 타입 매핑
|속성|설명|기본값
|:---:|:---:|:---:
|value|EnumType.ORDINAL: enum 순서를 데이터베이스에 저장,  EnumType.STRING: enum 이름을 데이터베이스에 저장|EnumType.ORDINAL
#### @Temporal
날짜 타입 매핑
>  LocalDate, LocalDateTime을 사용할 때는 생략 가능(최신 하이버네이트 지원)

|속성|설명|기본값
|:---:|:---:|:---:
|value|TemporalType.DATE: 날짜, 데이터베이스 date 타입과 매핑 (예: 2013–10–11), TemporalType.TIME: 시간, 데이터베이스 time 타입과 매핑 (예: 11:11:11), TemporalType.TIMESTAMP: 날짜와 시간, 데이터베이스 timestamp 타입과 매핑(예: 2013–10–11 11:11:11) 
#### @Lob
BLOB, CLOB 매핑
* 매핑하는 필드 타입이 문자면 CLOB 매핑, 나머지는 BLOB 매핑
  * CLOB: String, char[], java.sql.CLOB 
  * BLOB: byte[], java.sql. BLOB
#### @Transient
특정 필드를 컬럼에 매핑하지 않음(매핑 무시)
* 필드 매핑X 
* 데이터베이스에 저장X, 조회X 
* 주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용
### 기본 키 매핑
#### 기본 키 매핑 방법
* 직접 할당: @Id만 사용 
* 자동 생성(@GeneratedValue) 
  * IDENTITY: 데이터베이스에 위임, MYSQL
  * SEQUENCE: 데이터베이스 시퀀스 오브젝트 사용, ORACLE
    * @SequenceGenerator 필요
  * TABLE: 키 생성용 테이블 사용, 모든 DB에서 사용 
    * @TableGenerator 필요 
  * AUTO: 방언에 따라 자동 지정, 기본값
#### IDENTITY 전략 - 특징
기본 키 생성을 데이터베이스에 위임 
* 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용 (예: MySQL의 AUTO_ INCREMENT) 
* JPA는 보통 트랜잭션 커밋 시점에 INSERT SQL 실행
* AUTO_ INCREMENT는 데이터베이스에 INSERT SQL을 실행 한 이후에 ID 값을 알 수 있음 
* IDENTITY 전략은 em.persist() 시점에 즉시 INSERT SQL 실행 하고 DB에서 식별자를 조회
#### SEQUENCE 전략 - 특징
데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트(예: 오라클 시퀀스) 
* 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용

</br>

## 데이터베이스 스키마 자동 생성
### hibernate.hbm2ddl.auto
|옵션|설명|
|:---:|:---:|
|create|기존테이블 삭제 후 다시 생성 (DROP + CREATE)
|create-drop|create와 같으나 종료시점에 테이블 DROP update 변경분만 반영(운영DB에는 사용하면 안됨)
|validate|엔티티와 테이블이 정상 매핑되었는지만 확인
|none|사용하지 않음

</br>

## 연관관계 매핑 기초
객체를 테이블에 맞추어 데이터 중심으로 모델링하면, 협력 관계를 만들 수 없다. 
* 테이블은 외래 키로 조인을 사용해서 연관된 테이블을 찾는다.
* 객체는 참조를 사용해서 연관된 객체를 찾는다. 
* 테이블과 객체 사이에는 이런 큰 간격이 있다.
### 단방향 연관관계
```java
@Entity 
public class Member { 
@Id @GeneratedValue private Long id;

@Column(name = "USERNAME") 
private String name; 

private int age; 

@ManyToOne @JoinColumn(name = "TEAM_ID") 
private Team team;
```
```java
//팀 저장 
Team team = new Team();
team.setName("TeamA"); em.persist(team); //회원 저장

Member member = new Member(); 
member.setName("member1"); 
member.setTeam(team); //단방향 연관관계 설정, 참조 저장
em.persist(member);
```
### 양방향 연관관계
```java
@Entity 
public class Member { 

@Id @GeneratedValue 
private Long id; 

@Column(name = "USERNAME") private String name;
private int age; 

@ManyToOne @JoinColumn(name = "TEAM_ID") 
private Team team;
…}
```
Member 엔티티는 단방향과 동일
```java
@Entity 
public class Team { 

@Id @GeneratedValue
private Long id;

private String name; 

@OneToMany(mappedBy = "team") 
List members = new ArrayList();
 … }
```
Team 엔티티는 컬렉션 추가

관례로 ArratList로 초기화한다. (add할때 널포인트가 안뜬다.)
```java
//조회 
Team findTeam = em.find(Team.class, team.getId());
int memberSize = findTeam.getMembers().size(); //역방향 조회
```
반대 방향으로 객체 그래프 탐색 가능
### 양방향 매핑 규칙
* 객체의 두 관계중 하나를 연관관계의 주인으로 지정 
* 연관관계의 주인만이 외래 키를 관리(등록, 수정) 
* 주인이 아닌쪽은 읽기만 가능 
* 주인은 mappedBy 속성 사용X 
* 주인이 아니면 mappedBy 속성으로 주인 지정
* 외래 키가 있는 있는 곳을 주인으로 정해라 (그래야 직관적이고 성능 이슈도 없다.)
* 여기서는 Member.team이 연관관계의 주인
### 양방향 매핑시 가장 많이 하는 실수
* 연관관계의 주인에 값을 입력하지 않음
* 양방향 매핑시 연관관계의 주인에 값을 입력해야 한다.
* 순수 객체 상태를 고려해서 항상 양쪽에 값을 설정하자 (연관관계 편의 메소드를 생성하여 하나의 메소드로 양쪽에 값을 집어넣으면 편하다.)
* 양방향 매핑시에 무한 루프를 조심하자 
  * 예: toString(), lombok, JSON 생성 라이브러리

## 다양한 연관관계 매핑
* 다대일: @ManyToOne
* 일대다: @OneToMany 
* 일대일: @OneToOne 
* 다대다: @ManyToMany
### 일대다 단반향
* 일대다 단방향은 일대다(1:N)에서 일(1)이 연관관계의 주인
* 테이블 일대다 관계는 항상 다(N) 쪽에 외래 키가 있음
* 객체와 테이블의 차이 때문에 반대편 테이블의 외래 키를 관리하는 특이한 구조 
* @JoinColumn을 꼭 사용해야 함. 그렇지 않으면 조인 테이블 방식을 사용함(중간에 테이블을 하나 추가함)
* 일대다 단방향 매핑의 단점
  * 엔티티가 관리하는 외래 키가 다른 테이블에 있음 
  * 연관관계 관리를 위해 추가로 UPDATE SQL 실행
  * 직관적이지 않고 성능 상 좋지 않으니 일대다 단방향 매핑보다는 다대일 양방향 매핑을 사용하자
### 일대다 양방향
이런 매핑은 공식적으로 존재X 
* @JoinColumn(insertable=false, updatable=false)을 통해 읽기 전용 필드를 사용해서 양방향 처럼 사용하는 방법
* 다대일 양방향을 사용하자
### 일대일 관계
* 일대일 관계는 그 반대도 일대일 
* 주 테이블이나 대상 테이블 중에 외래 키 선택 가능 
* 외래 키에 데이터베이스 유니크(UNI) 제약조건 추가
* 주 테이블에 외래 키 단방향 : 가능
* 주 테이블에 외래 키 양방향 : 다대일 양방향 매핑처럼 외래 키가 있는 곳이 연관관계의 주인. 반대편은 mappedBy 적용
* 대상 테이블에 외래 키 단방향 : 불가능
* 대상 테이블에 외래 키 양방향 : 주 테이블에 외래 키 양방향과 같다고 보면 된다(반대)
### 다대다
* 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없음 
* 연결 테이블을 추가해서 일대다, 다대일 관계로 풀어내야함
* 객체는 컬렉션을 사용해서 객체 2개로 다대다 관계 가능
* @ManyToMany 사용
* @JoinTable로 연결 테이블 지정 
* 다대다 매핑: 단방향, 양방향 가능
### 다대다 매핑의 한계 
* 편리해 보이지만 실무에서 사용X 
* 연결 테이블이 단순히 연결만 하고 끝나지 않음  (주문시간, 수량 같은 데이터가 들어올 수 있음)
* 쿼리가 이상하게 날라갈 수도 있다.
### 다대다 한계 극복
* 연결 테이블용 엔티티 추가(연결 테이블을 엔티티로 승격) 
* @ManyToMany -> @OneToMany, @ManyToOne

<br>

## 고급 매핑
### 상속관계 매핑
* 관계형 데이터베이스는 상속 관계X 
* 슈퍼타입 서브타입 관계라는 모델링 기법이 객체 상속과 유사 
* 상속관계 매핑: 객체의 상속과 구조와 DB의 슈퍼타입 서브타입 관계를 매핑
* 슈퍼타입 서브타입 논리 모델을 실제 물리 모델로 구현하는 방법 
  * 각각 테이블로 변환 -> 조인 전략 
  * 통합 테이블로 변환 -> 단일 테이블 전략
  * 서브타입 테이블로 변환 -> 구현 클래스마다 테이블 전략
### 주요 어노테이션 
* @Inheritance(strategy=InheritanceType.XXX)
  * JOINED: 조인 전략 
  * SINGLE_TABLE: 단일 테이블 전략
  * TABLE_PER_CLASS: 구현 클래스마다 테이블 전략 
* @DiscriminatorColumn(name=“DTYPE”) (DTYPE 필요시 클래스에 쓰는 어노테이션)
* @DiscriminatorValue(“XXX”) (자식클래스의 DTYPE 값을 변경하고 싶을때 쓰는 어노테이션)
### @MappedSuperclass
* 공통 매핑 정보가 필요할 때 사용(id, name)
* 상속관계 매핑X 
* 엔티티X, 테이블과 매핑X 
* 부모 클래스를 상속 받는 자식 클래스에 매핑 정보만 제공 
* 조회, 검색 불가(em.find(BaseEntity) 불가) 
* 직접 생성해서 사용할 일이 없으므로 추상 클래스 권장
* 테이블과 관계 없고, 단순히 엔티티가 공통으로 사용하는 매핑 정보를 모으는 역할 
* 주로 등록일, 수정일, 등록자, 수정자 같은 전체 엔티티에서 공통 으로 적용하는 정보를 모을 때 사용
>@Entity 클래스는 엔티티나 @MappedSuperclass로 지 정한 클래스만 상속 가능






















