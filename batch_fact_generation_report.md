# Batch Fact Generation Report

**날짜**: 2026-02-23
**작업**: Knowledge Graph에 ~1000개 팩트 일괄 추가

---

## 1. 결과 요약

| 항목 | Before | After | 변동 |
|------|--------|-------|------|
| Nodes | 1,740 | 2,719 | +979 |
| Facts | 1,526 | 2,506 | **+980** |
| Edges | 2,237 | 3,252 | +1,015 |
| Components | 182 | 181 | -1 (더 연결됨) |

- **목표 1000 대비 달성률**: 98% (980/1000)
- **Edge 에러**: 0건 (수정 완료)
- **Belief 커버리지**: 40개 중 34개 belief에 새 evidence 연결 (85%)

---

## 2. 배치별 팩트 수

| Batch | 도메인 | Facts | 생성 방식 |
|-------|--------|-------|-----------|
| credit_liquidity | AI Credit, PC, ETF, CCP, Basis, Reserve, Dollar, Regulation | 78 | 직접 작성 |
| macro_fiscal | GDP, Employment, Consumer, Inflation, Fiscal, Fed, Tariff, UST 30Y | 59 | 직접 작성 |
| international | BOJ, Europe, China, Dollar/FX, EM, Geopolitics, Global Rates | 142 | Agent 작성 |
| structural | Climate, Insurance, Demographics, Greenflation, Water, CBDC, Automation, SaaS | 128 | Agent 작성 |
| impl_evidence | Unpriced hypothesis evidence (22개 cluster) | 62 | 직접 작성 |
| credit_liquidity_2 | Credit 도메인 deep dive | 46 | 직접 작성 |
| macro_fiscal_2 | Macro 도메인 deep dive | 36 | 직접 작성 |
| impl_evidence_2 | 추가 hypothesis evidence (15개 cluster) | 33 | 직접 작성 |
| extra | BOJ, Europe, China, Auto, Energy, Geopolitics, Climate, Demo, Insurance | 51 | 직접 작성 |
| em_frontier | EM sovereign, CB, FX, countries, restructuring, digital, climate | 33 | 직접 작성 |
| japan_deep | BOJ policy, JGB, economy, yen, financial system, real estate | 24 | 직접 작성 |
| china_asia | Property, economy, financial, tech, commodities, Asia ex-China | 26 | 직접 작성 |
| commodities_energy | Oil, gas, metals, agriculture, energy transition, critical minerals | 26 | 직접 작성 |
| microstructure_vol | Options/vol, flows, market structure, positioning, credit micro, Treasury | 27 | 직접 작성 |
| fixed_income | Curve, MBS, IG/HY, muni, pension/insurance, inflation-linked, cross-domain | 23 | 직접 작성 |
| fed_liquidity | Fed policy, FHLB, global liquidity, banking stress, regulation, dealer BS | 25 | 직접 작성 |
| geopolitics_demo | US-China trade, tech export, Europe geo, Middle East, demographics, sanctions, climate | 25 | 직접 작성 |
| cross_domain | AI deep, PC×banking, ETF structure, basis trade, CCP, regulation, implications | 28 | 직접 작성 |
| final_gap1 | Automation/AI labor, insurance, longevity, water, climate, greenflation | 28 | 직접 작성 |
| final_gap2 | US fiscal, labor, consumer, housing, equity, tech, Europe, CBDC | 30 | 직접 작성 |
| final_gap3 | US macro indicators, Fed, FX, credit, Europe/UK, structural, belief evidence | 26 | 직접 작성 |
| final_gap4 | Remaining gaps: US/EU/China/Japan/energy/cross-domain/belief evidence | 24 | 직접 작성 |
| **합계** | | **980** | |

---

## 3. Belief별 Evidence 수

상위 15개 (supports + challenges 합산):

| Belief | Edges | 주요 도메인 |
|--------|-------|-------------|
| BLF_SUPPLY_CHAIN_BLOC | 55 | Trade, China, EM, Tech, Geo |
| BLF_GREENFLATION | 53 | Energy, Transition, Metals, Climate |
| BLF_BOJ_RATE | 52 | JGB, Yen, Japan economy, Banks |
| BLF_DOLLAR_FUNDING_FRAGILITY | 48 | FX swap, EM, Repo, Capital flows |
| BLF_SOVEREIGN_DEBT_SPIRAL | 46 | Fiscal, EM debt, EU, Japan |
| BLF_CHINA_PROPERTY_COMMODITY_DRAG | 43 | Property, LGFV, Commodities, Deflation |
| BLF_AI_CREDIT | 38 | Capex, Data centers, Revenue, Chips |
| BLF_US_GROWTH_DECELERATION | 37 | GDP, ISM, Housing, Labor |
| BLF_TARIFF_INFLATION_PIPELINE | 37 | Tariffs, Pass-through, Food, Energy |
| BLF_CLIMATE_MACRO_VARIABLE | 34 | Disasters, Insurance, Sea level, Carbon |
| BLF_RESERVE_SCARCITY | 33 | Gold, FX reserves, QT, RRP, Fed |
| BLF_SHADOW_BANKING_SYSTEMIC | 32 | Private credit, NBFI, BDC, NAV lending |
| BLF_ETF_LIQUIDITY_ILLUSION | 31 | 0DTE, Passive, Concentration, VIX |
| BLF_GREAT_LIQUIDITY_MIGRATION | 27 | NISA, SWF, Pension, MMF, Non-bank |
| BLF_EUROPE_FISCAL_DIVERGENCE | 25 | France, Italy, Germany, ECB |

Edge 방향:
- **supports**: 939 (93%)
- **challenges**: 58 (6%)
- **기타**: 19 (1%)

---

## 4. 데이터 품질

### 출처
- **고유 source 수**: 588개
- 1차 데이터: IMF, BIS, Fed, ECB, BOJ, NBS, BLS, Census, FDIC 등 주요 통계기관
- 시장 데이터: Bloomberg, ICE, CME, Markit 등
- 리서치: Goldman Sachs, JPMorgan, McKinsey, Preqin 등
- 학술/국제기구: NBER, OECD, UN, World Bank 등

### 날짜 범위
- 대부분 2024-2025년 데이터
- 일부 구조적 트렌드는 2023년 데이터 포함

### ID 체계
- `FACT_{도메인약어}_{주제}_{세부}` 형식
- 배치별 prefix: FACT_CL_, FACT_MF_, FACT_INTL_, FACT_STR_, FACT_IE_, FACT_EM_, FACT_JP_, FACT_CN_, FACT_OIL_, FACT_VOL_, FACT_FI_, FACT_FED_, FACT_GEO_, FACT_CROSS_, FACT_AUTO_, FACT_INS_, FACT_LONG_, FACT_WATER_, FACT_CLIMATE_, FACT_GREEN_, FACT_FISC_, FACT_LABOR_, FACT_CONSUMER_, FACT_RE_, FACT_EQ_, FACT_TECH_, FACT_EU_, FACT_CBDC_, FACT_STRUCT_, FACT_MISC_, FACT_BLF_

---

## 5. 수정 이력

| 문제 | 원인 | 수정 |
|------|------|------|
| `FACT_STR_CBDC_DIGITAL_EURO` edge 에러 | structural batch에서 ID 불일치 | → `FACT_STR_CBDC_ECB_DIGITAL_EURO` |
| `FACT_MULTIP_OLAR_ORDER_TRANSITION` edge 에러 | 기존 edge 파일 오타 | → `FACT_MULTIPOLAR_ORDER_TRANSITION` |
| `BLF_AI_CREDIT_TECTONIC` 등 4개 belief ID | 실제 belief ID와 다른 이름 사용 | → `BLF_AI_CREDIT` 등으로 일괄 치환 (46건) |
| `BLF_AUTOMATION_ACCELERATION` 등 3개 belief ID | 실제 belief ID와 다른 이름 사용 | → `BLF_AUTOMATION_LABOR_CLIFF` 등으로 일괄 치환 (13건) |

모든 수정 후 `status` 명령어에서 edge 에러 **0건** 확인.

---

## 6. 생성 방법론

### 도구
- `_gen_facts.py`: 공통 헬퍼 (`make_fact`, `make_edge`, `write_batch`)
- 도메인별 `_gen_{domain}.py`: 팩트를 튜플 리스트로 정의 → 헬퍼로 YAML 변환

### 접근
1. **도메인 분할**: 40개 belief의 도메인을 16개 클러스터로 그룹핑
2. **Deep dive 전략**: 1차 배치 후 부족한 영역에 2차(credit2, macro2, impl_ev2) 추가
3. **Cross-domain**: implication hypothesis evidence, 도메인 간 연결 팩트 별도 생성
4. **Gap-fill**: 최종 4개 배치로 커버리지 미달 belief에 추가 evidence

### 파일 구조
```
graph/nodes/episodic/2026-02/batch_{name}.yaml   ← 팩트 노드
graph/edges/batch_{name}.yaml                     ← 팩트→belief edge
```

---

## 7. 미커버 Belief (6개)

다음 belief에는 이번 배치에서 직접 edge가 연결되지 않음 (기존 evidence로 커버):

- `BLF_CARRY_TRADE_STRUCTURAL_SHIFT`
- `BLF_CORPORATE_GOVERNANCE_REFORM`
- `BLF_DEBT_MONETIZATION_NORM`
- `BLF_PASSIVE_CONCENTRATION`
- `BLF_PRIVATE_CREDIT_BUBBLE`
- `BLF_YIELD_CURVE_CONTROL_GLOBAL`

이들은 기존 사이클(001-013)에서 이미 evidence가 있거나, 다른 belief의 evidence가 간접적으로 커버.

---

## 8. 삭제 대상 임시 파일

```
_gen_facts.py, _gen_credit.py, _gen_macro.py, _gen_intl.py, _gen_structural.py,
_gen_impl_ev.py, _gen_credit2.py, _gen_macro2.py, _gen_impl_ev2.py, _gen_extra.py,
_gen_em.py, _gen_japan.py, _gen_china.py, _gen_commodities.py, _gen_microstructure.py,
_gen_fi.py, _gen_fed_liquidity.py, _gen_geopolitics.py, _gen_cross_domain.py,
_gen_final1.py, _gen_final2.py, _gen_final3.py, _gen_final4.py
```

총 23개 파일. 이 보고서 작성 후 삭제.
