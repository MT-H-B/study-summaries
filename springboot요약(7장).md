# 7장 테스트 코드 작성하기

## 7.1 테스트 코드를 작성하는 이유
### 문제 미리 발견
### 리팩토링 리스크 감소
### 테스트 빠르게
### 명세 문서로서의 기능
### 좋은코드 생산
### 명확한 목적 표현 & 불필요한 내용 추가 방지 

## 7.2 단위 테스트와 통합 테스트
### 단위 테스트
### 통합  테스트

## 7.3 테스트 코드를 작성하는 방법
## 7.3.1 Given-When-Then 패턴
### Given
### When
### Then

## 7.3.2 좋은 테스트를 작성하는 5가지 속성(F.I.R.S.T)
### Fast
### Isolated
### Repeatable
### Self-Validating
### Timely

## 7.4 JUnit을 활용한 테스트 코드 작성
### 7.4.1 JUnit의 세부 모듈
#### JUnit Platform
#### JUnit Jupiter
#### JUnit Vintage

### 7.4.2 스프링 부트 프로젝트 생성
### 7.4.3 스프링 부트의 테스트 설정
#### 라이브러리
##### JUnit
##### Spring Test & Spring Boot Test
##### AssertJ
##### Hamcrest
##### Mockito
##### JSONassert
##### JsonPath
### 7.4.4 JUnit의 생명주기
#### @Test
#### @BeforeAll
#### @BeforeEach
#### @AfterAll
#### @AfterEach
### 7.4.5 스프링 부트에서의 테스트
### 7.4.6 컨트롤러 객체의 테스트
### 7.4.7 서비스 객체의 테스트
### 7.4.8 리포지토리 객체의 테스트

## 7.5 JaCoCo를 활용한 테스트 커버리지 확인
### 7.5.1 JaCoCo 플러그인 설정
### 7.5.2 JaCoCo 테스트 커버리지 확인

## 7.6 테스트 주도 개발(TDD)
### 7.6.1 테스트 주도 개발의 개발 주기
#### 실패 테스트 작성
#### 테스틑 통과하는 코드 작성
#### 리팩토링
### 7.6.2 테스트 주도 개발의 효과
#### 디버깅 시간 단축
#### 생산성 향상
#### 재설계 시간 단축
#### 기능 추가와 같은 추가 구현이 용이  