# harness-ah-my-marketing

> 김새암의 메타 광고 마케팅 하네스.
> 마지막 업데이트: 2026-04-19

---

## 목적

메타 광고 기획·세팅·카피·소재 제작을 AI와 함께 수행.
LMF(Language Market Fit) 기반으로 고객의 언어를 찾아 광고 성과를 높인다.

---

## 트랙 구조

### Paid Track (메타 광고)

```
/taxonomy → /copy-lmf → /ad-creative → /meta-setup → /ad-analysis
    │             │            │              │              │
  네이밍       소구점 발굴    소재 제작      캠페인 세팅    결과 분석
  컨벤션       (LMF 4단계)   (HTML→PNG)    (명세서)      (Compounding)
```

### Organic Track (SNS 콘텐츠)

```
/cardnews
    │
카드뉴스 기획·제작 (인스타, 블로그 등)
```

---

## 스킬 목록

| 트랙 | 명령어 | 역할 | 선행 조건 |
|------|--------|------|----------|
| Paid | `/taxonomy` | 광고 네이밍 컨벤션 정의·검증 | 없음 |
| Paid | `/copy-lmf` | LMF 4단계 소구점·카피 발굴 | `/taxonomy` 완료 |
| Paid | `/ad-creative` | 광고 소재 HTML → PNG 제작 | `/copy-lmf` 완료 |
| Paid | `/meta-setup` | 캠페인→광고세트→광고 세팅 명세서 | `/ad-creative` 완료 |
| Paid | `/ad-analysis` | 캠페인 결과 분석 + Compounding | 캠페인 종료 후 |
| Organic | `/cardnews` | SNS 카드뉴스 기획·제작 | 없음 (독립 실행) |
| Meta | `/harness-review` | 하네스 자기 검토·개선 | 월 1회 또는 캠페인 3회 후 |

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
  → .claude/skills/{스킬명}.md        # 스킬 실행 시
  → .claude/hooks.md                  # 게이트 확인 시
  → docs/clients/{name}/taxonomy.md   # 택소노미 확인 시
  → docs/clients/{name}/campaign-log.md  # 이전 실험 결과 시
  → docs/lmf-learnings.md             # 전체 LMF 누적 인사이트
  → .claude/rules/meta-ads.md         # 메타 규정 확인 시
```

---

## 클라이언트 추가

`docs/clients/_template/` 복사 → `docs/clients/{name}/` 생성 후 시작.

**현재 클라이언트:** `_template/` (신규 템플릿)
