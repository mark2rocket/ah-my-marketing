# /amm-campaign-plan — 마케팅 캠페인 기획

## 트리거

`/amm-campaign-plan` 또는 "캠페인 기획", "마케팅 플래닝", "캠페인 플랜"

## 역할

LMF 캠페인 실행 전 전체 마케팅 전략을 수립한다.
타겟·예산·KPI·스프린트 계획을 정의하여 이후 LMF 탐색의 방향을 잡는다.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 확인)
**Post:** GATE-5 (산출물 저장 확인)

## 독립 실행

선행 스킬 없이 독립 실행 가능. 신규 클라이언트의 첫 번째 스킬.
기존 클라이언트라면 lmf-learnings.md와 이전 campaign-log.md를 Phase 0에서 자동 로드한다.

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 캠페인을 기획할까요? (클라이언트명)"
2. "제품/서비스를 한 줄로 설명해주세요."
3. "이번 캠페인의 마케팅 목표는? (ACQ 신규획득 / RET 리텐션 / REV 매출 / BRD 브랜딩)"
4. "총 예산과 집행 기간을 알려주세요. (예: 500만원 / 1개월)"
5. "주요 타겟을 알려주세요. (성별, 연령대, 직업, 관심사 등 자유롭게)"
6. "가장 중요한 지표(Primary Metric)는? (CPI / CPA / ROAS / CTR / LD 등)"
7. "참고할 경쟁 브랜드나 레퍼런스 광고가 있나요? (없으면 '없음')"

답변 확인 후 → Phase 0으로 진행.

## 실행 흐름

### Phase 0 — 이전 인사이트 로드

```
1. docs/lmf-learnings.md 읽기 (있으면)
   → 이전 winner 소구점, 반복 실패 패턴 파악

2. docs/clients/{name}/campaign-log.md 읽기 (있으면)
   → 직전 캠페인 결과 · 검증된 가설 파악

3. docs/clients/{name}/brand-guide.md 읽기 (있으면)
   → 브랜드 톤, 제약 조건 파악
```

파일이 없으면: "첫 번째 캠페인 기획입니다. 제로에서 시작합니다."

### Phase 1 — 캠페인 브리프 작성

MKT목표·CPG목표 코드 확정 후 아래를 정의한다:

| 항목 | 내용 |
|------|------|
| MKT목표 | ACQ / RET / REV / BRD |
| CPG목표 | AC / CON / TR / LD / AW |
| 총 예산 | |
| 집행 기간 | |
| 스프린트 수 | S01~S0N |

### Phase 2 — 타겟 페르소나 초안

주요 페르소나 1~3개를 아래 형식으로 정의한다:

| 코드 | 누구인가 | 핵심 Pain Point | 현재 대안 | Why 우리? |
|------|---------|----------------|---------|---------|
| P1 | | | | |
| P2 | | | | |

> 이 페르소나는 `/amm-copy-lmf` Phase 1에서 구체화된다.

### Phase 3 — KPI 및 성공 기준

| 구분 | 지표 | 목표값 | 비고 |
|------|------|--------|------|
| Primary | | | LMF를 찾았을 때 변하는 지표 |
| Secondary | | | 실패 원인 파악용 |
| Guardrail | | | 절대 망가지면 안 되는 지표 |

캠페인 off 기준: (예: CPA가 목표의 2배 초과 시 3일 이상 지속되면 pause)

### Phase 4 — 스프린트 계획

| 스프린트 | 기간 | 목표 | 예산 배분 | 검증할 핵심 가설 |
|---------|------|------|---------|---------------|
| S01 | | LMF 탐색 (발산) | | |
| S02 | | winner 소구점 확장 | | |
| S03 | | 고도화 / 스케일업 | | |

## 산출물

```
output/{name}/plan/{YYMMDD}-campaign-plan/
└── campaign-plan.md
```

## 다음 단계

→ `/amm-copy-lmf` — 페르소나 기반 소구점·카피 발굴 (LMF 4단계)

## 참고

→ `docs/clients/{name}/brand-guide.md` (브랜드 제약)
→ `docs/lmf-learnings.md` (누적 인사이트)
→ `docs/taxonomy-reference.md` (MKT목표·CPG목표 코드표)
