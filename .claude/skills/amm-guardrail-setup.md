# /amm-guardrail-setup — 캠페인 가드레일 설정

## 트리거

`/amm-guardrail-setup` 또는 "가드레일 설정", "경고 기준 설정", "캠페인 가드레일"

## 역할

캠페인별 Primary·Secondary·Guardrail 메트릭과 임계값을 정의한다.
`guardrails.md`에 저장하며, 이후 `/amm-ad-optimize` 실행 시 자동으로 로드·대조한다.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-0 (브랜드·제품 확인) → GATE-1 (클라이언트 확인)
**Post:** GATE-5 (산출물 저장 확인)

## 독립 실행

`/amm-meta-setup` 완료 후 실행 권장. 캠페인 집행 시작 전에 반드시 설정.

## Sprint Contract

스킬 실행 전 **AskUserQuestion 툴**로 순서대로 질문한다:

1. "어떤 클라이언트의 가드레일을 설정할까요? (클라이언트명)"
2. "광고 계정명을 알려주세요. (예: main / kr-performance / default)"
3. "가드레일을 설정할 캠페인명을 알려주세요. (택소노미 기준)"
4. "캠페인 최적화 목표는 무엇인가요? (CPA / ROAS / CTR / CPI / CPL)"
5. "Primary Metric 임계값을 설정해주세요.
   예) CPA: 15000원 초과 시 경고 / ROAS: 2.0 미만 시 경고"
6. "Guardrail Metric(절대 망가지면 안 되는 지표)을 설정해주세요.
   예) 일 예산 소진율 80% 초과 / CTR 0.3% 미만"
7. "캠페인 자동 off 기준을 설정해주세요.
   예) CPA 30,000원 초과 3일 연속 / ROAS 1.0 미만"

답변 확인 후 → Phase 0 진행.

## 실행 흐름

### Phase 0 — 기본값 로드

```
1. .amm/config.json 읽기
   → default_guardrails 로드 (CPA 한도, CTR 최소값, ROAS 최소값)
   → 없으면: "/amm-setup을 먼저 실행해주세요." → 중단

2. docs/clients/{name}/accounts/{account}/guardrails.md 읽기
   → 있으면: 기존 설정 출력 후 "덮어쓸까요? 업데이트할까요?" AskUserQuestion
   → 없으면: 신규 생성
```

### Phase 1 — 가드레일 명세 작성

Sprint Contract 답변 기반으로 아래 형식으로 정리한다:

```markdown
## 메트릭 정의

| 구분 | 메트릭 | 임계값 | 조치 |
|------|--------|--------|------|
| Primary | CPA | > {금액}원 | 경고 메일 발송 + 소재 교체 검토 |
| Secondary | CTR | < {수치}% | 소재 리뷰 트리거 |
| Guardrail | 일 예산 소진율 | > 80% | 즉시 경고 + 집행 일시중단 검토 |

## 자동 off 기준

- {기준 1}: {N}일 연속 유지 시 캠페인 일시중단 권고
- {기준 2}: 단일 발생 시 즉시 경고

## 최소 운영 기간

- 데이터 수집 최소 기간: {N}일 (임계값 판단 전 유예 기간)
```

### Phase 2 — 파일 저장

```
docs/clients/{name}/accounts/{account}/guardrails.md
```

저장 완료 후 출력:

```
✅ guardrails.md 저장 완료
   경로: docs/clients/{name}/accounts/{account}/guardrails.md

설정된 가드레일:
  Primary:   CPA > {금액}원 → 경고
  Secondary: CTR < {수치}% → 소재 리뷰
  Guardrail: 일 예산 소진율 > 80% → 즉시 경고
  Auto-off:  {기준}

→ /amm-ad-optimize 실행 시 이 가드레일이 자동으로 적용됩니다.
→ 경고 메일 발송을 활성화하려면 Gmail MCP를 연결해주세요.
```

## 산출물

```
docs/clients/{name}/accounts/{account}/
└── guardrails.md   ← 캠페인별 가드레일 정의
```

## 다음 단계

→ `/amm-ad-optimize` — 집행 중 성과 최적화 (가드레일 자동 대조)
→ `/amm-meta-setup` — 광고 세팅 명세서 작성

## 참고

→ `.amm/config.json` (기본 가드레일 임계값)
→ `docs/clients/{name}/accounts/{account}/campaign-log.md` (이전 캠페인 CPA·CTR 기준값)
→ `.claude/skills/amm-ad-optimize.md` (가드레일 적용 시점)
