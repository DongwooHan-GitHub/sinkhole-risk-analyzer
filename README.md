# 🕳️ Sinkhole & Pothole Risk Analyzer

싱크홀 및 포트홀을 이미지로 감지하고, 위치 기반의 도로 정보 · 기상 · 교통 데이터를 결합하여 위험도를 분석하는 AI 기반 시스템입니다.

---

## 📖 Description

이 프로젝트는 사용자가 업로드한 도로 이미지와 GPS 위치를 바탕으로 다음 정보를 종합 분석합니다:

- 📷 **YOLOv8 기반 객체 탐지**로 포트홀 및 싱크홀 감지
- 🧭 **GIS API**를 통한 도로 속성 확인 (도로 유형, 재질, 자전거도로 여부 등)
- 🌦️ **기상청 API**로 날씨 및 강수량, 기온 등 수집
- 🚗 **교통 API**로 해당 위치의 교통량 및 정체 수준 분석

이를 통해 도로 파손 상태에 대한 **정확한 객체 감지 결과**와, 해당 파손의 **위험도 지수(Risk Level)** 를 사용자에게 전달합니다.

> 사용자는 웹에서 이미지를 업로드하면 분석 결과와 함께 "어디가 위험한가", "왜 위험한가"에 대한 정량적 근거를 확인할 수 있습니다.

---

## 🗄️ Dataset

### 📌 Pothole / Sinkhole Detection

| Class     | Count |
|-----------|-------|
| Pothole   | 8,500 |
| Sinkhole  | 3,200 |
| Background / None | 10,000+ |

> 다양한 도로 상황에서 촬영된 실제 이미지 및 합성 이미지로 구성되어 있습니다.

---

## 🔧 Stack

### Language & Framework
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=flat&logo=fastapi)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

### Deep Learning
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![YOLOv8](https://img.shields.io/badge/YOLOv8-000000?style=flat&logo=github&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat&logo=opencv&logoColor=white)

### Data Handling & Logging
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas)
![NumPy](https://img.shields.io/badge/Numpy-013243?style=flat&logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat)
![Logging](https://img.shields.io/badge/Logging-000000?style=flat)

### API
- **기상청 단기 예보 API**
- **VWorld WFS 도로 정보 API**
- **ITS 교통 API (서울시 TOPIS 등)**

---

## 🗂️ Directory Structure

project/
├── main.py # FastAPI 서버 실행
├── upload.html # 사용자 업로드 페이지 (HTML/JS)
├── detector.py # YOLOv8 객체 탐지 처리
├── utils.py # 유틸 함수 모음
├── db.py # SQLite 연동
├── detection_results.db # 탐지 결과 DB
│
├── pothole_api.py # 포트홀 분석 API
├── weather_api.py # 기상청 API 호출
├── traffic_api.py # 교통 API 호출
├── gis_api.py # 도로 속성(GIS) API
├── gps_image_api.py # 이미지 + 위치 연동 API
│
├── uploaded_images/ # 사용자 업로드 이미지 저장
├── static/ # 정적 파일 (JS, CSS 등)
├── logs/ # 로그 파일 저장
│ ├── gis_api.log
│ ├── traffic_api.log
│ └── road_gis.log


---

## 🔁 전체 서비스 순서도

1. **사용자 요청**
   - HTML 페이지에서 이미지 + 위치 업로드
   - POST `/analyze-image` 호출

2. **FastAPI 서버 처리**
   - 업로드된 이미지와 좌표 수신
   - 병렬 분석 시작

3. **객체 탐지 (YOLOv8)**
   - 싱크홀/포트홀 객체 감지 (bbox + confidence)

4. **GIS 도로 속성 조회**
   - VWorld API로 도로 재질/종류 등 확인

5. **기상 정보 수집**
   - 기상청 API 통해 해당 위치 날씨 확인

6. **교통 정보 수집**
   - ITS API 통해 교통량/정체 등 확인

7. **위험도 분석**
   - 감지 결과 + 도로 속성 + 날씨 + 교통 → 위험도 종합 판단
   - 결과 JSON 생성
     ```json
     {
       "type": "sinkhole",
       "risk_level": "High",
       "reasons": ["폭우", "왕복 4차선 도로", "YOLO confidence: 0.91"]
     }
     ```

8. **사용자에게 응답 반환**
   - 분석 결과를 HTML 페이지에 시각화 + JSON 데이터 전달

---

## 🏁 시연 예시 (DEMO)

👉 사진 업로드 → 분석 결과 표시  
👉 위험 요소 설명까지 시각적으로 안내

> 추후 추가될 영상 링크 또는 시연 GIF 삽입 예정

---

## 📌 TODO / 향후 개선 방향

- [ ] 실시간 지도 기반 파손 위치 마커 표시
- [ ] 예측 위험도 색상별 레벨링 (ex. Green/Yellow/Red)
- [ ] 관리자용 DB 대시보드 (Streamlit 또는 SQLite Viewer)

---

## 👨‍💻 Author

**Dongwoo Han**  
AI 기반 컴퓨터 비전 개발자 포트폴리오  
GitHub: [@DongwooHan-GitHub](https://github.com/DongwooHan-GitHub)

