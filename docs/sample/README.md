# Puppeteer 연동 검증 가이드

## 목적

`/ad-creative`, `/cardnews` 스킬의 HTML→PNG 변환이 실제로 동작하는지 검증.
새 클라이언트 세팅 전 반드시 이 샘플로 먼저 테스트한다.

## 샘플 파일

- `test-slide.html` — 1080×1080 광고 소재 샘플 (VP1_IMG1x1_v1)

## Puppeteer MCP로 스크린샷 찍기

Claude Code 세션에서 아래 순서로 실행:

```
1. mcp__puppeteer__puppeteer_navigate로 test-slide.html 열기
   path: file:///Users/{username}/obsidian/haven-vault/40-ClaudeCode/harness-ah-my-marketing/docs/sample/test-slide.html

2. mcp__puppeteer__puppeteer_screenshot으로 캡처
   path: docs/sample/test-slide.png
   width: 1080
   height: 1080

3. 생성된 test-slide.png 확인
   → 텍스트 잘림 없음
   → 컬러 정확
   → 파일 크기 정상 (100KB~1MB)
```

## 체크리스트

- [ ] HTML 파일이 브라우저에서 정상 렌더링되는가?
- [ ] Puppeteer 스크린샷이 1080×1080으로 생성되는가?
- [ ] 한글 폰트(Noto Sans KR)가 깨지지 않는가?
- [ ] PNG 파일이 `output/` 경로에 저장되는가?

## 실패 시 대응

| 증상 | 원인 | 해결 |
|------|------|------|
| 폰트 깨짐 | 구글폰트 로딩 실패 | 인터넷 연결 확인 또는 로컬 폰트로 변경 |
| 흰 화면 | HTML 경로 오류 | `file://` 절대경로 사용 |
| 잘림 | 캔버스 크기 미지정 | clip 파라미터 확인 |
