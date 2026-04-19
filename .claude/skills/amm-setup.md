# /amm-setup — 하네스 초기 설정

## 트리거

`/amm-setup` 또는 "하네스 설정", "초기 설정", "setup"

## 역할

harness-ah-my-marketing을 처음 clone한 사용자가 실행하는 온보딩 스킬.
Meta Ads API 연결 정보·알림 이메일·기본 가드레일을 수집하여 `.amm/config.json`에 저장한다.

**반드시 다른 스킬보다 먼저 실행한다.**

## Hook 게이트

없음 (이 스킬이 모든 게이트의 선행 조건)

## 독립 실행

선행 스킬 없음. 최초 1회 실행. 이후 설정 변경 시 재실행.

## Sprint Contract

스킬 실행 전 **AskUserQuestion 툴**로 순서대로 질문한다:

1. "사용자 이름 또는 팀명을 알려주세요. (config에 기록됩니다)"
2. "Meta Ads Access Token을 입력해주세요. (Meta 개발자 포털에서 발급)"
3. "Meta Ad Account ID를 입력해주세요. (예: act_123456789)"
4. "가드레일 경고를 받을 이메일 주소를 입력해주세요."
5. "기본 CPA 한도를 설정해주세요. 이 금액을 초과하면 경고합니다. (예: 15000)"
6. "기본 최소 CTR을 설정해주세요. 이 수치 이하로 떨어지면 경고합니다. (예: 0.5)"
7. "기본 최소 ROAS를 설정해주세요. (예: 2.0 — 해당 없으면 0 입력)"

답변 확인 후 → Phase 1 진행.

## 실행 흐름

### Phase 1 — config.json 생성

`.amm/config-template.json`을 기반으로 사용자 입력값을 채워
`.amm/config.json`을 생성한다.

```json
{
  "user": "{사용자명}",
  "meta": {
    "access_token": "{입력값}",
    "ad_account_id": "{입력값}"
  },
  "notifications": {
    "email": "{입력값}",
    "gmail_mcp_connected": false
  },
  "default_guardrails": {
    "cpa_limit": {입력값},
    "ctr_min": {입력값},
    "roas_min": {입력값}
  },
  "setup_completed_at": "{YYYY-MM-DD}"
}
```

### Phase 2 — .gitignore 확인

`.gitignore`에 `.amm/` 항목이 있는지 확인한다.
→ 없으면: `.gitignore`에 `.amm/` 추가.
→ 이미 있으면: 통과.

> **중요:** `.amm/config.json`은 Access Token이 포함되어 있어 절대 git에 커밋하지 않는다.

### Phase 3 — 클라이언트 폴더 안내

```
✅ .amm/config.json 생성 완료
✅ .gitignore에 .amm/ 추가 완료

다음 단계:
1. 첫 클라이언트를 추가하려면:
   → /amm-brand-setup 실행

2. 캠페인 가드레일을 설정하려면:
   → /amm-guardrail-setup 실행

3. 바로 캠페인을 시작하려면:
   → /amm-campaign-plan 실행
```

## 산출물

```
.amm/
└── config.json     ← .gitignore로 보호 (절대 커밋 금지)
```

## 다음 단계

→ `/amm-brand-setup` — 클라이언트·제품 정보 등록
→ `/amm-guardrail-setup` — 캠페인별 가드레일 설정

## 참고

→ `.amm/config-template.json` (설정 항목 전체 목록)
→ `docs/clients/_template/` (클라이언트 템플릿)
