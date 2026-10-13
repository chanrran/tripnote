# 트립노트 (Tripnote)

종료 예정인 트리플 앱을 대체하는 개인용 여행 일정 앱.

## 쓰는 사람
- 조영찬 · 박지혜 (두 명)

## 기능
- 여행 생성/수정/삭제
- 일자별(Day) 장소 추가
- Google Places 자동완성으로 장소 검색 (표시 이름은 직접 수정 가능) (이름·주소·위도·경도·영업시간 자동 입력)
- 지도에 핀 + 일자별 동선 표시
- 교통수단별 색상 표시 (도보 / 대중교통 / 차 / 비행기)
- 예상 비용 입력, 일자별 합계
- 각 장소에서 "구글맵 길찾기"로 바로 넘기기
- 현재 시각 하이라이트 (여행 당일)
- 내 위치 표시
- URL로 공유 (`?trip=xxx&day=2`)
- 모바일에서 입력·수정 가능
- PC와 모바일 반응형 (한 파일)

## 기술 스택
- 바닐라 HTML/CSS/JS (빌드 없음)
- Firebase Firestore (실시간 동기화)
- Google Maps JavaScript API + Places + Geometry
- GitHub Pages 호스팅

## 로컬에서 테스트
`index.html`을 그대로 브라우저로 열면 됩니다. 단, Google Places/Maps API 호출을 위해 로컬 서버가 필요합니다:

```sh
cd tripnote
python3 -m http.server 8000
# http://localhost:8000 접속
```

## 배포
GitHub Pages (public repo)에 push 하면 자동으로 `https://chanrran.github.io/tripnote/`에서 접근 가능.

## 보안
- Firestore 보안 규칙(`firestore.rules`)로 특정 User-Agent 또는 특정 Origin에서만 접근 가능하게 설정 가능.
- 현재는 테스트 모드 (30일간 모든 요청 허용). 운영 시 반드시 `firestore.rules`를 적용할 것.
- Google Maps API 키는 GitHub Pages 도메인에서만 사용 가능하도록 GCP 콘솔에서 제한 설정 필요.

## 데이터 구조 (Firestore)
```
trips (collection)
  {tripId}
    title, startDate, endDate, createdAt
    places (subcollection)
      {placeId}
        day, orderNo, name, address, lat, lng,
        arriveTime, departTime, memo, transitMode, cost,
        placeId, hours, createdAt
```
