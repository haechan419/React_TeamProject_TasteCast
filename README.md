# 🌤️ 오늘 뭐 먹지 - 날씨 기반 음식 추천 웹 애플리케이션

## 📌 기획의도
저희 프로젝트 "오늘 뭐 먹지"는 **날씨에 따라 적합한 음식을 추천하는 웹 애플리케이션**을 개발하고자 시작되었습니다.  

기존의 레시피 추천 사이트들은 날씨 정보를 반영한 음식 추천 기능이 부족하며, 사용자가 **선호하지 않는 재료를 필터링**하는 기능 역시 제공하지 않는 경우가 많습니다.  

이러한 한계를 보완하기 위해, 저희는 **사용자 맞춤형 음식 추천 + 개인화된 재료 필터링 기능**을 결합한 서비스를 기획하게 되었습니다.

🔗 참고 사이트: [만개의 레시피](https://www.10000recipe.com/)  
- 기존에는 좋아하는 종류, 상황, 재료, 방법, 테마 등만 필터링 가능  
- 저희는 **싫어하는 재료 필터링** 및 **날씨별 추천 기능**을 추가하였습니다.  

메인 화면에서는 **사용자 지역의 날씨에 따라 어울리는 레시피 4개**를 제안합니다.

📘 프로젝트 노션: [React Project Notion](https://www.notion.so/React-Project-5-28caa939b4b3808eb6d8c6c5236f855b)

---

개발 기간
- 25년 10월 13일 (월) ~ 25년 10월 20일 (월) 총 8일

---

팀원
김지원
박건영
한해찬

- 팀장 : 김지원
- 팀원 : 박건영, 한해찬

## 7. API 명세서

| API 이름 | 현재 위치 날씨 조회API |
| --- | --- |
| 기능 설명 | latitude, longitude를 기반으로 현재 날씨 정보를 조회 |
| 요청 URL | https://api.openweathermap.org/data/2.5/weather |
| Method | GET |
| 요청 파라미터 | <ul><li>`lat` : 위도 (예: `37.5665`)</li><li>`lon` : 경도 (예: `126.9780`)</li><li>`appid` : OpenWeather API Key</li><li>`lang` : 언어 코드 (예: `"en"`)</li><li>`units` : 단위 (예: `"metric"` → 섭씨)</li></ul> |
| 요청 예시 | https://api.openweathermap.org/data/2.5/weather?lat=37.5665&lon=126.9780&appid=af13879ad6b38a34dc000a5f2bc5df90&lang=en&units=metric |
| 요청 헤더 | 없음 |
| 응답 형식 | JSON |
| 응답 예시(성공) | json { "coord": { "lon": 126.978, "lat": 37.5665 }, "weather": [ { "main": "Clouds", "description": "broken clouds" } ], "main": { "temp": 20.5, "feels_like": 20.2, "humidity": 60 }, "wind": { "speed": 3.5 }, "name": "Seoul" } |
| 응답 코드 | 200 OK |
| 에러 예시 | json { "cod": 401, "message": "Invalid API key." } |
| 에러 코드 | 400 Bad Request(잘못된 요청), 401 Unauthorized(API 키 오류) |
| 비고 | <ul><li>무료 버전은 초당 60회 호출 제한</li><li>언어(`lang`)를 `"kr"`로 설정하면 한글로도 가능</li></ul> |

| API 이름 | 좌표 기반 행정구역명 조회 API |
| --- | --- |
| 기능 설명 | latitude, longitude를 이용해 해당 좌표의 행정구역(구/군 단위)를 조회 |
| 요청 URL | `https://dapi.kakao.com/v2/local/geo/coord2address.json` |
| Method | GET |
| 요청 파라미터 | <ul><li>`x` : 경도 (longitude)</li><li>`y` : 위도 (latitude)</li></ul> |
| 요청 예시 | "https://dapi.kakao.com/v2/local/geo/coord2address.json?x=126.9780&y=37.5665" \ -H "Authorization: KakaoAK 5181242ef139662b62dcb7b691d43139" |
| 요청 헤더 | `Authorization: KakaoAK {REST_API_KEY}` |
| 응답 형식 | JSON |
| 응답 예시(성공) | json { "documents": [ { "address": { "region_1depth_name": "서울특별시", "region_2depth_name": "중구", "region_3depth_name": "태평로1가" } } ] } |
| 응답 코드 | 200 OK |
| 에러 예시 | json { "code": -2, "msg": "Invalid API key" } |
| 에러 코드 | 400 Bad Request(잘못된 요청), 401 Unauthorized(API 키 오류) |
| 비고 | <ul><li>좌표 기준은 WGS84 (일반적인 GPS 좌표계)</li><li>일일 호출 제한: 무료 30,000회</li></ul> |

--
## 프로젝트 구조
index.js
App.js
src/
  ├──components/
      ├──common/
          ├──Filters/
              ├──Filters.js
          ├──ImageUploadBox/
              ├──ImageUploadBox.js
          ├──LikeButton/
              ├──LikeButton.js
          ├──WeatherInfo/
              ├──WeatherInfo.js
      ├──recipe/
          ├──IngredientManager/
              ├──IngredientManager.js
          ├──RecipeFilter/
              ├──RecipeFilter.js
          ├──RecipeItem/
              ├──RecipeItem.js
  ├──data/
      ├──dummyRecipes_2500.js
      ├──dummyRecipes.js
  ├──hooks
      ├──useGeolocation.js
      ├──useKakaoDistrict.js
      ├──useWeather.js
  ├──layout/
      ├──Layout.jst
  ├──modules/
      ├──toggleLike.js
      ├──transWeatherDescription.js
      ├──useKakaoDistrict.js
  ├──pages
      ├──Home/
          ├──Home.js
      ├──RecipeCreate/
          ├──RecipeCreate.js
      ├──RecipeDetail/
          ├──RecipeDetail.js
      ├──RecipeList/
          └──RecipeList.js


---

## ⚙️ 구현할 핵심 기능

### 🧾 CRUD 기능
- 새로운 레시피를 사용자가 직접 등록할 수 있음  
- 등록된 레시피 및 기존 레시피 조회 가능  
- 기존 레시피 수정 가능  
- 저장된 레시피 삭제 가능

![CRU](./images/CRU.gif)

![D](./images/D.gif)

---

### 🧭 SPA (Single Page Application)
- **React Router**를 이용하여 네비게이션바 클릭 시 페이지 전환  
  - 오늘의 추천  
  - 전체 레시피  
  - 레시피 추가  

![SPA](./images/router.gif)

---

### 📜 가상 스크롤
- **React-Virtualized**를 사용하여 2,500개 이상의 데이터를 효율적으로 렌더링  
- 브라우저 성능 저하 방지

![가상 스크롤](./images/scroll.gif)

---

### ⚡ 코드 스플리팅
- **React.lazy** + **Suspense**를 사용하여 이미지가 많은 페이지의 초기 로딩 속도 개선

![코드 스플리팅](./images/lazy_suspense.gif) 

---

### 🌍 외부 데이터 연동
- **Geolocation API**를 사용해 사용자 위치(위도, 경도) 확인  
- **Kakao Map API**로 지역명(구/군) 조회  
- **OpenWeather API**로 지역의 날씨 정보 확인

![외부 데이터 연동](./images/api.gif)

---

### 📱 반응형 웹
- **PC / 태블릿 / 모바일** 환경 대응  
- 네비게이션바 및 메인 화면 반응형 디자인 적용

![반응형 웹](./images/web.gif)

---

## 👥 팀원 역할 분담

### 👩‍💻 김지원
- 주제 선정 및 일정 수립  
- 역할 분담  
- 레시피 더미 데이터 제작  
- 전체 레시피 페이지 UI 구현  
- 가상 스크롤(React-virtualized) 기능 구현  
- 사용자 레시피 필터 기능 구현  
- 레시피 삭제 및 수정 반영 기능 구현  
- 페이지 CSS 통일 및 UI 개선  
- SPA 라우팅 기능 구현  
- 코드 통합  

---

### 👨‍💻 박건영
- 이미지 업로드 및 미리보기 기능 구현  
- SPA 라우팅 기능 구현  
- 레시피 클릭 시 상세 페이지 이동  
- 네비게이션으로 상세페이지 이동  
- useParams를 이용한 ID별 상세 표시  
- 삭제 시 홈으로 이동  
- 더미데이터 작성 및 구조 정리  
- 수정 모드 기능 구현  
- 이미지 업로드 박스 개선  
- 상세 페이지 UI 및 기능 구현  
- 추가 페이지 UI 및 기능 구현  
- CSS 분리 및 정리  
- 코드 통합  
- Lazy & Suspense 기능 구현  

---

### 👨‍💻 한해찬
- **OpenWeatherMap API** 적용  
- 날씨 API와 더미데이터의 **날씨 필터링 기능 구현**  
- 네비게이션 바 UI 및 기능 구현  
- **KakaoMap API**로 현재 사용자 지역 출력  
- **OpenWeather 아이콘**을 날씨 상태에 따라 변경  
- 메인 페이지 **좋아요 버튼 기능 구현**  
- Flow Chart 제작  
- 메인 페이지 UI 및 기능 구현  
- 기능별 컴포넌트 분리  
- 레시피 클릭 시 상세 페이지 이동  
- **다크모드 기능 구현**  
- 코드 통합  
- SPA 라우팅 기능 구현  

---

## 🧩 Flow & UI

### 🧭 Flow Chart
![Flow Chart](./images/flow_chart.drawio.png)






