# harness-ah-my-marketing

> 김새암의 메타 광고 마케팅 하네스.
> 마지막 업데이트: 2026-04-19 (v2 — 7단계 워크플로우)

---

## 목적

메타 광고 기획·세팅·카피·소재 제작을 AI와 함께 수행.
LMF(Language Market Fit) 기반으로 고객의 언어를 찾아 광고 성과를 높인다.

---

## 트랙 구조

### Paid Track (메타 광고) — 7단계

```
/amm-campaign-plan → /amm-copy-lmf → /amm-taxonomy → /amm-ad-creative → /amm-meta-setup → /amm-ad-optimize → /amm-ad-analysis
       │               │           │            │               │              │              │
   캠페인 기획      소구점 발굴   네이밍 정의   소재 제작       광고 세팅     집행 중 최적화   결과 분석
   (전략·KPI)     (LMF 4단계)  (컨벤션 확정)  (HTML→PNG)     (명세서)      (예산·소재)    (Compounding)
```

> 각 스킬은 독립 실행 가능. 선행 산출물이 있으면 자동 로드, 없으면 즉시 시작.

### Organic Track (SNS 콘텐츠)

```
/amm-cardnews   /amm-reels         /blog
    │            │              │
카드뉴스     릴스 기획·제작   블로그·뉴스레터
(HTML→PNG)  (HTML→Playwright MP4)  (마크다운 초안)
```

---

## 아웃풋 폴더 규칙

모든 산출물은 `output/{클라이언트명}/{계정명}/` 하위에 **날짜 포함 폴더**로 저장한다.
클라이언트 공통 정보(`brand-guide.md`)는 `docs/clients/{name}/`에 유지하고,
계정별 데이터는 `docs/clients/{name}/accounts/{account}/`에 분리 관리한다.

`{account}` 예시: `main`, `kr-performance`, `brand-01`, `default`

```
docs/clients/{name}/
├── brand-guide.md                          ← 클라이언트 공통 (변경 없음)
└── accounts/{account}/                     ← 광고 계정 단위
    ├── taxonomy.md
    ├── taxonomy-log.csv
    ├── campaign-log.md
    └── lmf-learnings.md

output/{name}/{account}/
├── plan/       {YYMMDD}-campaign-plan/
├── copy/       {YYMMDD}-lmf/  |  {YYMMDD}-hook-lab/
├── creative/   {YYMMDD}-{VP코드}/
├── setup/      {YYMMDD}-meta-setup/
├── optimize/   {YYMMDD}-optimize/
├── analysis/   {YYMMDD}-analysis/
├── cardnews/   {YYMMDD}-{주제}/
├── reels/      {YYMMDD}-{VP코드}/
├── blog/       {YYMMDD}-{slug}/
└── research/   {YYMMDD}-user-interview/  |  {YYMMDD}-competitor-scan/  |  {YYMMDD}-landing-audit/
```

---

## 스킬 목록

| 트랙 | 순서 | 명령어 | 역할 | 독립 실행 |
|------|------|--------|------|----------|
| Paid | 1 | `/amm-campaign-plan` | 캠페인 전략·KPI·스프린트 기획 | 가능 |
| Paid | 1.5 | `/amm-user-interview` | 페르소나 스폰 + 심층 인터뷰 시뮬레이션 | 가능 |
| Paid | 2 | `/amm-copy-lmf` | LMF 4단계 소구점·카피 발굴 | 가능 |
| Paid | 3 | `/amm-taxonomy` | 광고 네이밍 컨벤션 정의·검증 | 가능 |
| Paid | 4 | `/amm-ad-creative` | 광고 소재 HTML → PNG 제작 | 가능 |
| Paid | 5 | `/amm-meta-setup` | 캠페인→광고세트→광고 세팅 명세서 | 가능 |
| Paid | 6 | `/amm-ad-optimize` | 집행 중 성과 최적화 (예산·소재·타겟) | 가능 |
| Paid | 7 | `/amm-ad-analysis` | 캠페인 종료 후 결과 분석 + Compounding | 가능 |
| Organic | — | `/amm-cardnews` | SNS 카드뉴스 기획·제작 (HTML→PNG) | 가능 |
| Organic | — | `/amm-reels` | 릴스 기획·제작 (HTML→Playwright MP4) | 가능 |
| Organic | — | `/amm-blog` | 블로그·뉴스레터 초안 작성 | 가능 |
| Research | — | `/amm-competitor-scan` | 경쟁사 광고·메시지 분석 + 차별화 기회 발굴 | 가능 |
| Research | — | `/amm-hook-lab` | 훅 문구 대량 발산 + ICE 채점 Top 10 선별 | 가능 |
| Research | — | `/amm-landing-audit` | 광고↔랜딩 메시지 일관성 LMF 감사 | 가능 |
| Meta | — | `/amm-harness-review` | 하네스 자기 검토·개선 | 가능 |

---

## 🚫 절대 규칙 (위반 불가)

> **메타 등 외부 광고 플랫폼에서 AI 에이전트는 절대 삭제 작업을 수행하지 않는다.**
> 삭제가 필요한 상황이 발생하면 반드시 사용자에게 직접 삭제를 요청한다.
> 이 규칙은 "게이트 무시" 명령으로도 우회할 수 없다.

## 핵심 원칙 (3가지만)

1. **택소노미 우선** — 네이밍 없이 세팅 금지
2. **고객 언어** — "~제공합니다" 금지. 고객 Pain Point 언어로
3. **Generator/Evaluator 분리** — 카피 생성과 ICE 채점은 별도 패스

---

## 읽기 순서 (AI 에이전트용)

```
CLAUDE.md (지금 여기)
  → .claude/skills/{스킬명}.md                                    # 스킬 실행 시
  → .claude/hooks.md                                              # 게이트 확인 시
  → docs/clients/{name}/brand-guide.md                           # 브랜드 공통 정보 확인 시
  → docs/clients/{name}/accounts/{account}/taxonomy.md           # 택소노미 확인 시
  → docs/clients/{name}/accounts/{account}/campaign-log.md       # 이전 실험 결과 시
  → docs/clients/{name}/accounts/{account}/lmf-learnings.md     # 계정별 LMF 누적 인사이트
  → .claude/rules/meta-ads.md                                    # 메타 규정 확인 시
```

---

## 클라이언트 추가

`docs/clients/_template/` 복사 → `docs/clients/{name}/` 생성 후 시작.

**현재 클라이언트:** `_template/` (신규 템플릿)
