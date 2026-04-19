# 한국의 마케터를 위한 마케팅 하네스

> Claude Code 기반 AI-Native 마케팅 워크플로우 — 메타 광고 기획부터 분석까지

LMF(Language Market Fit) 프레임워크로 고객의 언어를 찾아 광고 성과를 높이는 **18개 스킬 시스템**.  
캠페인 기획부터 결과 분석·누적까지, 메타 광고의 전 단계를 AI 에이전트와 함께 수행합니다.

---

## 핵심 개념

### LMF (Language Market Fit)

광고 성과의 핵심은 **타겟이 쓰는 언어**로 말하는 것.  
공급자 언어("~를 제공합니다")를 고객 언어("아직도 ~하고 있나요?")로 바꾸는 4단계 프로세스.

```
Prepare → Diverge → Converge → Apply
  준비      발산      수렴      적용
(컨텍스트  (소구점   (카피     (광고
  로드)    발굴)    선별)     적용)
```

### Generator / Evaluator 분리

카피 생성(Generator)과 ICE 채점(Evaluator)은 **물리적으로 별도 세션**에서 수행.  
파일 저장 = 세션 경계. 생성 직후 채점 금지.

### Compounding 루프

매 캠페인 결과가 `lmf-learnings.md`와 `campaign-log.md`에 누적 → 다음 캠페인 Phase 0에서 자동 로드 → 복리 효과.

### Evidence-based ICE

ICE 채점의 Confidence 항목에 **근거 레벨 태그**를 부여해 "직관"과 "검증된 전략"을 명확히 구분.

| 태그 | 의미 |
|------|------|
| `[Strong]` | 자사 A/B 데이터 + 업계 연구 모두 뒷받침 |
| `[Moderate]` | 자사 데이터 또는 업계 사례 중 하나 |
| `[Emerging]` | 사용자 인터뷰·정성 데이터만 있음 |
| `[Expert]` | 마케터 직관 기반 — 검증 전 가설로 취급 |
| `[Contested]` | 상반된 결과 존재 — 맥락 변수 명시 필요 |

---

## 트랙 구조

### Paid Track — 7단계 순환

```mermaid
flowchart LR
    A["/amm-campaign-plan<br/>캠페인 기획"] --> B["/amm-copy-lmf<br/>LMF 소구점 발굴"]
    B --> C["/amm-taxonomy<br/>네이밍 정의"]
    C --> D["/amm-ad-creative<br/>소재 제작"]
    D --> E["/amm-meta-setup<br/>광고 세팅"]
    E --> F["/amm-ad-optimize<br/>집행 최적화"]
    F --> G["/amm-ad-analysis<br/>결과 분석"]
    G -->|Compounding| A

    style A fill:#4F46E5,color:#fff
    style B fill:#7C3AED,color:#fff
    style C fill:#2563EB,color:#fff
    style D fill:#0891B2,color:#fff
    style E fill:#059669,color:#fff
    style F fill:#D97706,color:#fff
    style G fill:#DC2626,color:#fff
```

### Organic Track — SNS 콘텐츠

```mermaid
flowchart LR
    CN["/amm-cardnews<br/>카드뉴스 HTML→PNG"]
    RE["/amm-reels<br/>릴스 HTML→MP4"]
    BL["/amm-blog<br/>블로그·뉴스레터"]
```

### Research Track — 기획·리서치

```mermaid
flowchart LR
    UI["/amm-user-interview<br/>유저 인터뷰 시뮬레이션"]
    CS["/amm-competitor-scan<br/>경쟁사 분석"]
    HL["/amm-hook-lab<br/>훅 문구 발산 ICE Top 10"]
    LA["/amm-landing-audit<br/>랜딩페이지 LMF 감사"]
```

### Meta — 설정·검토

```mermaid
flowchart LR
    BS["/amm-brand-setup<br/>브랜드·제품 셋업"]
    SP["/amm-strategy-panel<br/>전문가 패널 전략 검토"]
    RM["/amm-ralph-meta<br/>전체 파이프라인"]
    HR["/amm-harness-review<br/>하네스 자기 검토"]
```

---

## 스킬 목록

### Paid Track

| 순서 | 스킬 | 역할 | 주요 산출물 |
|------|------|------|------------|
| 1 | `/amm-campaign-plan` | 캠페인 전략·KPI·스프린트 기획 | `campaign-plan.md` |
| 1.5 | `/amm-user-interview` | 페르소나 3명 스폰 + 심층 인터뷰 | `personas.md`, `interview-log.md`, `insights.md` |
| 2 | `/amm-copy-lmf` | LMF 4단계 소구점·카피 발굴 | `lmf-brief.md`, `copy-candidates.md`, `ice-priority.md` |
| 3 | `/amm-taxonomy` | 광고 네이밍 컨벤션 정의·검증 | `taxonomy.md` |
| 4 | `/amm-ad-creative` | 광고 소재 HTML → PNG 제작 | `{VP코드}-v1.html`, `.png` |
| 5 | `/amm-meta-setup` | 캠페인→광고세트→광고 세팅 명세서 | `meta-setup.md` |
| 6 | `/amm-ad-optimize` | 집행 중 성과 최적화 | `optimize-plan.md` |
| 7 | `/amm-ad-analysis` | 캠페인 종료 후 결과 분석 + Compounding | `analysis.md` |

### Organic Track

| 스킬 | 역할 | 주요 산출물 |
|------|------|------------|
| `/amm-cardnews` | SNS 카드뉴스 기획·제작 | `slide-{N}.html`, `.png` |
| `/amm-reels` | 릴스 기획·제작 (9:16, CSS 애니메이션) | `reel.html`, `reel.mp4` |
| `/amm-blog` | 블로그·뉴스레터 초안 작성 | `outline.md`, `blog-draft.md` |

### Research Track

| 스킬 | 역할 | 주요 산출물 |
|------|------|------------|
| `/amm-user-interview` | 정성 리서처 + 페르소나 시뮬레이터 (IDI) | `personas.md`, `insights.md` |
| `/amm-competitor-scan` | 경쟁사 광고·메시지 분석, 차별화 기회 발굴 | `competitor-messages.md`, `scan-insights.md` |
| `/amm-hook-lab` | 6유형 훅 발산 + ICE 채점 Top 10 선별 | `hook-candidates.md`, `hook-top10.md` |
| `/amm-landing-audit` | 광고↔랜딩 메시지 일관성 LMF 감사 | `message-map.md`, `audit-report.md`, `ab-hypotheses.md` |

### Meta

| 스킬 | 역할 |
|------|------|
| `/amm-brand-setup` | 브랜드·제품 정보 최초 셋업 — 모든 스킬 GATE-0 선행 작업 |
| `/amm-strategy-panel` | 마케팅 전문가 6인 근거 기반 전략 검토 패널 |
| `/amm-ralph-meta` | Paid Track 7단계 전체 오케스트레이션 (순차 실행) |
| `/amm-harness-review` | 하네스 자기 검토·개선 |

---

## 스킬 상세

### `/amm-brand-setup` — 브랜드·제품 셋업 ⭐ 시작점

신규 클라이언트 온보딩 시 **가장 먼저 실행**하는 스킬. 모든 스킬의 GATE-0 통과를 위한 선행 조건.

```
Sprint Contract: 브랜드명·채널·타겟·톤앤매너·컬러·제품 수 수집
Phase 1: 제품 수에 따라 분기
  → 제품 1~3개: brand-guide.md 통합 기재
  → 제품 4개 이상: products/ 폴더로 분리 저장
Phase 2: brand-guide.md 생성 (+ products/P{N}.md)
Phase 3: 완료 확인 → /amm-taxonomy로 이동
```

### `/amm-strategy-panel` — 전문가 패널 전략 검토

캠페인 기획 직전 또는 소구점 선정 전, **6명의 마케팅 전문가**가 근거 기반으로 전략을 검토한다.

```
전문가 6인 (고정 구성):
┌─────────────────────┬─────────────────────────────────┐
│ 소비자 심리학자     │ Why 고객은 이 메시지에 반응하는가 │
│ 퍼포먼스 마케터     │ 실제 CTR·CPA 데이터 관점          │
│ 행동경제학자        │ 인지 편향·의사결정 프레임          │
│ 브랜드 전략가       │ 장기 자산 vs 단기 성과 균형        │
│ 데이터 사이언티스트 │ 측정 가능성·실험 설계             │
│ 비판적 실패 분석가  │ 이 전략이 실패하는 시나리오        │
└─────────────────────┴─────────────────────────────────┘

모든 주장에 근거 레벨 태그 [Strong/Moderate/Emerging/Expert/Contested] 필수
→ Core 소구점(검증 우선) / Peripheral 소구점(탐색용) 분리
→ 검증 가설 설계 (최소 실험 단위)

--quick 모드: 학문 지도·근거 탐색 생략, 컨텍스트 수집부터 시작
```

### `/amm-campaign-plan` — 캠페인 기획

캠페인 목표·타겟·KPI·예산·스프린트를 구조화한다.

```
Phase 0: lmf-learnings.md + campaign-log.md 자동 로드 (Compounding)
Phase 1: 시장·브랜드 컨텍스트 정리
Phase 2: 목표 설정 (KPI 수치화)
Phase 3: 타겟 페르소나 정의
Phase 4: 스프린트 기획 (소구점 우선순위)
```

### `/amm-copy-lmf` — LMF 소구점·카피 발굴

LMF 4단계로 고객의 언어를 찾고 카피 후보를 만든다.

```
Phase 0: 컨텍스트 로드 (campaign-plan, brand-guide, lmf-learnings)
Phase 1 (Prepare): Pain Point → VP → USP 매핑
Phase 2 (Diverge): 소구점별 카피 후보 대량 발산 [Generator]
         → creative-design.md 카피 정제 필수 패스 (3회)
         → 파일 저장 (GATE-4 경계)
Phase 3 (Converge): ICE 채점 + Top 5 선별 [Evaluator]
Phase 4 (Apply): LMF 브리프 확정
```

**ICE 채점 기준 (근거 레벨 태그 포함):**

| 기준 | 설명 |
|------|------|
| Impact (1~10) | 타겟 페르소나 도달 범위 (TAM 영향력) |
| Confidence (1~10) | 이 소구점이 효과적이라는 근거의 강도 + `[근거 레벨]` 태그 |
| Ease (1~10) | 소재·예산 제작 용이성 |

### `/amm-hook-lab` — 훅 문구 발산

6가지 유형으로 훅 문구를 대량 발산하고 ICE 채점으로 Top 10을 선별한다.

```
유형별 발산 (Generator):
┌─────────┬────────────────────────────┐
│ PAIN    │ "아직도 ~하고 있어요?"      │
│ QUES    │ "~해본 적 있나요?"          │
│ STAT    │ "XX%가 선택한 이유"         │
│ SOC     │ "~하는 사람들이 말하는 것"  │
│ EMO     │ "그때 그 기분, 기억하세요?" │
│ BENE    │ "~하면 ~가 달라집니다"      │
└─────────┴────────────────────────────┘

ICE 채점 (Evaluator) → Top 10 채널 매핑
```

### `/amm-landing-audit` — 랜딩페이지 LMF 감사

CTR은 높은데 전환이 안 되는 문제를 진단한다.

```
Phase 1: 광고↔랜딩 메시지 매핑표 작성
Phase 2: 공급자 언어 vs 고객 언어 분류 + 미스매치 패턴 진단
Phase 3: 개선 우선순위 + 수정안 (즉시/단기/중기)
Phase 4: A/B 테스트 가설 3개 생성
```

### `/amm-user-interview` — 유저 인터뷰 시뮬레이션

정성적 리서처 역할로 3명의 페르소나를 스폰하고 심층 인터뷰를 진행한다.

```
[P-A] 적극적 수용층  — 얼리어답터 성향
[P-B] 회의적·비판층  — 허점을 지적하는 냉정한 평가자
[P-C] 가격·시간 민감층 — 현실적 제약 우선

→ Probing 질문으로 심리적 동인·Pain Point·Objection 발굴
→ 소셜 바이어스(예의상 좋다고 답하기) 배제
→ 발굴된 고객 언어 → /amm-copy-lmf Phase 2에 직접 활용
```

### `/amm-competitor-scan` — 경쟁사 분석

시장 언어 지형도를 그려 차별화 포인트를 발굴한다.

```
Phase 1: 경쟁사 메시지 수집 (URL/이미지/자유 기술)
Phase 2: VP 소구점 패턴 분류 (PAIN/SOC/BENE 등)
Phase 3: 포화 소구점 vs 미개척 소구점 지형도
Phase 4: 차별화 VP 후보 + /amm-copy-lmf 검증 가설
```

### `/amm-ralph-meta` — 전체 오케스트레이션

Paid Track 7단계를 순서대로 실행하는 마스터 스킬.

```
진입 질문: 클라이언트명 / 계정명 / 시작 단계(1-7) / 건너뛸 단계
각 STEP: 이전 산출물 자동 로드 → 스킬 실행 → 체크포인트 확인 → 다음 STEP
진행 표시: ✅완료 / 🔄진행중 / ⏳대기
```

---

## Hook 게이트 시스템

실수를 나중에 고치지 말고, 처음부터 막는 체크포인트.

```
GATE-0 → GATE-1 → GATE-2 → GATE-3 → [스킬 실행] → GATE-4 → GATE-5 → GATE-6
```

| 게이트 | 적용 | 체크 내용 |
|--------|------|----------|
| GATE-0 | 모든 스킬 | brand-guide.md 존재 확인 → 없으면 /amm-brand-setup 유도 |
| GATE-1 | 모든 스킬 | 클라이언트 폴더 + 광고 계정 폴더 존재 확인 |
| GATE-2 | creative, setup, optimize, analysis | taxonomy.md + VP 코드 존재 확인 |
| GATE-3 | ad-creative, cardnews | LMF 브리프 존재 확인 |
| GATE-4 | copy-lmf Phase 3 진입 전 | Generator 파일 저장 확인 (Generator/Evaluator 분리) |
| GATE-5 | 모든 스킬 | 산출물 저장 확인 |
| GATE-6 | ad-analysis 완료 후 | campaign-log.md + lmf-learnings.md 업데이트 확인 |

---

## 아웃풋 폴더 구조

클라이언트별·광고 계정별로 완전 분리된 구조.

```
docs/clients/{클라이언트명}/
├── brand-guide.md                  ← 브랜드 공통 정보 (컬러·폰트·톤앤매너·제품 목록)
├── products/                       ← 제품 4개 이상일 때 개별 파일 분리
│   ├── P01.md
│   └── P02.md
└── accounts/{계정명}/              ← 광고 계정 단위 (main, kr-performance 등)
    ├── taxonomy.md
    ├── taxonomy-log.csv
    ├── campaign-log.md             ← 캠페인 이력 누적
    └── lmf-learnings.md           ← LMF 누적 인사이트 (Compounding)

output/{클라이언트명}/{계정명}/
├── plan/       {YYMMDD}-campaign-plan/
│               ├── campaign-plan.md
│               └── {YYMMDD}-strategy-panel/
│                   └── strategy-panel.md
├── copy/       {YYMMDD}-lmf/
│               ├── lmf-brief.md
│               ├── copy-candidates.md
│               ├── ice-priority.md
│               └── copy-final.md
│               {YYMMDD}-hook-lab/
│               ├── hook-candidates.md
│               └── hook-top10.md
├── creative/   {YYMMDD}-{VP코드}/
│               ├── {택소노미명}_IMG1x1_v01.html
│               └── {택소노미명}_IMG1x1_v01.png
├── setup/      {YYMMDD}-meta-setup/
│               └── meta-setup.md
├── optimize/   {YYMMDD}-optimize/
│               └── optimize-plan.md
├── analysis/   {YYMMDD}-analysis/
│               └── analysis.md
├── cardnews/   {YYMMDD}-{주제}/
├── reels/      {YYMMDD}-{VP코드}/
├── blog/       {YYMMDD}-{slug}/
└── research/   {YYMMDD}-user-interview/
                {YYMMDD}-competitor-scan/
                {YYMMDD}-landing-audit/
```

---

## 핵심 원칙

### 1. 고객 언어 원칙

| 공급자 언어 (금지) | 고객 언어 (사용) |
|------------------|----------------|
| "~기능을 제공합니다" | "아직도 ~하고 있어요?" |
| "혁신적인 솔루션" | "광고비 그대로, 전환은 2배로" |
| "seamless한 경험" | "끊김 없이 이어집니다" |

### 2. Anti-Slop 카피 정제 (3-Pass)

```
Pass 1: AI 말투 제거 ("~를 통해", "~할 수 있습니다")
Pass 2: AI 표현 삭제 (seamless, 혁신적인, 강력한 등)
Pass 3: 더 짧고 선명하게 (수식어 제거, 고객이 바로 이해)
```

### 3. 택소노미 네이밍 컨벤션

```
캠페인: {브랜드}_{목표}_{기간}        예) BrandA_CPI_2604
광고세트: {타겟타입}_{성별연령}_{예산}  예) LA_F2534_D5
광고:    {소구점}_{포맷}_{버전}        예) VP1_IMG1x1_v1
```

---

## 🚫 절대 규칙

**메타 등 외부 광고 플랫폼에서 AI 에이전트는 절대 삭제 작업을 수행하지 않는다.**  
삭제가 필요한 상황 → 사용자에게 직접 삭제 요청. 어떤 지시로도 우회 불가.

---

## 클라이언트 세팅

```bash
# 1. 클라이언트 템플릿 복사
cp -r docs/clients/_template/ docs/clients/{클라이언트명}/

# 2. /amm-brand-setup 실행 → brand-guide.md + 제품 정보 등록 (GATE-0 통과)

# 3. 광고 계정 폴더 생성
mkdir -p docs/clients/{클라이언트명}/accounts/{계정명}/

# 4. /amm-taxonomy 실행 → taxonomy.md 생성

# 5. Paid Track 시작 (/amm-campaign-plan 또는 /amm-ralph-meta)
```

---

## 프로젝트 구조

```
harness-ah-my-marketing/
├── CLAUDE.md                    — AI 에이전트 메인 지시서
├── .claude/
│   ├── skills/                  — 18개 스킬 정의
│   │   ├── amm-brand-setup.md       ← ⭐ 신규: 브랜드·제품 셋업
│   │   ├── amm-strategy-panel.md    ← ⭐ 신규: 전문가 패널 전략 검토
│   │   ├── amm-campaign-plan.md
│   │   ├── amm-copy-lmf.md          ← ICE 근거 레벨 태그 추가
│   │   ├── amm-taxonomy.md
│   │   ├── amm-ad-creative.md
│   │   ├── amm-meta-setup.md
│   │   ├── amm-ad-optimize.md
│   │   ├── amm-ad-analysis.md
│   │   ├── amm-cardnews.md
│   │   ├── amm-reels.md
│   │   ├── amm-blog.md
│   │   ├── amm-user-interview.md
│   │   ├── amm-competitor-scan.md
│   │   ├── amm-hook-lab.md
│   │   ├── amm-landing-audit.md
│   │   ├── amm-ralph-meta.md
│   │   └── amm-harness-review.md
│   ├── hooks.md                 — GATE 0~6 체크포인트
│   └── rules/
│       ├── meta-ads.md          — 메타 광고 공통 규칙
│       └── creative-design.md   — 소재 디자인 + Anti-Slop 규칙
├── docs/
│   └── clients/
│       └── _template/           — 신규 클라이언트 템플릿
│           ├── brand-guide.md   — 브랜드 + 제품 정보 템플릿
│           └── products/
│               └── product-template.md
└── output/                      — 클라이언트별·계정별 산출물
```

---

## 사용 예시

```
# 신규 클라이언트 온보딩
/amm-brand-setup
→ /amm-taxonomy

# 전략 검토 후 캠페인 기획
/amm-strategy-panel
→ /amm-campaign-plan
→ /amm-copy-lmf

# 새 캠페인 전체 실행
/amm-ralph-meta

# 경쟁사 분석 후 훅 발산
/amm-competitor-scan
→ /amm-hook-lab

# 랜딩페이지 전환율 문제 진단
/amm-landing-audit
```

---

*Built with Claude Code · LMF Framework by 김새암*
