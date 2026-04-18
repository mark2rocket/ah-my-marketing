# /taxonomy — 메타 광고 네이밍 컨벤션 정의

## 트리거

`/taxonomy` 또는 "택소노미", "네이밍 컨벤션", "광고명 규칙"

## 역할

클라이언트의 메타 광고 네이밍 컨벤션을 정의하고, 기존 광고명을 검증·수정한다.
모든 다른 스킬(`/meta-setup`, `/copy-lmf`, `/cardnews`)의 기반이 되는 선행 스킬.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 컨텍스트 확인)
**Post:** GATE-5 (산출물 저장 확인)

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 택소노미를 정의할까요? (클라이언트명 또는 프로젝트명)"
2. "기존 광고명 샘플이 있나요? 있다면 붙여넣어 주세요. (없으면 신규 정의로 진행합니다)"

답변 확인 후 → `docs/clients/{name}/` 폴더 존재 여부를 자동 확인하고 Phase 1로 진행.

## 실행 흐름

### Phase 1 — 컨텍스트 수집

```
1. docs/clients/{name}/brand-guide.md 읽기 (있으면)
2. 기존 광고명 샘플 분석 (사용자 제공 시)
3. 없으면 신규 정의 모드로 진행
```

### Phase 2 — 택소노미 정의

`docs/taxonomy-reference.md`를 기반으로 클라이언트 커스터마이징:

```
질문 순서:
1. 제품코드는? (예: MYSVC, APPX, B2BSAS — 영문 대문자 약어)
2. 주력 마케팅 목표는? (ACQ / RET / REV / BRD)
3. 주력 캠페인 목표는? (AC / CON / TR / LD)
4. 페르소나는 몇 개? → P1~PN 코드 부여 및 한 줄 설명
5. 주요 VP/USP는 몇 개? → VP1~VPN / USP1~USPN 정의
6. 주로 사용하는 지면은? (FEED / REEL / STO / ALL)
7. 스프린트는 S01부터 시작할지, 이어갈지?
```

### Phase 3 — 택소노미 문서 생성

`docs/clients/{name}/taxonomy.md` 생성 또는 업데이트:

```markdown
# {클라이언트명} 택소노미

## 제품코드: {CODE}

## 네이밍 구조
캠페인: {MKT목표}_{CPG목표}_{제품코드}_{스프린트}_{YYMMDD}
세트:   {OPT목표}_{페르소나}_{제품코드}_{타겟}_{지면}_{VP}_{YYMMDD}
광고:   {USP}_{카피훅}_{소재유형}_{YYMMDD}_{버전}

## 페르소나 코드
| 코드 | 설명 |
|------|------|
| P1   | |
| P2   | |

## VP / USP 정의
| 코드 | 소구점 내용 | ICE Impact | ICE Ease | 상태 |
|------|------------|-----------|---------|------|
| VP1  | | | | 테스트중/Winner/기각 |

## 전체 광고명 예시
캠페인: ACQ_AC_{CODE}_S01_{YYMMDD}
세트:   CPI_P1_{CODE}_LA_FEED_VP1_{YYMMDD}
광고:   USP1_PAIN_IMG1x1_{YYMMDD}_v01
```

### Phase 4 — taxonomy-log.csv 초기화

`docs/clients/{name}/taxonomy-log.csv` 생성:
- `docs/clients/_template/taxonomy-log.csv` 복사
- 헤더 유지, 샘플 행 삭제 후 실제 광고명 기록 시작

### Phase 5 — 검증

기존 광고명이 있으면:
1. 3레벨(캠페인/세트/광고) 각각 필드 수 확인 (언더스코어 개수)
2. taxonomy-log.csv에서 중복 광고명 탐색
3. 컨벤션 위반 항목 표시 + 수정안 제시 (원본 → 수정본)

## 산출물

- `docs/clients/{name}/taxonomy.md` (생성/업데이트)
- 검증 리포트 (인라인 출력)

## 참고

→ `docs/taxonomy-reference.md` (불변 레퍼런스)
→ `.claude/rules/meta-ads.md` (메타 광고 공통 규칙)
