# /amm-reels — 릴스 기획 및 제작

## 트리거

`/amm-reels` 또는 "릴스", "릴스 제작", "숏폼", "영상 콘텐츠"

## 역할

SNS 오가닉 콘텐츠용 릴스 기획(훅·스크립트·CTA) →
HTML CSS 애니메이션 →
Playwright MCP 영상 녹화 → MP4.
인스타그램, 유튜브 쇼츠, 틱톡 등 세로형 숏폼 채널 전용.

## Hook 게이트 (→ `.claude/hooks.md` 상세)

**Pre:** GATE-1 (클라이언트 확인)
**Post:** GATE-5 (산출물 저장 확인)

## 독립 실행

선행 스킬 없이 독립 실행 가능.
`docs/clients/{name}/brand-guide.md`가 있으면 자동 로드.

## Sprint Contract (시작 전 합의)

스킬 실행 전 **AskUserQuestion 툴**로 아래를 순서대로 질문한다:

1. "어떤 클라이언트의 릴스를 제작할까요? (클라이언트명)"
2. "광고 계정명을 알려주세요. (예: main / kr-performance / default)"
3. "릴스의 핵심 메시지 또는 소구점을 알려주세요. (VP 코드 또는 자유 주제)"
4. "영상 길이는 몇 초인가요? (권장: 15초 / 30초 / 60초)"
5. "브랜드 컬러를 알려주세요. (HEX 코드 — 모르면 '기본값 사용')"
6. "배경 음악 분위기는? (없음 / 경쾌 / 감성 / 강렬 — 스크립트 템포 결정용)"

답변 확인 후 → Phase 1으로 진행.

## 실행 흐름

### Phase 0 — 브랜드·제품 로드 (GATE-0)

```
0. (GATE-0) docs/clients/{name}/brand-guide.md 읽기
   → 없으면: "/amm-brand-setup을 먼저 실행해주세요." → 중단
   → 제품 1개: 자동 선택, 이후 전체 진행에서 이 제품 정보 사용
   → 제품 2~3개: AskUserQuestion으로 선택 요청
     "이번에 광고할 제품/서비스를 선택해주세요: P01 {제품명} / P02 {제품명}"
   → 제품 4개 이상: products/ 폴더 목록 출력 → AskUserQuestion으로 선택 → P{N}.md 로드
```

### Phase 1 — 릴스 스크립트 기획

릴스 구조 설계 (3단 구성):

```
1. 훅 (0~3초)   — 첫 1초 안에 멈추게 만드는 장면·문구
2. 본문 (3~25초) — 핵심 메시지 전달 (문제→해결 또는 Before→After)
3. CTA (마지막)  — 행동 유도 ("저장해두세요" / "팔로우" / "링크 클릭")
```

스크립트 테이블:
| 씬 | 시간 | 화면 텍스트 | 내레이션/자막 | 전환 효과 |
|----|------|-----------|-------------|---------|
| 1 | 0~3s | | | 즉시 |
| 2 | 3~8s | | | fade |
| 3 | 8~15s | | | slide |

> 스크립트 완성 후 → `.claude/rules/creative-design.md` "카피 정제 필수 패스" 적용 후 Phase 2 진행.

### Phase 2 — HTML 애니메이션 생성

9:16 캔버스 (1080×1920)에 씬별 HTML을 단일 파일로 생성:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700;900&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      width: 1080px;
      height: 1920px;
      background: {bg_color};
      font-family: 'Noto Sans KR', sans-serif;
      overflow: hidden;
      position: relative;
    }

    /* 씬 기본 — 숨김 상태 */
    .scene {
      position: absolute;
      inset: 0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 80px;
      opacity: 0;
    }

    /* 씬 전환 애니메이션 */
    .scene.active { animation: fadeIn 0.5s forwards; }
    .scene.exit   { animation: fadeOut 0.3s forwards; }

    @keyframes fadeIn  { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
    @keyframes fadeOut { from { opacity: 1; } to { opacity: 0; } }

    .hook-text {
      font-size: 96px;
      font-weight: 900;
      color: {text_color};
      text-align: center;
      line-height: 1.2;
    }
    .body-text {
      font-size: 64px;
      font-weight: 700;
      color: {text_color};
      text-align: center;
      line-height: 1.4;
    }
    .cta-text {
      font-size: 56px;
      font-weight: 900;
      color: {cta_color};
      text-align: center;
    }
    .brand {
      position: absolute;
      top: 60px;
      left: 60px;
      font-size: 32px;
      font-weight: 700;
      color: {brand_color};
    }
    .progress {
      position: absolute;
      bottom: 0;
      left: 0;
      height: 8px;
      background: {brand_color};
      animation: progress {duration}s linear forwards;
    }
    @keyframes progress { from { width: 0; } to { width: 100%; } }
  </style>
</head>
<body>
  <div class="brand">{브랜드명}</div>
  <div class="progress"></div>

  <div class="scene" id="scene-hook">
    <p class="hook-text">{훅 문구}</p>
  </div>

  <div class="scene" id="scene-body">
    <p class="body-text">{본문 메시지}</p>
  </div>

  <div class="scene" id="scene-cta">
    <p class="cta-text">{CTA 문구}</p>
  </div>

  <script>
    const scenes = ['scene-hook', 'scene-body', 'scene-cta'];
    const timings = [{hook_duration}, {body_duration}, {cta_duration}]; // 초 단위

    let current = 0;

    function showScene(idx) {
      scenes.forEach((id, i) => {
        const el = document.getElementById(id);
        el.classList.remove('active', 'exit');
        if (i === idx) el.classList.add('active');
        else if (i < idx) el.classList.add('exit');
      });
    }

    showScene(0);
    let elapsed = 0;
    timings.forEach((duration, idx) => {
      elapsed += (idx === 0 ? 0 : timings[idx - 1]) * 1000;
      setTimeout(() => showScene(idx), elapsed);
    });
  </script>
</body>
</html>
```

> `.claude/rules/creative-design.md` 금지 목록 준수:
> 그라데이션·글로우·그림자·border 컬러 금지. 타이포그래피로만 위계.

### Phase 3 — Playwright MCP 영상 녹화

```javascript
// Playwright MCP로 실행
// recordVideo 옵션으로 9:16 MP4 녹화

const context = await playwright.newContext({
  viewport: { width: 1080, height: 1920 },
  recordVideo: {
    dir: '{output_path}/',
    size: { width: 1080, height: 1920 }
  }
});

const page = await context.newPage();
await page.goto(`file://{html_path}/reels.html`);

// 영상 전체 재생 대기 (씬 전환 포함)
await page.waitForTimeout({total_duration_ms});

await context.close(); // 녹화 자동 저장
// 저장 경로: {output_path}/reels-{YYMMDD}.mp4 (Playwright 자동 명명)
```

### Phase 4 — 검토 체크리스트

- [ ] 첫 3초 안에 훅이 화면에 등장하는가?
- [ ] 텍스트가 잘리지 않고 가독성이 확보되었는가?
- [ ] 브랜드 컬러가 정확한가?
- [ ] CTA가 마지막에 명확하게 등장하는가?
- [ ] 전체 영상 길이가 요청한 초 수와 일치하는가?
- [ ] AI스러운 표현(seamless, 혁신적인 등)이 없는가?

## 산출물

```
output/{name}/{account}/reels/{YYMMDD}-{소구점코드}/
├── reels.html          — 애니메이션 소스
├── storyboard.md       — 스크립트·씬 설계
└── reels-{YYMMDD}.mp4  — Playwright 녹화 영상
```

## 브랜드 가이드 미제공 시 기본값

```
배경색: #0A0A0F
텍스트 색: #FFFFFF
강조 색: #0066FF
브랜드 위치: 좌상단
```

## 참고

→ `docs/clients/{name}/brand-guide.md` (브랜드 에셋)
→ `.claude/rules/creative-design.md` (디자인 금지 목록 — 반드시 확인)
→ `.claude/rules/meta-ads.md` (카피 금지 표현)
→ `.claude/skills/amm-copy-lmf.md` (소구점 및 카피)
