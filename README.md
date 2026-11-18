# B-*Cat*
**B**uilding energy **B**enchmarking ***Cat***egories in South Korea
건축물대장 용도체계를 준용한 건물에너지 벤치마킹용 용도 카테고리

[![License: BSD 3-Clause](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](LICENSE)

> **상태:** 임시 분류(draft) · 최신본 `v2024.01.23` (`대표용도_매칭_240123.xlsx`)
> 다양한 의견 수렴을 거쳐 카테고리의 수정·변경·병합이 이루어질 예정입니다.

---

## 1. 개요

- 건축물대장은 3수준 용도분류 체계를 가지며, 3수준 용도는 세움터에서 **"대표용도"**(`FLR_MAIN_PURPS_CD`)라는 이름으로 사용됩니다.
- 3수준 용도는 건축물대장 **층별개요(층용도)** 또는 **전유부(용도)**에 기재됩니다.
- 대표용도는 매우 세분화되어 있어 기후지역 × 용도 × 규모로 층화하면 그룹별 표본수가 극단적으로 적어집니다.
- B-Cat은 **용도적 유사성을 유지하면서 충분한 표본수를 확보**하도록 대표용도를 재구성한 **2수준 벤치마킹 용도 카테고리**입니다.

| 수준 | 컬럼 | 개수 (240123 기준) | 예시 |
|---|---|---|---|
| 입력: 세움터 대표용도 | `FLR_MAIN_PURPS_CD` | 482 | `09101` 종합병원 |
| 벤치마킹 세부용도 (LV2) | `LV2_B_PURPS_CD2` | 178 | `BM09010` 종합병원 |
| 벤치마킹 주용도 (LV0) | `LV0_B_PURPS_CD` | 23 (BM01–BM16, BMZ0–BMZ9) | `BM09` 의료시설 |


---

## 2. 적용 범위

| 구분 | 대장 대분류 코드 | 비고 |
|---|---|---|
| **포함** | 01 단독주택 ~ 16 위락시설 | 대장 대분류와 BM 주용도(BM01–BM16)를 1:1로 통일 |
| **포함** | Z0, Z3, Z5–Z9 (구 용도코드) | BMZx로 별도 유지 |
| **제외** | 17 공장 ~ 31 야영장시설 | 공장, 창고, 위험물, 자동차관련, 동·식물관련, 분뇨·쓰레기, 교정·군사, 방송통신, 발전, 묘지, 관광휴게, 가설건축물, 장례, 자원순환, 야영장 |
| **미반영** | 23년 6월 이후 신설 코드 | 예: `03300` 전기자동차충전소 |

- 제외 대분류에 속하는 건물은 B-Cat 매칭 결과가 없습니다(`NA`). 분석 시 별도 처리해 주십시오.


---

## 3. 파일 구성

| 파일 | 버전 | 주요 내용 |
|---|---|---|
| `대표용도_매칭_240123.xlsx` | **최신** | 240111 + KALIS 송부 시트 추가 |
| `대표용도_매칭_240111.xlsx` | 이전 | 의료시설 수정 (종합병원·치과병원 분리), 학원시설 신설 |
| `대표용도_매칭_230628.xlsx` | 초기 드래프트 | BM 대분류를 대장 대분류와 통일 |

### 시트 설명 (최신본 기준)

| 시트 | 용도 | 비고 |
|---|---|---|
| `메타정보` | 버전별 수정 이력 | |
| `0 활용방법` | 그룹화 절차 | |
| `1 BM용도분류표+대표용도` | **기준 매칭표** (대표용도 → LV2 → LV0) | **이 시트를 사용하십시오** |
| `2 BM용도분류표` | LV2·LV0 코드 사전 | |
| `3 컬럼설명` | 컬럼 정의 | 일부 구 컬럼명 기재 (아래 4장 참조) |
| `연구단 송부 240122` | 외부 송부 이력 | ⚠️ 알려진 문제 참조, 분석에 사용 금지 |
| `참고_세움터 주용도 비교 23년6월` | 세움터 코드표(23.6) 대조 | 참고용 |
| `매칭표(old)` | 구 분류 | 참고용, 사용 금지 |


---

## 4. 컬럼 정의

| 컬럼 | 설명 | 형식 |
|---|---|---|
| `FLR_MAIN_PURPS_CD` | 세움터 대표용도 코드 (층별개요/전유부) | 5자리 문자열, 예 `01001` |
| `FLR_MAIN_PURPS_CD_NM` | 세움터 대표용도명 | |
| `LV2_B_PURPS_CD2` | 벤치마킹 세부용도 **고유 코드** = `LV0_B_PURPS_CD` + `LV2_B_PURPS_CD` | 예 `BM01002` |
| `LV2_B_PURPS_CD` | 주용도 내 세부용도 일련번호 (주용도 간 중복됨) | 3자리, 예 `002` |
| `LV2_B_PURPS_CD_NM` | 벤치마킹 세부용도명 | |
| `LV0_B_PURPS_CD` | 벤치마킹 주용도 코드 | 예 `BM01`, `BMZ3` |
| `LV0_B_PURPS_CD_NM` | 벤치마킹 주용도명 | |

- 구 컬럼명 대응: `B2_PURPS_CD_NM` → `LV2_B_PURPS_CD_NM`, `B1_PURPS_CD_NM` → `LV0_B_PURPS_CD_NM`
- **조인 키는 반드시 `LV2_B_PURPS_CD2`를 사용**하십시오. `LV2_B_PURPS_CD`만으로는 주용도 간에 중복됩니다.
- 코드는 **문자열**로 읽어야 앞자리 0이 보존됩니다 (예: pandas `dtype=str`).


---

## 5. 사용법

1. 건축물대장(층별개요 또는 전유부)에서 대표용도 코드 `FLR_MAIN_PURPS_CD`를 확인합니다.
2. 최신본의 `1 BM용도분류표+대표용도` 시트와 `FLR_MAIN_PURPS_CD`로 조인하여 LV2·LV0를 부여합니다.
3. 복합용도 건물은 연면적 최대 용도 등 사용자가 정한 규칙으로 건물 단위 대표용도를 먼저 정한 뒤 적용합니다.

```python
import pandas as pd

bcat = pd.read_excel("대표용도_매칭_240123.xlsx",
                     sheet_name="1 BM용도분류표+대표용도",
                     header=1, dtype=str)

df = df.merge(bcat[["FLR_MAIN_PURPS_CD", "LV2_B_PURPS_CD2",
                    "LV2_B_PURPS_CD_NM", "LV0_B_PURPS_CD", "LV0_B_PURPS_CD_NM"]],
              on="FLR_MAIN_PURPS_CD", how="left")
```


---

## 6. 알려진 문제 (Known issues)

| # | 내용 | 영향 | 조치 |
|---|---|---|---|
| 1 | `연구단 송부 240122` 시트의 `LV2_B_PURPS_CD2` 코드가 기준 시트와 불일치 (450행 중 361행, 하나의 코드에 여러 세부용도명이 대응하는 경우 55건). 세부용도명은 대체로 정상 | 해당 시트 코드로 조인하면 분류 오류 | 기준 시트(`1 …`) 사용, 정정본 배포 예정 |
| 2 | BMZ7 주용도명이 `가축시설(Z7)`과 `축사(Z7)`로 혼용 | 집계 시 그룹 분리 | 다음 버전에서 통일 |
| 3 | `기타 운동시설` / `기타운동시설` 표기 혼용 | 이름 기준 집계 시 분리 | 코드 기준 집계 권장 |
| 4 | `2 BM용도분류표`에만 있는 코드 `BMZ7002` | 영향 없음 | 정리 예정 |
| 5 | `3 컬럼설명` 시트가 구 컬럼명(B1/B2) 설명 | 혼동 | 4장 대응표 참조 |
| 6 | 17~31번 대분류, 23.6 이후 신설 코드 미포함 | 매칭 결과 `NA` | 2장 참조 |


---

## 7. 버전 관리 및 변경 이력

- 버전 표기: `vYYYY.MM.DD` (파일명의 `YYMMDD`와 대응)
- 향후 배포는 GitHub **Releases / tag**로 관리하고, 기계가독용 **CSV(UTF-8)**를 함께 제공할 예정입니다.

| 버전 | 일자 | 변경 내용 |
|---|---|---|
| — | 2020-12-04 | 최초 작성 |
| — | 2021-12-10 | BM 용도분류 1차 확정 |
| v2023.06.28 | 2023-06-28 | 1차 확정본 일부 수정, BM 대분류를 대장 대분류와 통일 (대표용도 450개) |
| v2024.01.11 | 2024-01-11 | 의료시설 수정: 종합병원·치과병원 분리 / 학원·교습소 → 학원시설 / 대분류 헤더코드 32개 추가 (대표용도 482개) |
| v2024.01.23 | 2024-01-22 | KALIS 송부 시트 추가 (분류체계 변경 없음) |
| (예정) | 2024 하반기 이후 | 1차 의견 수렴 버전 |


---

## 8. 주의사항 및 향후 과제

- 본 분류는 **임시적 용도 카테고리**이며, 의견 수렴에 따라 수정·변경·병합될 수 있습니다.
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
