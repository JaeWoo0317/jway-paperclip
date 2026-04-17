# 🚀 배포 가이드

## GitHub Pages 배포 (가장 간단, 무료)

### 1. GitHub 저장소 설정

1. https://github.com/JaeWoo0317/jway-paperclip 로 이동
2. **Settings** 탭 클릭
3. 좌측 메뉴 **Pages** 클릭
4. **Source**: `Deploy from a branch`
5. **Branch**: `main` / `/ (root)` 선택
6. **Save** 클릭

### 2. 접속 URL

배포 후 약 1~5분 뒤에 다음 URL로 접속 가능:

```
https://jaewoo0317.github.io/jway-paperclip/
```

### 3. 커스텀 도메인 (선택)

자체 도메인이 있다면:
1. 도메인 DNS 설정: CNAME → `jaewoo0317.github.io`
2. Settings → Pages → Custom domain에 입력
3. Enforce HTTPS 체크

## 대안 배포 방법

### Vercel (무료, 자동 배포)

1. https://vercel.com 가입 (GitHub 연동)
2. **New Project** → `jway-paperclip` 선택
3. Framework Preset: **Other**
4. Build Command: (비워둠)
5. Output Directory: (비워둠)
6. **Deploy** 클릭

결과 URL: `https://jway-paperclip.vercel.app`

장점: 배포 속도 빠름, 자동 HTTPS, Preview 배포 지원

### Netlify (무료, 드래그&드롭)

1. https://app.netlify.com 가입
2. **Sites** → 드래그&드롭으로 `index.html` 업로드
3. 자동으로 URL 생성

### 로컬 공유 (개발/테스트용)

```bash
# Python 3
python -m http.server 8000

# Node.js
npx serve .

# 뒤에 ngrok으로 외부 공유 가능
ngrok http 8000
```

## 배포 후 할 일

### 1. 랜딩페이지 연결

배포 완료 후:
- `landing.html` → `index.html`로 랜딩페이지 덮어쓰기
- 기존 index.html → `app.html`로 이동
- 랜딩페이지의 "시작하기" 버튼이 `/app.html` 연결

### 2. SEO 메타 태그 추가

`index.html` `<head>`에 추가:
```html
<meta name="description" content="AI 에이전트 팀으로 새 서비스 아이디어를 MVP 기획, 디자인, 마케팅까지 자동 완성">
<meta property="og:title" content="JWAY Paperclip - AI 에이전트 경영 플랫폼">
<meta property="og:description" content="6명의 AI 에이전트가 협업해서 실제 비즈니스 결과물을 만듭니다">
<meta property="og:image" content="https://jaewoo0317.github.io/jway-paperclip/og-image.png">
<meta property="og:url" content="https://jaewoo0317.github.io/jway-paperclip/">
```

### 3. Analytics 설정

Google Analytics 또는 간단한 Plausible.io 추가:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### 4. 홍보

무료로 시작할 수 있는 홍보 채널:
- **Product Hunt** (영문 소개)
- **디스콰이엇** (https://disquiet.io) 제품 등록
- **긱뉴스** (https://news.hada.io) 뉴스 투고
- **링크드인** (사이드 프로젝트 스토리)
- **트위터/X** (빌드 인 퍼블릭)

### 5. 수익화 방법

**가장 빠른 방법 (30일 내):**

1. **프리미엄 템플릿 판매 (Gumroad)**
   - `templates/` 폴더의 템플릿들을 $19~49로 판매
   - Gumroad: https://gumroad.com
   - 수수료 10%

2. **1:1 컨설팅 (Calendly + Toss Payments)**
   - "당신의 사업 아이디어에 AI 팀 구성해드립니다" ($100~500/시간)
   - Calendly로 예약 받기
   - 토스페이먼츠/Stripe로 결제

3. **오픈소스 + 유료 기능 (향후)**
   - 기본 무료
   - Next.js 버전 출시 시 SaaS 구독 ($9~29/월)
