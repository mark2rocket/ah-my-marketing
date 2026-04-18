# 하네스 Hook 게이트 정의

> 각 스킬 실행 전/후에 반드시 확인하는 체크포인트.
> "실수를 나중에 고치지 말고, 처음부터 막아라."

---

## Pre-Hook (실행 전 게이트)

### GATE-1: 클라이언트 컨텍스트 확인
**적용:** 모든 스킬

```
체크:
□ 클라이언트명이 명시되었는가?
□ docs/clients/{name}/ 폴더가 존재하는가?
  → 없으면: 아래 전체 경로를 출력 후 중단.

  ❌ 클라이언트 폴더가 없습니다. 아래 순서로 진행하세요:
  Step 1. docs/clients/_template/ 폴더를 docs/clients/{name}/으로 복사
  Step 2. docs/clients/{name}/brand-guide.md 작성
  Step 3. /amm-taxonomy 실행 → taxonomy.md 생성
  Step 4. 원래 요청한 스킬 재시작
```

### GATE-2: 택소노미 선행 확인
**적용:** `/amm-ad-creative`, `/amm-meta-setup`, `/amm-ad-optimize`, `/amm-ad-analysis`

```
체크:
□ docs/clients/{name}/taxonomy.md 존재하는가?
□ 소구점(VP) 코드가 1개 이상 정의되어 있는가?
  → 없으면: "/taxonomy를 먼저 실행하세요." 출력 후 중단
```

### GATE-3: LMF 브리프 선행 확인
**적용:** `/amm-ad-creative`, `/amm-cardnews`

```
체크:
□ output/{name}/copy/lmf-brief-*.md 존재하는가?
  → 없으면: "/copy-lmf를 먼저 실행하세요." 출력 후 중단
```

### GATE-4: Generator/Evaluator 분리 확인
**적용:** `/amm-copy-lmf` Phase 3 (ICE 채점) 진입 직전

**"세션" 정의:** 여기서 세션이란 Claude 대화 컨텍스트가 아니라
`output/{name}/copy/copy-candidates-*.md` 파일의 물리적 저장 여부를 의미한다.
파일이 디스크에 저장되어야만 Generator 세션이 완료된 것으로 간주한다.

```
체크:
□ output/{name}/copy/copy-candidates-{YYMM}.md 파일이 저장되어 있는가?
  → 없으면: 아래를 출력하고 Phase 3 진입 차단.

  ❌ Generator 세션이 완료되지 않았습니다.
  Phase 3(ICE 채점)은 별도 세션에서 실행해야 합니다.
  
  지금 해야 할 것:
  Step 1. 생성된 카피 후보를 output/{name}/copy/copy-candidates-{YYMM}.md로 저장
  Step 2. 저장 완료 후 "Phase 3 시작"이라고 입력
  
  ※ "만들고 바로 채점해줘" 같은 연속 요청도 이 게이트를 통과해야 합니다.
     파일 저장 없이 채점으로 넘어가는 것은 Generator/Evaluator 원칙 위반입니다.
```

---

## Post-Hook (실행 후 게이트)

### GATE-5: 산출물 저장 확인
**적용:** 모든 스킬

```
체크:
□ 산출물이 output/{name}/ 하위에 저장되었는가?
□ 파일명이 택소노미 컨벤션을 따르는가?
  → 미저장 시: 저장 경로 명시 후 저장 요청
```

### GATE-6: Compounding 업데이트 확인
**적용:** `/amm-ad-analysis` 완료 직후 (강제 차단)

```
체크:
□ docs/clients/{name}/campaign-log.md에 이번 캠페인 결과가 기록되었는가?
□ docs/lmf-learnings.md에 winner 소구점 + 고객 언어가 추가되었는가?
  → 미기록 시: 다음 스킬 실행 차단. 아래를 출력.

  ❌ Compounding 업데이트 없이 다음 단계로 진행할 수 없습니다.
  
  지금 해야 할 것:
  Step 1. docs/clients/{name}/campaign-log.md에 이번 결과 append
  Step 2. docs/lmf-learnings.md에 winner 소구점 + 고객 언어 append
  Step 3. 완료 후 다음 스킬 재시작
  
  ※ 이 기록이 없으면 다음 /copy-lmf의 Phase 0가 빈 데이터로 시작됩니다.
     Compounding 루프 전체가 무력화됩니다.
```

---

## Hook 발동 우선순위

```
GATE-1 → GATE-2 → GATE-3 → [스킬 실행] → GATE-4 → GATE-5 → GATE-6
```

게이트 실패 시: 중단 메시지 출력 → 해결 방법 안내 → 재시작 요청.
게이트를 건너뛰려면 사용자가 명시적으로 "게이트 무시"라고 입력해야 한다.
