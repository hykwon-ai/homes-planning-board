# HOMES 기획보드 운영 가이드

> 작성일: 2026-04-24
> 용도: Claude 없이도 프로젝트를 직접 수정·배포·운영할 수 있도록 모든 정보를 기록

---

## 1. 프로젝트 개요

- **사이트 주소**: https://hykwon-ai.github.io/homes-planning-board/
- **GitHub 저장소**: https://github.com/hykwon-ai/homes-planning-board
- **브랜드 컬러**: 노란 `#F5C518` / 어두운 `#1A1A1A`
- **구조**: 단일 HTML 파일 (CSS·JS 모두 인라인) + `favicon.png`

### 주요 파일 위치
| 파일 | 경로 |
|------|------|
| 작업 원본 | `/Users/kwon/Desktop/index.html` |
| GitHub 배포용 폴더 | `/Users/kwon/Desktop/homes-planning-board/` |
| 파비콘 | `/Users/kwon/Desktop/homes-planning-board/favicon.png` |

**핵심 규칙**: 수정은 `~/Desktop/index.html`에서 하고, 배포할 때 `~/Desktop/homes-planning-board/` 폴더로 복사해서 푸시.

---

## 2. API 키 및 계정 정보 (⚠️ 외부 유출 금지)

### 카카오 지도 API
- **JavaScript 키**: `7ba7a90e1d62f8383fcc7847dd0e1524`
- 발급처: https://developers.kakao.com
- HTML 10번째 줄에 직접 삽입됨
- 허용 도메인: `https://hykwon-ai.github.io`

### Supabase
- **프로젝트 URL**: `https://fdvqpurwqzaykmasdzea.supabase.co`
- **Anon Publishable Key**: `sb_publishable_gGCtue1KGJroJmxV5V3WqA_8UolAXEE`
- 대시보드: https://supabase.com/dashboard/project/fdvqpurwqzaykmasdzea
- **DB 테이블**: `projects` (id, title, location, date, manager, tab1~tab7 JSONB, created_at, updated_at)
- **Edge Function**: `claude-proxy`
  - URL: `https://fdvqpurwqzaykmasdzea.supabase.co/functions/v1/claude-proxy`
  - Verify JWT: **OFF** (반드시 꺼둬야 브라우저에서 호출 가능)
  - Secret: `ANTHROPIC_API_KEY` (Supabase 대시보드 → Settings → Edge Functions)

### Anthropic Claude API
- 콘솔: https://console.anthropic.com
- 요금 충전 페이지: https://console.anthropic.com/settings/billing
- **요금제는 Claude.ai 구독과 별개** — 별도 크레딧 충전 필요
- 최소 $5 충전이면 수백 번 호출 가능

### GitHub
- 계정: `hykwon-ai`
- 저장소: `homes-planning-board`
- 배포 방식: GitHub Pages (main 브랜치 루트)

---

## 3. 자주 하는 작업

### 3-1. HTML 수정 후 사이트 업데이트

**방법 A — 직접 편집 (텍스트 에디터 사용)**

1. `~/Desktop/index.html`을 VS Code, 메모장, 또는 TextEdit으로 열기
   - Mac 기본 TextEdit은 자동 서식 변경이 많아 비추천
   - 추천: [VS Code 다운로드](https://code.visualstudio.com/) (무료)
2. 원하는 부분 수정 후 저장 (Cmd+S)
3. 브라우저로 `index.html` 직접 열어서 결과 확인 (Finder에서 더블클릭)
4. 문제 없으면 배포 진행 (아래 3-2 참고)

**방법 B — AI(Claude/ChatGPT)에 부탁**

1. 원하는 수정 사항을 구체적으로 설명:
   > "index.html 파일에서 탭 3의 라이프사이클 표 색상을 파란색으로 바꿔줘"
2. AI가 수정된 코드를 주면 해당 부분만 붙여넣기
3. 전체 파일을 받는 게 안전하면 파일 통째로 교체

### 3-2. GitHub에 업로드 (배포)

터미널 (Terminal 앱) 열고 아래 **한 줄씩** 실행:

```bash
cd ~/Desktop/homes-planning-board
cp ~/Desktop/index.html .
git add index.html
git commit -m "수정: 어떤 걸 바꿨는지 간단히"
git push
```

- `git push` 실행 후 "Username / Password" 물으면:
  - Username: `hykwon-ai`
  - Password: **GitHub Personal Access Token** (비밀번호 아님)
  - Token 분실 시 https://github.com/settings/tokens → "Generate new token (classic)" → `repo` 권한 체크
- 1~2분 후 https://hykwon-ai.github.io/homes-planning-board/ 에 반영됨
- **캐시 문제로 안 보이면**: Cmd+Shift+R 강력 새로고침 또는 시크릿 창(Cmd+Shift+N)

### 3-3. 파비콘(아이콘) 변경

1. 새 이미지를 `~/Desktop/homes-planning-board/favicon.png`로 덮어쓰기
   - 권장: 정사각형 512×512 PNG
2. 위 3-2 단계대로 push

### 3-4. 새 프로젝트 작업 시작

- 사이트 우측 상단 **+ 새 프로젝트** 버튼 클릭
- URL에 `?id=...` 없는 빈 상태로 열림
- 입력하는 순간 자동 저장되고 URL에 새 ID가 붙음
- 저장된 프로젝트는 해당 URL로 언제든 복원 가능

### 3-5. 저장된 프로젝트 공유

- 우측 상단 **🔗 공유** 버튼 → URL 자동 복사
- 같은 URL을 열면 누구나 같은 데이터 볼 수 있음 (인증 없음)
- 지우려면 Supabase 대시보드 → Table Editor → `projects` → 해당 행 삭제

---

## 4. 문제 해결 (트러블슈팅)

### ❌ "저장 실패" 표시
- **원인 1**: 인터넷 연결 확인
- **원인 2**: Supabase 프로젝트 일시정지 (무료 플랜은 7일 미사용 시 멈춤)
  - 해결: https://supabase.com/dashboard → 프로젝트 클릭 → "Restore" 버튼

### ❌ 카카오 지도가 안 뜸 (401 오류)
- **원인**: 허용 도메인 설정 문제
- 해결: https://developers.kakao.com → 내 앱 → 플랫폼 → Web → `https://hykwon-ai.github.io` 등록 확인

### ❌ AI 버튼 눌러도 반응 없음
- 브라우저 콘솔 열어서 오류 확인 (Mac: Cmd+Option+J)
- **"Edge Function 오류 (401)"** → Supabase Edge Function의 "Verify JWT" 꺼져있는지 확인
- **"credit balance too low"** → Anthropic API 크레딧 충전 필요
- **"ANTHROPIC_API_KEY not set"** → Supabase → Settings → Edge Functions → Secrets에 키 등록 확인

### ❌ GitHub Pages가 업데이트 안 됨
- 최대 5분 정도 소요될 수 있음
- GitHub 저장소 → Actions 탭에서 배포 상태 확인
- 그래도 안 되면: Settings → Pages → Source가 "main / root"인지 확인

### ❌ `git push` 거부됨
```bash
# 로컬과 원격이 어긋난 경우
git pull origin main --rebase
git push
```

---

## 5. 기술 스택 (참고용)

- **프론트엔드**: 순수 HTML5 + CSS3 + Vanilla JavaScript
- **차트**: Chart.js 4.4.0 (CDN)
- **PDF**: html2pdf.js 0.10.1 (CDN)
- **지도**: 카카오 지도 JavaScript SDK
- **DB 클라이언트**: Supabase JS v2 (CDN)
- **백엔드**: Supabase (PostgreSQL + Edge Functions - Deno 런타임)
- **AI**: Anthropic Claude API (claude-sonnet-4-5)
- **호스팅**: GitHub Pages (main 브랜치 루트)

---

## 6. Claude를 다시 부를 때 전달할 맥락

다른 AI(또는 나중에 다시 Claude)에게 요청할 때 아래 문장을 복붙해서 시작:

> HOMES 브랜드 임대형 기숙사·호텔 기획보드 템플릿이야. 작업 원본은 `~/Desktop/index.html`이고 배포는 `~/Desktop/homes-planning-board/` 폴더에서 `git push`로 GitHub Pages(https://hykwon-ai.github.io/homes-planning-board/)에 올려. 단일 HTML 파일에 CSS·JS 인라인이고 Chart.js·Supabase JS·카카오 지도 SDK를 CDN으로 쓴다. Supabase 프로젝트는 `fdvqpurwqzaykmasdzea`, AI 연동은 `claude-proxy` Edge Function 경유. 운영 가이드는 같은 폴더의 `GUIDE.md`에 다 있어.

---

## 7. 비상 연락처 / 참고 링크

| 용도 | URL |
|------|-----|
| 배포 사이트 | https://hykwon-ai.github.io/homes-planning-board/ |
| GitHub 저장소 | https://github.com/hykwon-ai/homes-planning-board |
| Supabase 대시보드 | https://supabase.com/dashboard/project/fdvqpurwqzaykmasdzea |
| Anthropic 콘솔 | https://console.anthropic.com |
| Anthropic 결제 | https://console.anthropic.com/settings/billing |
| 카카오 개발자 센터 | https://developers.kakao.com |
| GitHub Tokens | https://github.com/settings/tokens |
| Chart.js 문서 | https://www.chartjs.org/docs/latest/ |
| Supabase 문서 | https://supabase.com/docs |

---

**💡 한 줄 요약**:
수정은 `~/Desktop/index.html` → 배포는 `homes-planning-board` 폴더에서 복사 후 `git add / commit / push` → 사이트 자동 반영.
