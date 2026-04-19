# /amm-strategy-panel — 마케팅 전략 전문가 패널

## 트리거

`/amm-strategy-panel` 또는 "전략 패널", "마케팅 전문가", "캠페인 전략 검토", "소구점 검토"

## 역할

캠페인 기획 직전 또는 소구점 선정 전, 6명의 마케팅 전문가가 근거 기반으로 전략을 검토한다.
"업계 상식"과 "검증된 전략"을 명확히 구분하여 Confidence 높은 소구점을 선택한다.

expert-panel v3.1 구조 기반. 마케팅 도메인 특화 버전.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-0 (브랜드·제품 확인) → GATE-1 (클라이언트 확인)
**Post:** GATE-5 (산출물 저장 확인)

## 독립 실행

선행 스킬 없이 독립 실행 가능.
`/amm-campaign-plan` 또는 `/amm-copy-lmf` 이후 실행 시 가장 효과적.

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 순서대로 질문:

1. "어떤 클라이언트의 전략을 검토할까요? (클라이언트명)"
2. "광고 계정명을 알려주세요. (예: main / kr-performance / default)"
3. "검토할 캠페인 전략 또는 소구점 가설을 알려주세요. (자유 기술 또는 VP 코드)"
4. "현재 가장 확신이 없는 결정은 무엇인가요? (예: 타겟 선택 / 소구점 방향 / 예산 배분)"
5. "빠른 검토(--quick)와 전체 검토 중 선택해주세요."

`--quick` 선택 시: Phase 1~2(학문 지도·근거 탐색) 생략, Phase 3(컨텍스트)부터 시작.

답변 확인 후 → Phase 0으로 진행.

## 실행 흐름

### Phase 0 — 선행 산출물 로드

```
0. (GATE-0) docs/clients/{name}/brand-guide.md 읽기
   → 없으면: "/amm-brand-setup을 먼저 실행해주세요." → 중단
   → 제품 1개: 자동 선택
   → 제품 2개 이상: AskUserQuestion으로 선택

1. output/{name}/{account}/plan/ 날짜순 최신 campaign-plan.md 자동 발견
   → 찾으면: 캠페인 목표·KPI·타겟·소구점 가설 로드

2. output/{name}/{account}/copy/ 날짜순 최신 copy-final.md 또는 ice-priority.md 자동 발견
   → 찾으면: 기존 ICE 채점 + 확정 소구점 로드 → 패널 토론의 출발점으로 사용

3. docs/clients/{name}/accounts/{account}/lmf-learnings.md 읽기
   → 이전 winner/loser 소구점 패턴 확인

4. 로드 결과 출력:
   "campaign-plan.md: ✅/❌ / copy-final.md: ✅/❌ / lmf-learnings.md: ✅/❌
    → 패널 검토 맥락 준비 완료"
```

---

### Phase 1 — 학문 지도 (--quick 생략)

검토할 전략과 관련된 마케팅 학문 분야 2~4개를 식별한다.

```markdown
## 학문 지도

| 분야 | 관련성 | 핵심 연구 주제 | 근거 풍부도 |
|------|--------|--------------|-----------|
| Consumer Behavior | | | |
| Behavioral Economics | | | |
| Marketing Science | | | |

**주 근거원**: [선택한 1~2개 분야와 이유]
```

> ✓ Gate: 분야 2개 이상 식별 + 주 근거원 선택 완료.

---

### Phase 2 — 근거 지형 탐색 (--quick 생략)

주 근거원에서 이 전략 유형의 효과에 대한 핵심 발견을 정리한다.

근거 수준 태그:
- `[Strong]` — 메타분석, 반복 재현된 실험
- `[Moderate]` — 복수의 업계 사례 또는 대규모 관찰 연구
- `[Emerging]` — 단일 연구, 예비 결과
- `[Expert]` — 전문가 합의, 경험 기반 (근거 약함 — 주의)
- `[Contested]` — 상반된 결과 존재

```markdown
## 근거 지형

### 설명적 발견 (이렇게 작동한다)
- [Strong/Moderate] 발견 내용 — 출처

### 처방적 발견 (이렇게 하면 효과적이다)
- [Strong/Moderate] 발견 내용 — 출처

### 업계 통념 경고 (근거 없이 퍼진 상식)
- [Contested/Expert] 흔히 믿지만 근거가 약하거나 상황에 따라 다른 것들
```

> ✓ Gate: 설명적·처방적 발견 각 2개 이상 + 근거 수준 태그 완료.

---

### Phase 3 — 컨텍스트 수집 (AskUserQuestion 필수)

**반드시 AskUserQuestion 도구로** 사용자 맥락을 수집한다.

```
AskUserQuestion(
  question: "현재 가장 고민되는 전략적 결정은 무엇인가요?",
  header: "전략 검토 초점",
  options: [
    { label: "소구점 선택", description: "여러 소구점 중 어디에 집중할지 모르겠다" },
    { label: "타겟 설정", description: "타겟을 넓힐지 좁힐지 판단이 안 된다" },
    { label: "예산 배분", description: "소구점별 예산을 어떻게 나눌지 모르겠다" },
    { label: "메시지 방향", description: "Pain vs Gain 중 어느 방향이 맞는지 모르겠다" }
  ]
)
```

> ✓ Gate: 사용자의 핵심 고민 + 현재 상황 파악 완료.

---

### Phase 4 — 전문가 패널 토론

> ⚠️ 이 토론은 Claude가 연구 데이터와 학습 기반으로 시뮬레이션한 것입니다. 실제 전문가 견해와 다를 수 있습니다.

#### 전문가 6인 (고정 구성)

| # | 역할 | 관점 | 핵심 기여 |
|---|------|------|----------|
| 1 | 소비자 심리학자 | Why 고객은 이 메시지에 반응하는가 | 인지·감정 프로세싱, 설득 심리 |
| 2 | 퍼포먼스 마케팅 전문가 | 실제 CTR/CPA 데이터 관점 | 광고 크리에이티브 효과, 채널 특성 |
| 3 | 행동경제학자 | 인지 편향과 의사결정 프레임 | 손실회피, 사회적 증명, 앵커링 |
| 4 | 브랜드 전략가 | 장기 자산 vs 단기 성과 균형 | 브랜드 일관성, 포지셔닝 |
| 5 | 데이터 사이언티스트 | 측정 가능성과 실험 설계 | A/B 테스트 유효성, 샘플 사이즈 |
| 6 | 비판적 실패 분석가 | 이 전략이 실패하는 시나리오 | Pre-mortem, 리스크 식별 |

#### 토론 구조 (4라운드)

**라운드 1 — 근거 기반 진단**
각 전문가가 Phase 2 근거 지형을 기반으로 현재 전략을 평가한다.
모든 주장에 근거 수준 태그 필수.

**라운드 2 — 사용자 맥락 적용**
사용자의 구체적 상황(Phase 3)에 이 근거가 어떻게 적용되는가.

**라운드 3 — 교차 검증**
전문가 간 의견 충돌 시 어떤 근거에 기반한 의견인지 명시하며 토론.

**라운드 4 — 우선순위 합의**
어떤 소구점/전략에 가장 높은 Confidence를 부여할 수 있는가.

**토론 규칙:**
- 모든 주장에 `[Strong]` / `[Moderate]` / `[Emerging]` / `[Expert]` / `[Contested]` 태그 필수
- `[Expert]` 의견은 반드시 "근거 제한적, 경험 기반 의견입니다"를 명시
- 만장일치라도 근거가 약하면 솔직하게 표기

> ✓ Gate: 6인 4라운드 완료 + 모든 주장에 근거 태그 부여.

---

### Phase 5 — 전략 권고

패널 토론 결과를 바탕으로 전략 방향을 정리한다.

```markdown
## 전략 권고

### Core 소구점 (검증 우선, Confidence 높음)
- VP{N}: {소구점} — 근거 레벨: [Strong/Moderate]
  → 이유: {패널 합의 근거}

### Peripheral 소구점 (탐색용, 검증 필요)
- VP{N}: {소구점} — 근거 레벨: [Emerging/Expert]
  → 가설: {검증해야 할 전제}
  → 주의: {[Contested] 항목이 있다면 명시}

### 기각 소구점
- VP{N}: {소구점} — 기각 이유: {근거 또는 리스크}

### 예산 배분 권고
- Core 소구점: {비율}% (검증 집중)
- Peripheral 소구점: {비율}% (탐색 병행)
```

---

### Phase 6 — 검증 가설 설계

각 Core 소구점에 대해 최소 검증 실험을 설계한다.

```markdown
## 검증 가설

### VP{N} — 최소 검증 실험
- 가설: "{소구점}을 소구하면 {타겟}의 CTR이 {현재 대비 X}% 개선될 것이다"
- 변수: {테스트할 단일 변수}
- 대조군: {현재 집행 중인 광고 또는 기본값}
- 측정 지표: {Primary Metric} / {Secondary Metric}
- 성공 기준: {구체적 수치}
- 실패 기준: {중단 트리거}
- 최소 운영 기간: {N일} (데이터 수집 충분성 기준)
```

> ✓ Gate: 가설 1개 이상 + 성공/실패 기준 명시 + 운영 기간 설정.

---

### Phase 7 — 저장

**AskUserQuestion 도구로** 저장 위치를 확인한다.

```
AskUserQuestion(
  question: "전략 패널 결과를 어디에 저장할까요?",
  header: "저장 위치",
  options: [
    { label: "산출물 폴더 저장", description: "output/{name}/{account}/plan/{YYMMDD}-strategy-panel/strategy-panel.md" },
    { label: "저장 안 함", description: "대화창 출력만 유지" }
  ]
)
```

저장 시 전체 Phase 1~6 내용을 축약 없이 저장한다.

---

## 산출물

```
output/{name}/{account}/plan/{YYMMDD}-strategy-panel/
└── strategy-panel.md   — 학문 지도 + 근거 지형 + 패널 토론 + 전략 권고 + 검증 가설 전문
```

## 다음 단계

→ `/amm-copy-lmf` — 권고된 Core 소구점으로 카피 발굴 (ICE Confidence에 `[Strong/Moderate]` 반영)
→ `/amm-campaign-plan` — 전략 권고 기반 캠페인 기획

## 참고

→ `docs/clients/{name}/brand-guide.md` (브랜드·제품 정보)
→ `docs/clients/{name}/accounts/{account}/lmf-learnings.md` (이전 소구점 성과)
→ `.claude/skills/amm-copy-lmf.md` (ICE 근거 레벨 태그 기준)
→ `.claude/rules/meta-ads.md` (메타 광고 전략 원칙)
