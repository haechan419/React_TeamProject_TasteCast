# 🌤️ 오늘 뭐 먹지 - 날씨 기반 음식 추천 웹 애플리케이션

## 📌 기획의도
저희 프로젝트 **"오늘 뭐 먹지"** 는 **날씨에 따라 적합한 음식을 추천하는 웹 애플리케이션**을 개발하고자 시작되었습니다.  

기존 레시피 추천 사이트는 날씨 정보를 반영하지 못하거나, 사용자가 **싫어하는 재료를 필터링**할 수 없다는 한계가 있었습니다.

이러한 한계를 보완하기 위해, 저희는 **현재 날씨에 따라 어울리는 음식을 추천**하고,**개인 취향(좋아요, 싫은 재료)을 반영**하는 맞춤형 레시피 웹사이트를 만들었습니다.

🔗 참고 사이트: [만개의 레시피](https://www.10000recipe.com/)  
- 기존에는 좋아하는 종류, 상황, 재료, 방법, 테마 등만 필터링 가능  
- 저희는 **싫어하는 재료 필터링** 및 **날씨별 추천 기능**을 추가하였습니다.  

메인 화면에서는 **사용자 지역의 날씨에 따라 어울리는 레시피 4개**를 제안합니다.

📘 프로젝트 노션: [React Project Notion](https://www.notion.so/React-Project-5-28caa939b4b3808eb6d8c6c5236f855b)

---

## 🗓️ 개발 기간
- **2025년 10월 14일 (화)** ~ **2025년 10월 20일 (월)** (총 7일)

---

## 👨‍👩‍👦 팀원

| 팀장 | 팀원 | 팀원 |
|:----:|:----:|:----:|
| [<img src="https://github.com/jwantit.png" width="80" height="80">](https://github.com/jwantit) | [<img src="https://github.com/keonyeong.png" width="80" height="80">](https://github.com/keonyeong) | [<img src="https://github.com/haechan419.png" width="80" height="80">](https://github.com/haechan419) |
| **김지원** | **박건영** | **한해찬** |


---

## 🧭 API 명세서

### 🌤️ 현재 위치 날씨 조회 API

| 항목 | 내용 |
|------|------|
| **기능 설명** | latitude, longitude를 기반으로 현재 날씨 정보를 조회 |
| **요청 URL** | `https://api.openweathermap.org/data/2.5/weather` |
| **Method** | `GET` |
| **요청 파라미터** | <ul><li>`lat` : 위도 (예: `37.5665`)</li><li>`lon` : 경도 (예: `126.9780`)</li><li>`appid` : OpenWeather API Key</li><li>`lang` : 언어 코드 (`en`, `kr` 등)</li><li>`units` : 단위 (`metric` → 섭씨)</li></ul> |
| **요청 예시** | `https://api.openweathermap.org/data/2.5/weather?lat=37.5665&lon=126.9780&appid=**비밀**&lang=en&units=metric` |
| **요청 헤더** | 없음 |
| **응답 형식** | JSON |
| **응답 예시 (성공)** | ```json { "coord": { "lon": 126.978, "lat": 37.5665 }, "weather": [ { "main": "Clouds", "description": "broken clouds" } ], "main": { "temp": 20.5, "feels_like": 20.2, "humidity": 60 }, "wind": { "speed": 3.5 }, "name": "Seoul" } ``` |
| **응답 코드** | `200 OK` |
| **에러 예시** | ```json { "cod": 401, "message": "Invalid API key." } ``` |
| **에러 코드** | `400 Bad Request`, `401 Unauthorized` |
| **비고** | <ul><li>무료 버전은 초당 60회 호출 제한</li><li>언어(`lang`)를 `"kr"`로 설정하면 한글 출력 가능</li></ul> |

---

### 🗺️ 좌표 기반 행정구역명 조회 API

| 항목 | 내용 |
|------|------|
| **기능 설명** | latitude, longitude를 이용해 해당 좌표의 행정구역(구/군 단위)을 조회 |
| **요청 URL** | `https://dapi.kakao.com/v2/local/geo/coord2address.json` |
| **Method** | `GET` |
| **요청 파라미터** | <ul><li>`x` : 경도 (longitude)</li><li>`y` : 위도 (latitude)</li></ul> |
| **요청 예시** | `"https://dapi.kakao.com/v2/local/geo/coord2address.json?x=126.9780&y=37.5665" \ -H "Authorization: KakaoAK **비밀**"` |
| **요청 헤더** | `Authorization: KakaoAK {REST_API_KEY}` |
| **응답 형식** | JSON |
| **응답 예시 (성공)** | ```json { "documents": [ { "address": { "region_1depth_name": "서울특별시", "region_2depth_name": "중구", "region_3depth_name": "태평로1가" } } ] } ``` |
| **응답 코드** | `200 OK` |
| **에러 예시** | ```json { "code": -2, "msg": "Invalid API key" } ``` |
| **에러 코드** | `400 Bad Request`, `401 Unauthorized` |
| **비고** | <ul><li>좌표 기준은 WGS84 (일반 GPS 좌표계)</li><li>무료 호출 제한: 일 30,000회</li></ul> |

---

## 🧩 Flow & UI

### 🧭 Flow Chart
![Flow Chart](./images/flow_chart.drawio.png)

---

### 🖥️ UI 미리보기

| 메인 페이지 | 전체 레시피 |
|:------------:|:------------:|
| ![메인 페이지](./images/main_page.png) | ![전체 레시피](./images/allRecipe.png) |

| 레시피 추가 | 레시피 수정 |
|:------------:|:------------:|
| ![추가 페이지](./images/createRecipe.png) | ![수정 페이지](./images/update_page.png) |

---

## 🗂️ 프로젝트 구조

```plaintext
index.js
App.js
src/
  ├── components/
  │   ├── common/
  │   │   ├── Filters/
  │   │   │   └── Filters.js
  │   │   ├── ImageUploadBox/
  │   │   │   └── ImageUploadBox.js
  │   │   ├── LikeButton/
  │   │   │   └── LikeButton.js
  │   │   ├── WeatherInfo/
  │   │   │   └── WeatherInfo.js
  │   ├── recipe/
  │   │   ├── IngredientManager/
  │   │   │   └── IngredientManager.js
  │   │   ├── RecipeFilter/
  │   │   │   └── RecipeFilter.js
  │   │   ├── RecipeItem/
  │   │   │   └── RecipeItem.js
  ├── data/
  │   ├── dummyRecipes_2500.js
  │   └── dummyRecipes.js
  ├── hooks/
  │   ├── useGeolocation.js
  │   ├── useKakaoDistrict.js
  │   └── useWeather.js
  ├── layout/
  │   └── Layout.js
  ├── modules/
  │   ├── toggleLike.js
  │   ├── transWeatherDescription.js
  │   └── useKakaoDistrict.js
  ├── pages/
     ├── Home/
     │   └── Home.js
     ├── RecipeCreate/
     │   └── RecipeCreate.js
     ├── RecipeDetail/
     │   └── RecipeDetail.js
     └── RecipeList/
         └── RecipeList.js
```
---

## ⚙️ 구현할 핵심 기능

### 레시피 목록 표시 및 필터링
- 좋아하는 음식만 보기
- 싫어하는 재료 제외
- 날씨별 필터링

### 좋아요(Like) 기능
- 각 레시피 카드에 ❤️ 버튼을 클릭하여 좋아요 동


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

### 🌙 다크모드
- **다크모드 기능 구현**  
- 사용자 환경에 따라 테마 자동 적용  

![다크모드](./images/darkMode.gif)

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
- OpenWeatherMap API 적용  
- 날씨 API와 더미데이터의 날씨 필터링 기능 구현  
- 네비게이션 바 UI 및 기능 구현  
- KakaoMap API로 현재 사용자 지역 출력  
- OpenWeather 아이콘을 날씨 상태에 따라 변경  
- 메인 페이지 좋아요 버튼 기능 구현  
- Flow Chart 제작  
- 메인 페이지 UI 및 기능 구현  
- 기능별 컴포넌트 분리  
- 레시피 클릭 시 상세 페이지 이동  
- 다크모드 기능 구현  
- 코드 통합  
- SPA 라우팅 기능 구현  

---

## 💻 개발 환경

| 구분 | 내용 |
|:----:|:----|
| **언어** | ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=Javascript&logoColor=white) ![CSS](https://img.shields.io/badge/CSS-1572B6?style=for-the-badge&logo=CSS3&logoColor=white) |
| **개발 도구** | ![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=React&logoColor=white) |
| **IDE** | ![VSCode](https://img.shields.io/badge/VS%20Code-007ACC?style=for-the-badge&logo=VisualStudioCode&logoColor=white) |
| **운영체제** | ![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=Windows&logoColor=white) |


---

## 🌐 접속 주소
- **프론트엔드** : [http://localhost:3000](http://localhost:3000)

---

## 🛠 기술 스택

### 프론트엔드
- **React** : 18  
- **React-DOM** : 18  
- **React-Router-DOM** : 7.9.4  
- **React-Virtualized** : 9.22.6  
- **SASS** : 1.93.2  
- **axios** : 1.12.2

---

## ⚙️ 환경 변수 및 설치

프로젝트 실행 전, 다음 환경 변수를 설치 및 설정해주세요.

```bash
# Yarn 전역 설치
npm install -g yarn

# React 관련 패키지 설치
yarn add react@18 react-dom@18 

# 스타일링 및 아이콘 패키지 설치
yarn add sass classnames react-icons

# 가상 스크롤
yarn add react-virtualized

# 라우팅
yarn add react-router-dom

# HTTP 요청
yarn add axios

# styled-components
yarn add styled-components
```

















