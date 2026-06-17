# 📈 비정형데이터처리 기말 프로젝트 — ESG-DART 4조

**가천대학교 빅데이터경영전공 · 비정형데이터처리 · 4조 **

---

## 📌 프로젝트 주제

**한국 사업보고서 ESG 공시 텍스트와 KCGS 등급의 연관성 분석**

DART 사업보고서의 II·IV·VI 세 섹션 텍스트를 Kiwi 형태소 분석기 + 사용자 확장 시드 사전 63개 + SBERT 임베딩으로 정량화하고, 한국ESG기준원(KCGS)의 ESG 등급과의 연관성을 OLS·Binary Logit·Ordered Logit 회귀로 분석한 프로젝트입니다. ESG 공시의 양(text intensity)보다 구체성(abstract ratio)이 등급과 더 강하게 연결되는지, 그리고 사업보고서의 어느 섹션이 가장 강한 ESG 신호를 담는지를 8개 가설(H1~H8)로 검증하였습니다.

---

## 📂 제출 ZIP 파일 구성

```
4조_ESG-DART_제출.zip
├── 📄 README.md                              ← 본 안내 파일
├── 📄 ESG_KCGS_Paper_FINAL.docx              ← Short Research Paper (소논문)
├── 📄 company_master.csv                     ← 필수 입력: 127개 기업 마스터
├── 📄 dart_sections_final_381rows_fixed.csv  ← 필수 입력: 381 firm-year 사업보고서 텍스트
├── 📄 seed_dictionary_final.csv              ← 필수 입력: 최종 시드 사전 (E·S·G별 단어)
└── 📓 ESG_KCGS_분석_제출용_4조.ipynb         ← 분석 노트북
```

---

## 🔧 노트북 실행 방법 (Google Colab)


실행 환경 : google colab - A100 GPU, 고용량 RAM(다른 환경에서 실행시 결과에 약간의 변화가 있을수 있어요)

### 1️⃣ Google Drive에 폴더 만들고 csv 옮기기

내 드라이브 안에 **`ESG_Project`** 라는 폴더를 만들고, ZIP에서 꺼낸 **3개 csv를 정확히 아래 위치에** 넣어주세요.

```
📁 내 드라이브/
└── 📁 ESG_Project/                                    ← 새로 만들 폴더
    │
    ├── 📄 company_master.csv                          ← ZIP의 csv 그대로 복사
    ├── 📄 dart_sections_final_381rows_fixed.csv       ← ZIP의 csv 그대로 복사
    ├── 📄 .env                                        ← 선택: OpenDART API 키 (Stage 0만 필요)
    │
    └── 📁 Results/                                    ← 새로 만들 하위 폴더
        └── 📄 seed_dictionary_final.csv               ← ZIP의 csv를 Results 폴더 안에 복사
```

> ⚠️ **주의** — `seed_dictionary_final.csv`는 `ESG_Project/` 바로 아래가 아니라 `ESG_Project/Results/` 폴더 안에 넣어야 합니다.

### 2️⃣ 각 csv의 역할

- **`company_master.csv`** — 127개 표본 기업의 기본 정보 (stock_code · fiscal_year · esg_grade · e_grade · s_grade · g_grade · industry · esg_year)
- **`dart_sections_final_381rows_fixed.csv`** — DART에서 추출한 381 firm-year 사업보고서의 II·IV·VI 세 섹션 본문 텍스트
- **`Results/seed_dictionary_final.csv`** — 전처리에 사용할 시드 사전 (PDF 시드 25개 + 사용자 추가 38개 = 총 63개)

### 3️⃣ API 키 설정 (Stage 0 실행 시에만 필요)

`dart_sections_final_381rows_fixed.csv`가 이미 폴더에 있으므로 Stage 0(DART API 수집)은 자동으로 스킵됩니다. 따라서 **API 키는 따로 준비하지 않아도 노트북이 실행됩니다.**

만약 직접 수집을 다시 해보고 싶다면 다음 세 방법 중 하나로 API 키 제공:

1. **Colab Secrets** (권장) — Colab 좌측 🔑 아이콘 → 키 이름 `OPENDART_API_KEY`로 저장
2. **`.env` 파일** — `ESG_Project/.env`에 `OPENDART_API_KEY=발급키` 한 줄 추가
3. **직접 입력** — 노트북 실행 도중 입력창에 붙여넣기

### 4️⃣ 실행 순서

1. ZIP 압축 풀고 3개 csv를 위 구조대로 Google Drive `ESG_Project` 폴더에 옮기기
2. `ESG_KCGS_분석_제출용_4조.ipynb`를 Colab에서 열기
3. 상단 메뉴 `런타임 → 모두 실행`
4. 첫 실행 시 Drive 마운트 권한 허용
5. Stage 0 → 1~6단계가 순차 실행되며 결과 csv와 Figure가 `Results/` 폴더에 자동 저장됩니다

> ⚠️ **주의** — 노트북은 위에서 아래로 순차 실행되어야 합니다. 중간 셀만 단독 실행하면 변수가 없어서 에러가 납니다.

---

## 🎨 노트북 표기 체계

- **🟦🟩🟨🟧🟪🟫** — 6단계 큰 구분 (1단계 ~ 6단계)
- 🟢 **E** (Environment, 환경) / 🟡 **S** (Social, 사회) / 🔵 **G** (Governance, 지배구조)

---

## 👥 조원 역할

📌 **연구 주제 설정, 가설 수립, 분석 방향에 대한 의견 조율은 모든 팀원이 함께 논의하여 결정하였으며, 아래는 최종 산출물 단계의 개별 담당 역할입니다.**

| 이름 | 담당 |
|---|---|
| **강소현** | 발표 |
| **신수현** | 발표 |
| **오지원** | PPT 제작 |
| **황예은** | 코드 · Mini Paper(소논문) |

---

## 🤖 AI 도구 활용 명시

본 프로젝트의 코드 작성 일부와 마크다운 문서화에는 생성형 AI의 도움을 받았습니다. 분석 방법론 결정·결과 해석·학술 보고서 작성·최종 검증은 모두 4조 팀원이 직접 수행하였습니다.

---

*가천대학교 빅데이터경영전공 · 비정형데이터처리 · ESG-DART 4조 · 2026년 6월 16일*
