# /amm-meta-setup — 메타 광고 세팅 파이프라인

## 트리거

`/amm-meta-setup` 또는 "광고 세팅", "캠페인 설계", "메타 광고 세팅"

## 역할

메타 광고 캠페인 → 광고세트 → 광고 전체 구조를 설계하고,
택소노미에 맞는 세팅 명세서를 생성한다.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 확인) → GATE-2 (택소노미 선행 확인)
**Post:** GATE-5 (산출물 저장 확인)

## 선행 조건

- `/amm-taxonomy` 완료 → `docs/clients/{name}/accounts/{account}/taxonomy.md` 존재
- `/amm-copy-lmf` 완료 → VP 코드 + 카피 확정
- `/amm-ad-creative` 완료 → 소재 파일명 확정 (세팅 명세서에 기재)

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 메타 광고를 세팅할까요? (클라이언트명)"
2. "광고 계정명을 알려주세요. (예: main / kr-performance / default)"
3. "캠페인 최적화 목표를 선택해주세요. (AC 앱설치 / CON 전환 / TR 트래픽 / LD 리드)"
4. "총 예산과 집행 기간을 알려주세요. (예: 300만원 / 2주)"
5. "타겟 오디언스 조건을 알려주세요. (성별, 연령대, 주요 관심사 또는 타겟 유형 LA·REM·INT)"
6. "이번 스프린트에서 테스트할 소구점(VP) 개수는 몇 개인가요?"
7. "주력 소재 포맷을 선택해주세요. (이미지 / 영상 / 카루셀 — 복수 선택 가능)"

답변 확인 후 → Phase 0 (선행 산출물 자동 로드)으로 진행.

## 실행 흐름

### Phase 0 — 선행 산출물 자동 로드

```
1. docs/clients/{name}/accounts/{account}/taxonomy.md 읽기 (GATE-2 통과 전제)
   → VP 코드 + 소구점 정의 + 네이밍 컨벤션 로드

2. output/{name}/{account}/copy/ 폴더에서 날짜순 최신 copy-final.md 자동 발견
   → 찾으면: VP별 확정 카피 로드 → 광고 명세서에 자동 기재
   → 없으면: 명세서 카피 항목을 "{카피 미확정 — 별도 입력 필요}"로 표시

3. output/{name}/{account}/creative/ 폴더에서 날짜순 최신 폴더 자동 발견
   → 찾으면: 소재 파일명 목록 로드 → 광고 명세서 소재 항목에 자동 기재
   → 없으면: 소재 항목을 "{소재 파일명 미확정 — /amm-ad-creative 실행 후 기재}"로 표시

4. 자동 로드 결과 출력:
   "taxonomy.md: ✅ / copy-final.md: ✅/❌ / creative 폴더: ✅/❌
    → Agent 1 진행 가능 여부 확인"
```

### Agent 1 — 전략 설계자 (Planner)

```
역할: 캠페인 구조 설계
입력: 클라이언트 컨텍스트 + 목표 + 예산
출력: 캠페인 구조도 (계층별 개수 + 예산 배분)
```

캠페인 구조 설계 원칙:
- 소구점(VP)별로 광고세트 분리 (타겟 변수 고정, 메시지만 변경)
- 또는 타겟별 광고세트 분리 (소구점 고정, 타겟만 변경)
- 한 광고세트에 3~5개 광고 권장 (메타 알고리즘 최적화)

### Agent 2 — 명세서 작성자 (Executor)

```
역할: 세팅 명세서 생성
입력: Agent 1의 구조도 + taxonomy.md
출력: output/{name}/{account}/reports/meta-setup-{YYMM}.md
```

## 산출물 형식

```markdown
# {클라이언트명} 메타 광고 세팅 명세서
기간: {시작일} ~ {종료일}
총 예산: {금액}원

## 캠페인 구조

### 캠페인 1: {캠페인명}
- 목표: {목표}
- 예산: {금액}원

#### 광고세트 1-1: {광고세트명}
- 타겟: {타겟 상세}
- 일예산: {금액}원
- 기간: {기간}

| 광고명 | 소구점 | 포맷 | 비율 | 카피 요약 |
|--------|--------|------|------|----------|
| {광고명} | VP1 | IMG | 1x1 | ... |

#### 광고세트 1-2: ...
```

## 캠페인 마스터 플랜 체크리스트

- [ ] Primary Metric 정의 (LMF를 찾았을 때 변하는 지표)
- [ ] Secondary Metric 정의 (실패 원인 파악용)
- [ ] Guardrail Metric 정의 (절대 망가지면 안 되는 지표)
- [ ] 캠페인 off 기준 설정
- [ ] 최소 운영 기간 설정 (데이터 수집 기간)

## 산출물

```
output/{name}/{account}/setup/{YYMMDD}-meta-setup/
└── meta-setup.md
```

## 다음 단계

→ `/amm-ad-optimize` — 광고 집행 중 성과 최적화

## 참고

→ `docs/clients/{name}/accounts/{account}/taxonomy.md` (소구점 코드)
→ `.claude/rules/meta-ads.md` (메타 광고 공통 규칙)
→ `.claude/skills/amm-copy-lmf.md` (카피 발굴)
