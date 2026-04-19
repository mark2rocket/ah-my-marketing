# /amm-ad-creative — 메타 광고 소재 제작

## 트리거

`/amm-ad-creative` 또는 "광고 소재", "배너 제작", "소재 만들기"

## 역할

`/amm-copy-lmf`에서 발굴한 소구점·카피를 메타 광고 소재(이미지)로 제작한다.
HTML 렌더링 → Puppeteer 스크린샷 → PNG. 페이드 광고 전용 포맷.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 확인) → GATE-2 (택소노미 확인) → GATE-3 (LMF 브리프 확인)
**Post:** GATE-5 (산출물 저장 확인 — 파일명이 택소노미 컨벤션인지 포함)

## 선행 조건

- `/amm-taxonomy` 완료 → VP 코드 정의됨
- `/amm-copy-lmf` 완료 → `output/{name}/copy/copy-final-*.md` 존재

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 광고 소재를 제작할까요? (클라이언트명)"
2. "제작할 소구점 코드를 알려주세요. (예: VP1, VP2 — taxonomy.md 기준)"
3. "소재 비율을 선택해주세요. (1x1 피드 / 4x5 세로피드 / 9x16 릴스·스토리 — 복수 선택 가능)"
4. "브랜드 기본 컬러를 알려주세요. (HEX 코드, 예: #0066FF)"
5. "이번 버전은 몇 번인가요? (예: v01, v02)"

답변 확인 후 → Phase 1 (소재 브리프 확인)으로 진행.

## 실행 흐름

### Phase 0 — 선행 산출물 자동 로드

```
1. output/{name}/copy/ 폴더에서 날짜순 최신 copy-final.md 자동 발견
   → 찾으면: VP 소구점 + 헤드라인 + 서브카피 + CTA 확인
   → 없으면: AskUserQuestion으로 카피 직접 입력 요청

2. docs/clients/{name}/taxonomy.md 읽기 (GATE-2 통과 전제)
   → VP 코드 + 네이밍 컨벤션 확인

3. docs/clients/{name}/brand-guide.md 읽기
   → 찾으면: 컬러·폰트 로드
   → 없으면: Sprint Contract 4번 질문으로 HEX 코드 직접 입력 요청

4. 자동 로드 결과 출력:
   "copy-final.md: {경로} ✅ / taxonomy.md: ✅ / brand-guide.md: ✅/❌
    → 진행 가능 여부 확인"
```

### Phase 1 — 소재 브리프 확인

```
1. Phase 0 로드 결과 기반으로 제작할 VP + 카피 정리
2. 택소노미 기준 파일명 생성
   형식: {브랜드코드}_{목표코드}_{서비스코드}_{시즌}_{기간}_{VP코드}_{포맷}_{비율}_v{N}
```

### Phase 2 — HTML 소재 생성

> 소재에 삽입할 카피가 Anti-Slop 정제를 거쳤는지 먼저 확인한다.
> 기준: `.claude/rules/creative-design.md` → "카피 정제 필수 패스"

비율별 캔버스 크기:

| 비율 코드 | width | height | 용도 |
|----------|-------|--------|------|
| 1x1 | 1080 | 1080 | 피드 |
| 4x5 | 1080 | 1350 | 세로 피드 |
| 9x16 | 1080 | 1920 | 릴스/스토리 |

**광고 소재 HTML 구조:**

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700;900&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      width: {width}px;
      height: {height}px;
      background: {bg_color};
      font-family: 'Noto Sans KR', sans-serif;
      position: relative;
      overflow: hidden;
    }
    .hook {
      /* 후킹 영역: 상단 30~40% */
      font-size: {hook_size}px;
      font-weight: 900;
      color: {text_color};
      line-height: 1.2;
    }
    .headline {
      /* 핵심 메시지: 중앙 */
      font-size: {headline_size}px;
      font-weight: 700;
    }
    .cta {
      /* CTA: 하단 */
      background: {cta_bg};
      color: {cta_text};
      padding: 20px 40px;
      border-radius: 8px;
      font-size: 28px;
      font-weight: 700;
    }
    .brand {
      position: absolute;
      top: 30px;
      left: 40px;
      font-size: 20px;
      color: {brand_color};
      font-weight: 700;
    }
  </style>
</head>
<body>
  <div class="brand">{브랜드명}</div>
  <div class="hook">{후킹 문구}</div>
  <div class="headline">{헤드라인}</div>
  <button class="cta">{CTA 문구}</button>
</body>
</html>
```

### Phase 3 — Puppeteer 스크린샷

```javascript
await puppeteer.navigate(`file://{html_path}`);
await puppeteer.screenshot({
  path: `{output_path}/{택소노미파일명}.png`,
  clip: { x: 0, y: 0, width: {width}, height: {height} }
});
```

### Phase 4 — 메타 정책 검증

- [ ] 이미지 내 텍스트가 20% 이하인가?
- [ ] 절대적 표현(최고, 유일, 완벽) 없는가?
- [ ] 전/후 비교 이미지 아닌가?
- [ ] CTA가 명확한가?
- [ ] 브랜드 로고/이름이 있는가?

## 산출물

```
output/{name}/creative/{YYMMDD}-{VP코드}/
├── {택소노미광고명}_IMG1x1_v01.html
├── {택소노미광고명}_IMG1x1_v01.png
├── {택소노미광고명}_IMG4x5_v01.html
└── {택소노미광고명}_IMG4x5_v01.png
```

## 다음 단계

→ `/amm-meta-setup` — 제작된 소재 파일명 기반 광고 세팅 명세서 작성

## 참고

→ `docs/clients/{name}/brand-guide.md` (컬러, 폰트)
→ `docs/clients/{name}/taxonomy.md` (VP 코드, 파일명 규칙)
→ `.claude/skills/amm-copy-lmf.md` (카피 원본)
→ `.claude/rules/meta-ads.md` (텍스트 비율, 카피 금지 표현)
→ `.claude/rules/creative-design.md` (디자인 금지 목록 — 반드시 확인)
