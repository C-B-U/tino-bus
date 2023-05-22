# 티노 버스 서비스

티노는 학교 버스는 너무 좋아해요! <br>
버스에 사랑에 빠진 티노는 하루종인 버스를 기다린답니다! <br>
티노를 위한 버스 알림 서비스를 만들어보아요!

## 요구사항

### 사전 진행

위 표에 있는 데이터는 기본 데이터이다.<br>
즉, 위 데이터를 기본 세팅 후에 진행한다. <br>
상시 운행중인 시간은 5분 단위로 간격을 두고 추가한다. <br>
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

### 버스 삭제

##### Request : DELETE api/buses/{id}

##### Response : 204 no content

#### 예외 상황

- 만약 id에 해당하는 버스 삭제시 예외처리 해야

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