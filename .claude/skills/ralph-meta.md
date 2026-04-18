# /ralph-meta — 메타 광고 풀 파이프라인 실행

## 트리거

`/ralph-meta` 또는 "풀 파이프라인", "처음부터 끝까지", "전체 광고 프로세스"

## 역할

Paid Track 7단계를 처음부터 끝까지 순서대로 실행하는 오케스트레이션 스킬.
각 단계 완료 후 사용자 확인을 거쳐 다음 단계로 진행한다.
중간 단계부터 시작하거나 특정 단계만 건너뛸 수도 있다.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 확인)
**Post:** GATE-5 (각 단계 산출물 저장 확인)

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 풀 파이프라인을 실행할까요? (클라이언트명)"
2. "몇 단계부터 시작할까요? (1~7 선택, 또는 '처음부터')"
   ```
   1. /campaign-plan  — 캠페인 기획
   2. /copy-lmf       — LMF 소구점·카피 발굴
   3. /taxonomy       — 광고 네이밍 컨벤션 정의
   4. /ad-creative    — 광고 소재 제작
   5. /meta-setup     — 광고 세팅 명세서
   6. /ad-optimize    — 집행 중 최적화
   7. /ad-analysis    — 결과 분석
   ```
3. "건너뛸 단계가 있나요? (없으면 '없음')"

답변 확인 후 → 지정된 단계부터 순서대로 실행.

## 실행 흐름

---

### STEP 1 — /campaign-plan

```
실행: .claude/skills/campaign-plan.md 전체 흐름 수행
산출물: output/{name}/plan/{YYMMDD}-campaign-plan/campaign-plan.md
```

**체크포인트:** 캠페인 브리프·KPI·스프린트 계획이 산출물로 저장되었는가?
→ 확인 후 STEP 2 진행.

---

### STEP 2 — /copy-lmf

```
실행: .claude/skills/copy-lmf.md 전체 흐름 수행
입력: STEP 1 산출물 (페르소나·마케팅 목표) 자동 로드
산출물: output/{name}/copy/{YYMMDD}-lmf/
         ├── lmf-brief.md
         ├── copy-candidates.md
         ├── ice-priority.md
         └── copy-final.md
```

**체크포인트:** ICE 채점 완료 + copy-final.md 저장 + GATE-4 통과?
→ 확인 후 STEP 3 진행.

---

### STEP 3 — /taxonomy

```
실행: .claude/skills/taxonomy.md 전체 흐름 수행
입력: STEP 2의 VP·USP 코드 자동 로드
산출물: docs/clients/{name}/taxonomy.md
        docs/clients/{name}/taxonomy-log.csv
```

**체크포인트:** 3레벨 네이밍 구조 정의 + taxonomy-log.csv 초기화?
→ 확인 후 STEP 4 진행.

---

### STEP 4 — /ad-creative

```
실행: .claude/skills/ad-creative.md 전체 흐름 수행
입력: STEP 2 copy-final.md + STEP 3 taxonomy.md 자동 로드
산출물: output/{name}/creative/{YYMMDD}-{VP코드}/
         ├── *.html
         └── *.png
```

**체크포인트:** PNG 소재 생성 + 메타 정책 검증 통과?
→ 확인 후 STEP 5 진행.

---

### STEP 5 — /meta-setup

```
실행: .claude/skills/meta-setup.md 전체 흐름 수행
입력: STEP 3 taxonomy.md + STEP 4 소재 파일명 자동 로드
산출물: output/{name}/setup/{YYMMDD}-meta-setup/meta-setup.md
```

**체크포인트:** 캠페인→광고세트→광고 전체 명세서 완성?
→ 확인 후 광고 집행 시작. 집행 중 STEP 6으로.

---

### STEP 6 — /ad-optimize

```
실행: .claude/skills/ad-optimize.md 전체 흐름 수행
입력: STEP 5 세팅 명세서 + 실시간 성과 데이터 (사용자 제공)
산출물: output/{name}/optimize/{YYMMDD}-optimize/optimize-plan.md
```

> 집행 기간 중 복수 실행 가능. 최적화 회차마다 폴더 날짜가 달라진다.

**체크포인트:** 액션 플랜 저장 + campaign-log.md 업데이트?
→ 확인 후 캠페인 종료 시 STEP 7으로.

---

### STEP 7 — /ad-analysis

```
실행: .claude/skills/ad-analysis.md 전체 흐름 수행
입력: 전체 캠페인 성과 데이터 (사용자 제공)
산출물: output/{name}/analysis/{YYMMDD}-analysis/analysis.md
        docs/clients/{name}/campaign-log.md (append)
        docs/lmf-learnings.md (append)
```

**체크포인트:** GATE-6 통과 (lmf-learnings.md + campaign-log.md 업데이트)?
→ Compounding 루프 완성. 다음 스프린트는 STEP 1부터 재시작.

---

## 진행 상황 트래커

파이프라인 실행 중 현재 상태를 아래 형식으로 출력한다:

```
[/ralph-meta 진행 현황] 클라이언트: {name}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 1. campaign-plan  ✅ 완료
 2. copy-lmf       ✅ 완료
 3. taxonomy       ✅ 완료
 4. ad-creative    🔄 진행 중
 5. meta-setup     ⏳ 대기
 6. ad-optimize    ⏳ 대기
 7. ad-analysis    ⏳ 대기
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 독립 실행 (중간 단계 시작)

이전 단계 산출물이 있으면 자동 로드하고 해당 단계부터 시작한다.
없는 경우 해당 단계 스킬의 AskUserQuestion으로 직접 수집한다.

