# 티노 버스 서비스

티노는 학교 버스를 너무 좋아해요! <br>
버스에 사랑에 빠진 티노는 하루종일 버스를 기다린답니다! <br>
티노를 위한 버스 알림 서비스를 만들어보아요!

## 요구사항

### 사전 진행
<img width="653" alt="image" src="https://github.com/C-B-U/tino-bus/assets/55674648/f591c732-05f3-4e07-9884-b736d4ab778d">


위 표에 있는 데이터는 기본 데이터이다.<br>
즉, 위 데이터를 기본 세팅 후에 진행한다. <br>
상시 운행중인 시간은 5분 단위로 간격을 두고 추가한다. <br>
예외 메시지는 자율적으로 작성한다. <br>
<a style="color: red;font-size: xx-large"> 제발 여기다 push/merge 하지 마세요!! fork 한 후에 각자 닉네임으로 branch 파서 제작하고 PR 날려주세용 </a> <br>
<a style="color: blue;font-size: xx-large"> 착한 우리 부원들은 문제 끝까지 읽고 풀기!!! 제발~ </a>

### 새로운 버스 추가

##### Request : POST api/buses

```json
{
  "startPosition": "school",
  "departureTime": "10:30"
}
```

##### Response : 201 created

```json
{
  "id": 6,
  "startPosition": "school",
  "startAt": "10:30"
}
```

#### 예외 상황

- 시작지가 같으며, 기존에 운행하는 버스와 출발 시간이 5분 미만으로 차이가 나면 안 됨.
  ex) 기존에 10:30분 출발 버스가 있으면, 1:34분에 출발하는 버스는 추가할 수 없음

### 버스 불러오기

##### Request : Get api/buses?start-position=출발위치

##### Response : 200 ok

```json
[
  {
    "id": 5,
    "startAt": "16:30"
  },
  {
    "id": 6,
    "startAt": "16:40"
  },
  {
    "id": 7,
    "startAt": "16:50"
  }
]
```

##### Request : Get api/buses?start-position=출발위치&start-at=12:26
##### Response : 200 ok
```json
[
  {
    "id": 5,
    "startAt": "12:30"
  },
  {
    "id": 6,
    "startAt": "12:40"
  },
  {
    "id": 7,
    "startAt": "12:50"
  }
]
```

##### Request : Get api/buses?start-position=출발위치&page=5
##### Response : 200 ok
```json
[
  {
    "id": 5,
    "startAt": "16:30"
  },
  {
    "id": 6,
    "startAt": "16:40"
  },
  {
    "id": 7,
    "startAt": "16:50"
  },
  {
    "id": 8,
    "startAt": "17:00"
  },
  {
    "id": 9,
    "startAt": "17:10"
  }
]
```
#### 파라미터값 :

- start-position : 필수
- start-at : 선택, 기본값 = 현재시간
- page : 선택, 기본값 = 3

#### 예외사항

- 파라미터값이 정해진 형식이 아니라면 예외처리

### 버스 수정
##### Request : put api/buses/{id}
```json
{
  "startPosition": "school",
  "departureTime": "10:30"
}
```

##### Response : 200 ok
```json
{
  "id": 5,
  "startPosition": "school",
  "departureTime": "10:30"
}
```

#### 예외상황
- 만약 id에 해당하는 버스가 없을 시 예외처리 해야
- 시작지가 같으며, 기존에 운행하는 버스와 출발 시간이 5분 미만으로 차이가 나면 안 됨.
  ex) 기존에 10:30분 출발 버스가 있으면, 10:34분에 출발하는 버스는 추가할 수 없음



### 버스 삭제

##### Request : DELETE api/buses/{id}

##### Response : 204 no content

#### 예외 상황
- 만약 id에 해당하는 버스가 없을 시 예외처리 해야

## 지원 환경
- java 11 이상
- gradle
- spring-boot 2.7

## 기본 세팅 라이브러리 (필요시 추가 가능)
- spring-boot-starter-data-jpa
- spring-boot-starter-validation
- lombok
- h2

## 규칙

0. 손보다 머리로 먼저 하세요
   1. 머리로 먼저 푼 다음 -> 기능 목록 작성
   2. 처음부터 TODO 를 100% 완벽하게 만들 수 없어
   3. 최대한 기능 목록 먼저 만들고, 구현하면서 유동적으로 변경, 추가 가능
1. 메서드 길이를 최대한 짧게 (최대 10)
2. 파라미터 개수도 최소화
3. MVC 모듈 분할
4. controller - service - repository 단계 분할
5. 일급 컬렉션 생성
6. Exception Handler 생성
7. else, switch, ? 금지
8. 들여쓰기는 최대 2까지만 서용
```java
class A { 
  void hello () {
    while {
    	if {
			// 여기까지만 허용
  	} 
    }
  }
}
```

9. 나만의 단위 테스트
10. 단위 구현 당 커밋 나누기, 커밋 규칙 준수
- method(주요 클래스) : 설명
- method :
  - test
  - feat
  - fix
  - chore
  - docs
  - refactor
  

11. 리펙터링 필수
12. 미리 깔끔하게 짜면 좋지만, 일단 구현 -> 후 리펙터링도 가능

13. 이것까지 바라지는 않는다 왜냐면 나도 못함
- 기능 목록 작성 이후, 단위 테스트를 먼저 만든다, 그 이후 테스트에 맞춰서 구현 : TDD -> 찐또배기로 하면 ㅈㄴ 어려워
- TDD 연습하는데 이런 문제가 더 없음
- 너의 목적이 TDD가 아니라 생각, 궁극적 목적 : 객체지향적 설계 : TDD 욕심 일단 부리지 말고, 객체지향적으로 먼저 접근해보고
- 그 이후 자신감이 붙으면 TDD도 트라이 해보셈

14. 앵간하면 배열 ㄴㄴ 리스트 쓰세용, 추가적으로 람다 스트림 변환 가능하면 람스 쓰세용, 반복문 쓰지 마시고(앵간하면)


- 궁극적 목표 : 객체지향적 설계 - 클린코드
- 만약에 자신감이 붙는다?
  - 람다 스트림
  - TDD

- 무지성 풀이 : 30분 컷
- 위에 규칙 다 지키면 솔직히 10시간 오바 쌉가능
- 다풀고 리뷰 원하면 수다방에 편하게 말씀해주세용
- 문제 오류 혹은 더 나은 방향으로 수정 가능하면 PR 편하게 날려주세용
