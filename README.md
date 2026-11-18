# B-*Cat*
**B**uilding energy **B**enchmarking ***Cat***egories in South Korea
건축물대장 용도체계를 준용한 건물에너지 벤치마킹용 용도 카테고리

[![License: BSD 3-Clause](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](LICENSE)

> **상태:** 최신본 `v2026.11.18` (`대표용도_매칭_261118.xlsx`) — 2026 신규 BM 용도 분류표(별표 1) 반영

---

## 1. 개요

- 건축물대장은 3수준 용도분류 체계를 가지며, 3수준 용도는 세움터에서 **"대표용도"**(`FLR_MAIN_PURPS_CD`)라는 이름으로 사용됩니다.
- 3수준 용도는 건축물대장 **층별개요(층용도)** 또는  전유부(용도)에 기재됩니다.
- 대표용도는 매우 세분화되어 있어 기후지역 × 용도 × 규모로 층화하면 그룹별 표본수가 극단적으로 적어집니다.
- B-Cat은 **용도적 유사성을 유지하면서 충분한 표본수를 확보**하도록 대표용도를 재구성한 **2수준 벤치마킹 용도 카테고리**입니다.

| 수준 | 컬럼 | 개수 (v2026.11.18) | 예시 |
|---|---|---|---|
| 입력: 세움터 층용도(대표용도) | `FLR_MAIN_PURPS_CD` | 722 | `09101` 종합병원 |
| BM 세부용도 (LV2) | `LV2_B_PURPS_CD2` | 250 | `BM09010` 종합병원 |
| BM 대분류 (LV0) | `LV0_B_PURPS_CD` | 35 (BM01–BM31, BMZ0·Z3·Z6·Z8·Z9) | `BM09` 의료시설 |

---

## 2. 적용 범위

| 구분 | 대장 대분류 코드 | BM 대분류 |
|---|---|---|
| **포함** | 01 단독주택 ~ 31 야영장시설 | 대장 대분류 번호와 동일한 `BMxx` |
| **포함** | 32 교정시설, 33 국방·군사시설 | `BM23` 교정및군사시설에 통합 |
| **포함** | 22 분뇨·쓰레기처리시설 | `BM30` 자원순환관련시설에 통합 (BM22 없음) |
| **포함** | Z5 문화및집회시설, Z7 축사·가축시설 | 각각 `BM05`, `BM21`로 통합 |
| **포함** | Z0, Z3, Z6, Z8, Z9 (구 용도코드) | `BMZx`로 유지 |



---

## 3. 파일 구성

| 파일 | 버전 | 주요 내용 |
|---|---|---|
| `대표용도_매칭_261118.xlsx` | **최신** | 2026 신규 BM 용도 분류표 반영 |
| `대표용도_매칭_261118.csv` | 최신 | 기준 매칭표 (시트 1과 동일, UTF-8) |
| `BM용도분류표_261118.csv` | 최신 | BM 세부용도 코드 사전 (시트 2와 동일) |
| `변경대응표_240123_261118.csv` | 최신 | 240123 → 2026 층용도코드별 구·신 분류 대응, 변경유형(동일/명칭변경/재배정/신규코드) |
| `archive/대표용도_매칭_240123.xlsx` 외 2개 | 이전 | 230628, 240111, 240123 버전 |

### 시트 설명 (최신본)

| 시트 | 용도 | 비고 |
|---|---|---|
| `메타정보` | 버전별 수정 이력 | |
| `0 활용방법` | 그룹화 절차 | |
| `1 BM용도분류표+대표용도` | **기준 매칭표** (층용도 → LV2 → LV0) | **이 시트를 사용하십시오** |
| `2 BM용도분류표` | LV2·LV0 코드 사전, 변경유형(유지/명칭변경/신설), 층용도코드 수 | |
| `3 컬럼설명` | 컬럼 정의 | |
| `4 폐지 세부용도` | 2026에서 사라진 기존 세부용도 27개 | 번호 재사용 안 함 |

---

## 4. 컬럼 정의

| 컬럼 | 설명 | 형식 |
|---|---|---|
| `FLR_MAIN_PURPS_CD` | 세움터 층용도(대표용도) 코드 (층별개요/전유부) | 5자리 문자열, 예 `01001` |
| `FLR_MAIN_PURPS_CD_NM` | 층용도명 | |
| `LV2_B_PURPS_CD2` | BM 세부용도 **고유 코드** = `LV0_B_PURPS_CD` + `LV2_B_PURPS_CD` | 예 `BM01002` |
| `LV2_B_PURPS_CD` | 대분류 내 세부용도 일련번호 (대분류 간 중복됨) | 3자리, 예 `002` |
| `LV2_B_PURPS_CD_NM` | BM 세부용도명 | |
| `LV0_B_PURPS_CD` | BM 대분류 코드 | 예 `BM01`, `BMZ3` |
| `LV0_B_PURPS_CD_NM` | BM 대분류명 | |
| `ETC_YN` | 기타여부 (별표 1의 `○`) | `Y` / `N` |

- **조인 키는 반드시 `LV2_B_PURPS_CD2`를 사용**하십시오. `LV2_B_PURPS_CD`만으로는 대분류 간에 중복됩니다.
- 코드는 **문자열**로 읽어야 앞자리 0이 보존됩니다 (예: pandas `dtype=str`).

### 코드 부여 원칙 (v2026)

1. 같은 대분류 안에서 세부용도명이 기존과 같으면 **기존 번호 유지** (141개)
2. 이름만 바뀌고 소속 층용도코드가 그대로면 **기존 번호 유지, 명칭변경** (11개, 예: 고시원 → 다중생활시설)
3. 그 밖의 세부용도는 대분류 내 기존 최대 번호 다음부터 **신규 번호** (98개)
4. 폐지된 세부용도 번호(27개)는 **재사용하지 않음**

---

## 5. 사용법

1. 건축물대장(층별개요 또는 전유부)에서 층용도코드 `FLR_MAIN_PURPS_CD`를 확인합니다.
2. 최신 매칭표와 `FLR_MAIN_PURPS_CD`로 조인하여 LV2·LV0를 부여합니다.

```python
import pandas as pd

bcat = pd.read_csv("대표용도_매칭_261118.csv", dtype=str)
# 또는 pd.read_excel("대표용도_매칭_261118.xlsx", sheet_name="1 BM용도분류표+대표용도", dtype=str)

df = df.merge(bcat, on="FLR_MAIN_PURPS_CD", how="left")
```

- 240123 기준 결과와 비교하려면 `변경대응표_240123_261118.csv`의 `OLD_LV2_B_PURPS_CD2` ↔ `NEW_LV2_B_PURPS_CD2`를 사용합니다.

---

## 6. 알려진 문제 및 검토 사항 (Known issues)

| # | 내용 | 비고 |
|---|---|---|
| 1 | 별표 1의 공식 출처(고시·지침명) 미확정 | 확정 후 README·메타정보에 기재 |
| 2 | 근린생활시설 대분류명이 `제1종근생` / `제2종근생` / `근린생활시설`(Z3)로 표기가 다름 | 원문 표기 유지, 코드로 구분 |
| 3 | `Z9001` 교도소는 대분류 `공공용시설`(BMZ9)이나 세부용도명은 `교정시설` | `BM23` 교정시설과 코드가 다름에 유의 |
| 4 | 층용도코드가 1개뿐인 세부용도 95개 | 표본수 확보 측면에서 병합 검토 필요 |
| 5 | 이전 버전(240123)의 `연구단 송부 240122` 시트 코드 불일치 | 구버전 문제, `archive/` 파일 사용 시 유의 |

---

## 7. 버전 관리 및 변경 이력

- 버전 표기: `vYYYY.MM.DD` (파일명의 `YYMMDD`와 대응)
- 이전 버전 파일은 `archive/` 폴더에 보관합니다.

| 버전 | 일자 | 변경 내용 |
|---|---|---|
| — | 2020-12-04 | 최초 작성 |
| — | 2021-12-10 | BM 용도분류 1차 확정 |
| v2023.06.28 | 2023-06-28 | 1차 확정본 일부 수정, BM 대분류를 대장 대분류와 통일 (대표용도 450개) |
| v2024.01.11 | 2024-01-11 | 의료시설 수정: 종합병원·치과병원 분리 / 학원·교습소 → 학원시설 / 대분류 헤더코드 32개 추가 (대표용도 482개) |
| v2024.01.23 | 2024-01-22 | KALIS 송부 시트 추가 (분류체계 변경 없음) |
| **v2026.11.18** | 2026-11-18 | **2026 신규 BM 용도 분류표(별표 1) 반영**: 층용도 722개(+240, 17~33번 대분류 포함), 대분류 35개, 세부용도 250개(유지 141·명칭변경 11·신설 98, 폐지 27). 기존 482개 중 세부용도 재배정 51개·명칭변경 23개. `기타여부` 컬럼 추가, CSV·변경대응표 제공 |


---

## 8. 주의사항 및 향후 과제

- 본 분류는 **건물에너지 벤치마킹용 용도 카테고리**이며, 의견 수렴에 따라 수정·변경·병합될 수 있습니다.
- 세부용도별 표본수와 에너지 사용 특성의 유사성에 대한 정량 검증 결과는 추후 공개 예정입니다.
- 데이터 기반(data-driven) 신규 분류 카테고리 연구가 필요합니다.
- 의견 및 오류 제보: GitHub **Issues**를 이용해 주십시오.

---

## 9. 관련 참고문헌

- 한국건설기술연구원 (2020). 공공건축물 에너지 소비량 보고 안정화 및 평가체계 사용기반 마련. 국토안전관리원 수탁용역, KICT 2020-052.
- 김혜기, 추한경, 김덕우 (2026). 공공건축물 에너지 성능개선 대상 선별관리를 위한 지역·용도별 평가지표 기준값 도출. *한국건축친환경설비학회 논문집*, 20(1), 11–28. [KCI](https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART003306106)
  - (영문) Development of Assessment Indicators for Selecting Public Buildings for Energy Performance Improvement: Derivation of Benchmarks and Explanatory Indicators

---

## 10. License

본 저작물은 **BSD 3-Clause License**에 따라 배포된다. 하단의 Copyright 및 NOTICE 문구와 해석상 충돌이 있는 경우, 이용·재배포 조건은 BSD 3-Clause License를 우선 적용한다. 전문은 [LICENSE](LICENSE)를 참조한다.

This work is distributed under the **BSD 3-Clause License**. In case of any conflict with the Copyright or NOTICE statements below, the terms of the BSD 3-Clause License shall prevail. See [LICENSE](LICENSE) for the full text.

---

출처 표기:
국문: 건물부문 탄소중립 가속화를 위한 건물에너지 소비 데이터 통합관리 기반기술 개발 (과제번호 : RS-2023-00244769)

## Funding
Research was conducted under the KICT Research Program (project no. 20220260-001, Data-Centric Checkup Technique of Building Energy Performance) funded by the Ministry of Science and ICT.

This work is supported by the Korea Agency for Infrastructure Technology Advancement(KAIA) grant funded by the Ministry of Land, Infrastructure and Transport (Grant RS-2023-00244769, Development of a data framework for integrating building energy datasets and applications to accelerate carbon neutrality in the building sector).

## Copyright
B-Cat : Building energy Benchmarking Categories in South Korea Copyright (c) 2023 Korea Institute of Civil Engineering and Building Technology, KICT (subject to receipt of any required approvals from South Korea Ministry of Land, Infrastructure and Transport, MOLIT). All rights reserved.

If you have questions about your rights to use or distribute this code, please contact KICT at [deukwookim@kict.re.kr](mailto:deukwookim@kict.re.kr).

NOTICE. This source code was developed under funding from the MOLIT South Korea Government consequently retains certain rights. As such, the South Korea Government has been granted for itself and others acting on its behalf a paid-up, nonexclusive, irrevocable, worldwide license in the code to reproduce, distribute copies to the public, prepare derivative works, and perform publicly and display publicly, and to permit other to do so.

끝.
