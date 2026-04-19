# /amm-brand-setup — 브랜드·제품 정보 셋업

## 트리거

`/amm-brand-setup` 또는 "브랜드 세팅", "제품 등록", "브랜드 정보 입력", "브랜드 셋업"

## 역할

새 클라이언트 또는 새 제품 추가 시 `brand-guide.md`를 최초 작성하거나 업데이트한다.
모든 스킬의 GATE-0 통과를 위한 선행 셋업 스킬.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 폴더 존재 확인)
**Post:** GATE-5 (brand-guide.md 저장 확인)

## 독립 실행

선행 스킬 없이 독립 실행 가능. 신규 클라이언트 온보딩 시 가장 먼저 실행한다.

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 브랜드 정보를 셋업할까요? (클라이언트명)"
2. "브랜드명과 슬로건을 알려주세요."
3. "주력 광고 채널을 선택해주세요. (메타 / 구글 / 네이버 / 기타)"
4. "타겟 고객을 알려주세요. (성별, 연령, 주요 특성)"
5. "브랜드 톤앤매너를 알려주세요. (말투, 퍼소낼리티 — 예: 친근한 해요체 / 신뢰감 있는 합니다체)"
6. "Primary 컬러 HEX 코드를 알려주세요. (예: #0066FF)"
7. "판매 중인 제품/서비스 수를 알려주세요. (1개 / 2~3개 / 4개 이상)"

답변 확인 후 → Phase 1 (제품 정보 수집)으로 진행.

## 실행 흐름

### Phase 1 — 제품 정보 수집

제품 수에 따라 분기:

**제품 1~3개 (brand-guide.md 통합 기재):**

각 제품마다 AskUserQuestion으로 순서대로 질문:
- "제품/서비스명을 알려주세요."
- "한 줄 설명을 알려주세요."
- "가격대를 알려주세요."
- "핵심 타겟을 알려주세요."
- "고객이 겪는 핵심 Pain Point를 알려주세요."
- "경쟁 대비 핵심 USP를 알려주세요."
- "랜딩페이지 URL을 알려주세요."

**제품 4개 이상 (products/ 폴더 분리):**

1. 제품 전체 목록부터 수집: "제품 코드와 이름을 목록으로 알려주세요. (예: P01 회원권 / P02 PT 패키지)"
2. 각 제품별 상세 정보를 동일한 질문으로 수집

### Phase 2 — 파일 생성

`docs/clients/_template/brand-guide.md` 기반으로 작성 후 저장:

**제품 1~3개:**
```
docs/clients/{name}/brand-guide.md  ← 브랜드 정보 + 제품 상세 통합
```

**제품 4개 이상:**
```
docs/clients/{name}/brand-guide.md      ← 브랜드 정보 + 제품 목록만
docs/clients/{name}/products/P01.md    ← 제품별 상세
docs/clients/{name}/products/P02.md
...
```

### Phase 3 — 완료 출력

```
✅ brand-guide.md 저장 완료: docs/clients/{name}/brand-guide.md
✅ 등록된 제품: P01 {제품명} [, P02 {제품명}, ...]
→ 이제 모든 스킬의 GATE-0을 통과할 수 있습니다.
→ 다음 단계: /amm-taxonomy (광고 네이밍 컨벤션 정의)
```

## 산출물

```
docs/clients/{name}/
├── brand-guide.md                  ← 항상 생성
└── products/                       ← 제품 4개 이상일 때만
    ├── P01.md
    └── P02.md
```

## 다음 단계

→ `/amm-taxonomy` — 광고 네이밍 컨벤션 정의
→ `/amm-campaign-plan` — 캠페인 기획 시작

## 참고

→ `docs/clients/_template/brand-guide.md` (작성 템플릿)
→ `docs/clients/_template/products/product-template.md` (제품 파일 템플릿)
→ `.claude/hooks.md` (GATE-0 상세)
