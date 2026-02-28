# Implication Generator — Layer 2 Meta-Belief System

## 1. 필요성: 팩트 나열의 한계

### 사이클만 돌리면 생기는 것
- FACT 노드 축적 (1,500+ 개)
- 개별 belief의 confidence 갱신
- 도메인 내 심화 (depth)

### 사이클만 돌려서는 안 되는 것

**도메인 간 교차 정보가 생성되지 않는다.**

크레딧 애널리스트는 AI capex 채권 발행량을 안다. 에너지 애널리스트는 데이터센터 전력 소비를 안다. 하지만 "AI capex가 에너지 전환 투자를 구축(crowding out)한다"는 **어느 쪽 도메인에도 속하지 않는 정보**다. 이 정보는 두 belief이 같은 fact를 공유한다는 **그래프 구조** 자체에 묻혀 있으며, 명시적으로 추출하지 않으면 영원히 잠재 상태로 남는다.

개별 belief은 **관찰**이다. belief 쌍의 교차점은 **정보**다.

1,500개 fact를 가지고 있어도 belief 간 관계를 질문하지 않으면, 지식 그래프는 정리된 스크랩북에 불과하다. 사이클을 1,000번 돌려도 같은 도메인 안에서 fact만 늘어난다. 외연이 확장되지 않는다.

---

## 2. 경과: 3Q 프레임워크

### 구조

```
Layer 0: FACT (관찰된 데이터)
Layer 1: FACT → BELIEF (bottom-up 귀납)
Layer 2: BELIEF × BELIEF → META-BELIEF (횡적 추론) ← 이것
Layer 3+: 금지 (FACT 앵커 없는 순수 논리 체인 차단)
```

### 프로세스

```
SCAN    belief 쌍 중 1-hop 이웃을 공유하는 쌍 탐색
        (공유 노드 수 오름차순 — 먼 도메인 쌍이 더 날카로운 정보 생성)

SELECT  공유 노드를 divergent 우선, degree 내림차순으로 정렬
        상위 5개만 LLM 프롬프트에 투입 (cap)

GENERATE  3개 질문 동시 생성:

  Q1 함의    "둘 다 참이면, 새로 뭘 아는가?"
             → 어느 belief도 단독으로 말하지 않는 것을 추출
             → 새 hypothesis 노드 생성

  Q2 부정    "둘 다 참이면, 뭐가 거짓이어야 하는가?"
             → 시장 컨센서스 중 깨지는 내러티브 특정
             → contrarian hypothesis 노드 생성

  Q3 시간검증  "6개월 전 이걸 알았으면 뭐가 달라야 했나?"
             → unpriced: 트레이드 가능했는데 아무도 안 함 (행동 대상)
             → priced_in: 이미 시장 반영 (참고만)
             → trivial: 알아도 포지션 불가 (버림)

CLASSIFY  Q3 결과로 priority 자동 분류

WRITE   hypothesis(quarantine) + question(observable test) YAML 자동 저장

VERIFY  후속 사이클에서 fact 증거를 붙여 confidence 갱신
        evidence 축적 → quarantine → active 승격 또는 invalidate
```

### 안전장치

- **Layer 3 차단**: source_beliefs만 있고 직접 FACT 이웃이 없는 순수 meta-belief은 입력에서 제외. 논리의 논리를 만드는 재귀 방지.
- **Quarantine 진입**: 생성 즉시 quarantine. evidence가 붙어야만 active로 승격. 근거 없는 추론이 belief으로 둔갑하는 것 차단.
- **Decay 전파**: source belief의 confidence가 하락하면, 연결된 implication이 자동으로 재검증 대상에 올라감.

---

## 3. 기대효과

### 3.1 정보 창출 — 추가 데이터 없이

기존 knowledge graph의 **구조**에서 새 정보를 추출한다. 새 fact를 수집하지 않아도 belief 조합만으로 가설이 생성된다. 32쌍에서 64개 hypothesis + 32개 검증 질문이 나왔다. 입력 데이터 제로, 출력 정보 96개.

### 3.2 Cross-Domain 사각지대 제거

| 기존 사이클 | Implication |
|------------|-------------|
| AI 도메인 안에서 AI fact 축적 | AI × 기후 → "GPU 에너지 기생" |
| 크레딧 안에서 스프레드 추적 | 크레딧 × 인구 → "연금 buyout이 shadow banking 리스크" |
| 유동성 안에서 reserve 모니터링 | 유동성 × 규제 → "규제 후퇴가 NBFI 레버리지 확대 허용" |

도메인 전문가는 자기 도메인의 fact는 잘 알지만, **다른 도메인 belief과의 교차 함의**는 구조적으로 못 본다. 이 시스템은 그 교차점을 강제로 질문한다.

### 3.3 컨센서스 오류 구조적 탐색

Q2는 "시장이 믿고 있는 것 중 뭐가 틀려야 하나"를 명시적으로 묻는다. 이것은 투자에서 가장 가치 있고 가장 찾기 어려운 것이다.

- "AI 투자가 결국 회수된다"가 거짓 → AI 크레딧 버블 테제
- "연금 개혁이 제때 된다"가 거짓 → 장기 국채 숏 테제
- "GFC 이후 규제가 시스템을 안전하게 했다"가 거짓 → NBFI 테일 리스크 테제

각각이 포지션으로 직접 전환 가능한 contrarian thesis다.

### 3.4 행동 우선순위 자동화

Q3 time-test 결과:
- **52개 unpriced** — 6개월 전 알았으면 돈 벌었는데 아직 시장이 안 봄. 지금 행동 대상.
- **10개 priced_in** — 이미 반영. 참고만.
- **2개 trivial** — 트레이드 불가. 무시.

64개 hypothesis를 동등하게 검토하는 대신, 52개에 집중하면 된다. 시간 배분 효율 4배.

### 3.5 자기강화 루프

```
belief 추가 → 새 implication 쌍 발생 → 3Q 생성 → hypothesis 생성
→ fact 검증 → confidence 상승 → active 승격 → 새 belief로서 다시 쌍 탐색
```

knowledge graph가 커질수록 가능한 belief 쌍이 조합론적으로 증가한다. n개 belief에서 n(n-1)/2 쌍. 39개 belief → 741쌍 가능, 그 중 32쌍이 유의미한 공유 노드 보유. belief이 50개가 되면 쌍 후보는 1,225개로 늘어난다.

---

## 4. 현재 상태 (2026-02-23)

| 지표 | 수량 |
|------|------|
| 처리된 belief 쌍 | 32 |
| 생성된 hypothesis | 64 (implication 32 + negation 32) |
| 생성된 검증 질문 | 32 |
| Unpriced (행동 대상) | 52 |
| 검증 완료 (evidence 보유) | 0 |
| Quarantine | 64 |
| Active 승격 | 0 |

다음 단계: evidence 부착 → confidence 갱신 → 승격/기각 판단.
