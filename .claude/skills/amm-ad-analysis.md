# /amm-ad-analysis — 광고 결과 분석

## 트리거

`/amm-ad-analysis` 또는 "결과 분석", "성과 분석", "캠페인 리뷰"

## 역할

메타 광고 캠페인 결과를 LMF 관점에서 분석하고,
winner 소구점·배운 점·다음 가설을 정리하여 Compounding 루프를 완성한다.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 확인)
**Post:** GATE-6 (lmf-learnings.md + campaign-log.md 업데이트 확인)

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 캠페인을 분석할까요? (클라이언트명)"
2. "분석할 캠페인명을 알려주세요. (택소노미 기준 전체 이름, 예: ACQ_AC_MYSVC_S01_260419)"
3. "분석 기간은 언제부터 언제까지인가요?"
4. "성과 데이터를 어떻게 제공하실 건가요? (직접 입력 / 파일 첨부 / 캡처 이미지)"
5. "이번 캠페인의 Primary Metric은 무엇인가요? (CPI / CPA / CTR / ROAS 등)"

답변 확인 후 → Phase 0 (누적 패턴 로드)으로 진행.

## 실행 흐름

### Phase 0 — 누적 패턴 로드 (Compounding)

**분석 시작 전 반드시 수행. 이전 캠페인과 비교해야 배움이 생긴다.**

```
1. docs/lmf-learnings.md 읽기
   → 전체 클라이언트 누적 인사이트 파악
   → "이번 결과가 전체 패턴과 일치하는가? 다른가?"

2. docs/clients/{name}/campaign-log.md 읽기
   → 이전 캠페인 winner/실패 패턴
   → ICE 예측 정확도 추세

3. 요약 출력:
   "총 {N}회 캠페인 데이터가 누적되어 있습니다.
    이전 winner: VP{N} ({소구점}).
    이번 분석과 비교하겠습니다."
```

### Phase 1 — 컨텍스트 로드

```
1. docs/clients/{name}/campaign-log.md 읽기 (이전 캠페인 패턴 파악)
2. docs/lmf-learnings.md 읽기 (전체 누적 인사이트)
3. docs/clients/{name}/taxonomy.md 읽기 (VP 코드 매핑)
4. output/{name}/copy/ice-priority-*.md 읽기 (기존 ICE 예측값)
```

### Phase 2 — 데이터 수집

사용자에게 아래 데이터 요청:

```
필수:
- 캠페인 기간 / 총 지출
- 소구점(VP)별 성과:
  | VP코드 | 노출수 | 클릭수 | CTR | CPC | 전환수 | CPA/CPI |
  |--------|--------|--------|-----|-----|--------|---------|

선택:
- 광고세트별 타겟 성과
- 소재 포맷별 성과 (이미지 vs 영상)
- 시간대별 성과
```

### Phase 3 — LMF 관점 분석

#### 3-1. Winner 소구점 판별

```
기준: Primary Metric 기준 상위 소구점 선정
판별 방법:
- CTR이 가장 높은 VP → 고객이 가장 공감한 언어
- CPA/CPI가 가장 낮은 VP → 실제 전환까지 이어진 언어
- 두 지표 모두 좋은 VP → 진짜 LMF 후보
```

#### 3-2. 실패 소구점 원인 분석

```
CTR은 높은데 CPA 높음 → 랜딩 페이지 미스매치
CTR도 낮고 CPA도 높음 → 소구점 자체가 미스
CTR은 낮은데 CPA 낮음 → 특정 타겟에서만 작동
```

#### 3-3. ICE 예측 vs 실제 대조

이전 ICE 예측값과 실제 성과를 비교하여 판단 기준 고도화:

| VP코드 | ICE 예측 | 실제 CTR | 실제 CPA | 예측 정확도 |
|--------|---------|---------|---------|-----------|

### Phase 4 — 배움 정리 및 다음 가설

```
배운 것:
- Winner 소구점이 잘 된 이유
- 실패 소구점의 공통 패턴
- 예상과 달랐던 점

다음 가설:
- winner 언어를 어떻게 변형/확장할 것인가?
- 새로운 퍼소나/VP 조합은?
- 소재 포맷 변경으로 성과 개선 여지는?
```

### Phase 5 — Compounding 업데이트

아래 두 파일을 자동 업데이트:

**1. `docs/clients/{name}/campaign-log.md` 업데이트:**
```markdown
### [{날짜}] {캠페인명}
Winner: VP{N} — {소구점}
Primary Metric: {결과}
잘 된 것 / 안 된 것 / 다음 가설
```

**2. `docs/lmf-learnings.md` 업데이트:**
```markdown
## [{날짜}] {클라이언트명} — {캠페인명}
winner 소구점 / 지표 / 배운 것 / 다음 가설 / 고객 언어 발굴
```

## 산출물

```
output/{name}/analysis/{YYMMDD}-analysis/
└── analysis.md           — 상세 분석 리포트
```

- `docs/clients/{name}/campaign-log.md` 업데이트 (append)
- `docs/lmf-learnings.md` 업데이트 (append)

## 다음 단계

→ `/amm-campaign-plan` — 다음 스프린트 기획 (Compounding 루프 재시작)

## 참고

→ `docs/clients/{name}/campaign-log.md` (이전 캠페인 패턴)
→ `docs/lmf-learnings.md` (전체 누적 인사이트)
→ `.claude/skills/amm-copy-lmf.md` (다음 LMF 사이클 시작)
