# /amm-competitor-scan — 경쟁사 광고·메시지 분석

## 트리거

`/amm-competitor-scan` 또는 "경쟁사 분석", "경쟁 광고 분석", "시장 메시지 스캔"

## 역할

경쟁사 광고·랜딩페이지·SNS 카피를 분석하여 시장에서 통용되는 소구점 패턴을 파악한다.
LMF 탐색 전 시장 언어 지형도를 그리고, 차별화 포인트를 발굴한다.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 확인)
**Post:** GATE-5 (산출물 저장 확인)

## 독립 실행

선행 스킬 없이 독립 실행 가능. `/amm-campaign-plan` 이전 또는 이후 모두 유효.

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 경쟁사를 분석할까요? (클라이언트명)"
2. "광고 계정명을 알려주세요. (예: main / kr-performance / default)"
3. "분석할 경쟁사 브랜드를 알려주세요. (2~5개 권장)"
4. "경쟁사의 광고나 랜딩페이지 URL 또는 캡처 이미지가 있나요? (있으면 제공, 없으면 '없음')"
5. "어떤 채널의 메시지를 주로 분석할까요? (메타 광고 / 랜딩페이지 / SNS / 전체)"
6. "특히 파악하고 싶은 것은? (소구점 패턴 / 카피 톤앤매너 / 가격 전략 / CTA 방식)"

답변 확인 후 → Phase 0으로 진행.

## 실행 흐름

### Phase 0 — 선행 인사이트 로드

```
0. (GATE-0) docs/clients/{name}/brand-guide.md 읽기
   → 없으면: "/amm-brand-setup을 먼저 실행해주세요." → 중단
   → 제품 1개: 자동 선택, 이후 전체 진행에서 이 제품 정보 사용
   → 제품 2~3개: AskUserQuestion으로 선택 요청
     "이번에 광고할 제품/서비스를 선택해주세요: P01 {제품명} / P02 {제품명}"
   → 제품 4개 이상: products/ 폴더 목록 출력 → AskUserQuestion으로 선택 → P{N}.md 로드

1. docs/clients/{name}/accounts/{account}/lmf-learnings.md 읽기 (있으면)
   → 찾으면: 이전 winner 소구점·고객 언어 로드 → 경쟁사 소구점과 중복 여부 확인용
   → 없으면: "누적 LMF 데이터 없음. 차별화 분석은 경쟁사 데이터만으로 진행합니다."

2. docs/clients/{name}/brand-guide.md 읽기 (있으면)
   → 브랜드 포지셔닝·톤 파악

3. 로드 결과 출력:
   "lmf-learnings.md: ✅/❌ / brand-guide.md: ✅/❌ → Phase 1 진행"
```

### Phase 1 — 경쟁사 메시지 수집

**경쟁사 2개 이상이면 독립 executor agent로 동시에 병렬 스캔.**
경쟁사 1개이면 직접 실행.

```
경쟁사별 병렬 Agent 실행 (모두 동시에 발사):

Task(subagent_type="oh-my-claudecode:executor", model="sonnet",
  prompt="
  역할: 경쟁사 광고·메시지 분석 전문가

  분석 대상: {경쟁사명}
  제공 정보: {URL / 이미지 경로 / 사용자 기술 텍스트}
  분석 채널: {메타 광고 / 랜딩페이지 / SNS / 전체}
  분석 관점: {소구점 패턴 / 카피 톤앤매너 / 가격 전략 / CTA 방식}

  URL 제공 시: 브라우저 MCP로 페이지 텍스트 추출 후 분석
  이미지 제공 시: 이미지에서 카피 텍스트 추출 후 분석
  없는 경우: 제공된 텍스트 기반으로 분석

  출력 형식 (아래 구조 그대로):
  ## {경쟁사명} 분석
  - 주력 소구점: (PAIN / SOC / BENE / STAT / EMO 중 분류)
  - 카피 훅 유형:
  - 타겟 페르소나 추정:
  - 강점:
  - 약점:
  - 주목할 카피 원문: (그대로 쓸 수 있는 고객 언어)
  ")

→ A사 / B사 / C사 동시 실행
→ 전체 완료 후 결과 취합 → competitor-messages.md에 경쟁사별 섹션으로 통합 저장
```

### Phase 2 — 소구점 패턴 분석

경쟁사별 메시지를 LMF 소구점 관점으로 분류:

| 경쟁사 | 주력 소구점 | 카피훅 유형 | 타겟 페르소나 추정 | 강점 | 약점 |
|--------|-----------|-----------|----------------|------|------|
| A사 | | PAIN / SOC / BENE | | | |
| B사 | | | | | |

### Phase 3 — 시장 언어 지형도

```
포화된 소구점 (많은 경쟁사가 이미 사용):
- ...

미개척 소구점 (경쟁사가 아직 없는 영역):
- ...

우리 클라이언트의 차별화 가능 포인트:
- ...
```

### Phase 4 — LMF 인사이트 도출

```
## 경쟁사 분석 인사이트

### 시장에서 검증된 고객 언어
- "..." (A사 사용 중, 효과 추정)

### 차별화 소구점 후보
- VP 후보 1: (경쟁사가 비어있는 영역)
- VP 후보 2:

### 피해야 할 표현 (포화 상태)
- ...

### 다음 /amm-copy-lmf에서 검증할 가설
- 가설 1: "[차별화 소구점]을 소구하면 [포화 시장]에서 돋보일 것이다"
```

## 산출물

```
output/{name}/{account}/research/{YYMMDD}-competitor-scan/
├── competitor-messages.md   — 경쟁사별 메시지 원문 정리
└── scan-insights.md         — 소구점 패턴 + 차별화 기회
```

## 다음 단계

→ `/amm-copy-lmf` — 차별화 소구점 후보를 LMF 가설로 발전
→ `/amm-user-interview` — 차별화 포인트를 페르소나에게 검증

## 참고

→ `docs/clients/{name}/brand-guide.md`
→ `docs/clients/{name}/accounts/{account}/lmf-learnings.md` (이전 winner 소구점과 중복 여부 확인)
→ `.claude/rules/meta-ads.md` (LMF 카피 방향)
