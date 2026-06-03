## 💨 우리나라 대기오염 데이터 분석

에어코리아에서 제공하는 2025년 5월 대기오염 데이터를 Pandas로 분석한 첫 번째 프로젝트.

### 📊 시각화 결과
<img width="950" height="470" alt="korea_air_quality_analysis" src="https://github.com/user-attachments/assets/a00ab1ed-421f-4ac4-b495-379833614bcf" />

### 📂 사용 데이터
- 출처: 에어코리아 (airkorea.xlsx)
- 내용: 시도별 SO2, NO2, O3, CO, PM10, PM2.5 월평균 수치

### 🔍 분석 내용
1. 데이터 불러오기 및 전처리 (헤더 수동 지정, 불필요한 행 제거)
2. 전국 평균 대비 시도별 PM2.5 차이 계산
3. 막대 그래프 시각화 (전국 평균 기준선 포함)

### 🛠️ 사용 기술
- `pandas` : 데이터 로드, 정제, 연산
- `matplotlib` : 시각화 (한글 폰트: NanumGothic)
- 실행 환경: Google Colab

### 💡 배운 점
- Colab 한글 폰트 설정 — NanumGothic 직접 등록으로 한글 깨짐 방지
- 음수 기호 깨짐 방지 — `axes.unicode_minus = False` 설정
- 기준선 그리기 — `axhline()`으로 전국 평균선 표시
- 막대 위 수치 표시 — `zip(bars, values)`로 위치 계산 후 `ax.text()`로 값 올리기

### 🔗 데이터 출처
- [에어코리아🔗](https://www.airkorea.or.kr)
