# 🚀 jwaytools.com 배포 가이드

`jwaytools.com` 도메인에 JWAY Paperclip을 올리는 **가장 쉬운 방법**입니다.

## 파일 구조

배포 후 URL 구조:

```
jwaytools.com/          ← 랜딩페이지 (index.html)
jwaytools.com/app.html  ← 실제 앱 (원래 index.html이었던 것)
jwaytools.com/templates/ ← 팀 템플릿 JSON 파일들
jwaytools.com/examples/ ← 결과물 샘플
```

## 방법 1: GitHub Pages + jwaytools.com (무료, 추천)

### Step 1: GitHub Pages 활성화

1. 브라우저에서 **https://github.com/JaeWoo0317/jway-paperclip** 열기
2. 상단 탭 **Settings** 클릭
3. 왼쪽 사이드바에서 **Pages** 클릭
4. **Source**: `Deploy from a branch` 선택
5. **Branch**: `main` 선택 → 오른쪽에 `/ (root)` 선택
6. **Save** 클릭

→ 몇 분 뒤 `https://jaewoo0317.github.io/jway-paperclip/`에서 접속 가능

### Step 2: 도메인 DNS 설정

**도메인 구매한 업체**(가비아, 후이즈, GoDaddy 등)에 로그인:

#### 가비아 기준
1. 내 도메인 관리 → `jwaytools.com` 선택
2. **DNS 설정** 탭
3. 다음 레코드 추가:

| 타입 | 호스트 | 값 |
|------|--------|-----|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | jaewoo0317.github.io |

4. 저장

### Step 3: GitHub에 도메인 연결

1. 저장소 **Settings** → **Pages**
2. **Custom domain**: `jwaytools.com` 입력 → Save
3. 체크 **Enforce HTTPS** (5~30분 후 체크박스 활성화됨)

### Step 4: 확인

- DNS 전파 대기 5분 ~ 최대 24시간
- `https://jwaytools.com` 접속 → 랜딩페이지 표시
- `https://jwaytools.com/app.html` 접속 → 앱 표시

## 방법 2: Vercel (더 빠름, 추천)

### Step 1: Vercel 가입

1. https://vercel.com 접속
2. **Sign up** → **Continue with GitHub**
3. 권한 허용

### Step 2: 프로젝트 import

1. 대시보드에서 **Add New... → Project**
2. `jway-paperclip` 저장소 선택 → **Import**
3. Framework Preset: **Other** (자동 감지됨)
4. Build Command: (비워두기)
5. Output Directory: (비워두기)
6. **Deploy** 클릭

→ 1~2분 뒤 `jway-paperclip.vercel.app`에서 접속 가능

### Step 3: 커스텀 도메인 연결

1. Vercel 프로젝트 → **Settings** → **Domains**
2. **Add** → `jwaytools.com` 입력
3. Vercel이 안내하는 DNS 레코드 복사
4. 도메인 업체 DNS 설정에 추가 (보통 A 레코드 1개 + CNAME 1개)
5. 5~30분 뒤 자동 연결 + HTTPS

## 방법 3: Cafe24/가비아 웹 호스팅 (FTP)

이미 웹 호스팅이 있으면:

### Step 1: 파일 준비

배포할 파일들:
```
- index.html (랜딩)
- app.html (앱)
- templates/ (폴더 전체)
- examples/ (폴더 전체)
- docs/ (선택)
```

### Step 2: FTP 업로드

- FTP 프로그램: FileZilla 추천 (무료)
- 호스팅 업체의 FTP 정보 (호스트/아이디/비밀번호)로 접속
- `public_html/` 또는 `www/` 폴더에 파일 업로드

### Step 3: 도메인 연결 확인

- 도메인 업체에서 도메인이 호스팅을 가리키는지 확인
- `http://jwaytools.com` 접속 확인

## 배포 후 체크리스트

- [ ] `jwaytools.com`에서 랜딩페이지가 뜨는가?
- [ ] "무료로 시작" 버튼 클릭 시 `app.html`로 이동하는가?
- [ ] `app.html`에서 앱이 정상 로드되는가? (비밀번호 설정 화면)
- [ ] `templates/` 폴더가 접근 가능한가? (`jwaytools.com/templates/`)
- [ ] 모바일에서도 랜딩페이지가 잘 보이는가?

## 문제 해결

### "사이트를 찾을 수 없음" 에러
- DNS 전파 대기 (최대 24시간)
- `nslookup jwaytools.com`으로 IP 확인

### HTTPS 경고
- GitHub Pages: Enforce HTTPS 체크박스 활성화 대기
- Vercel: 자동 처리됨

### app.html이 안 열림
- 파일 경로 확인: `jwaytools.com/app.html` 직접 접속
- 대소문자 주의 (일부 호스팅은 case-sensitive)

## 다음 단계

배포 성공 후:

1. **Google Analytics** 설치 (방문자 추적)
2. **Google Search Console** 등록 (검색 결과에 노출)
3. **Open Graph 이미지** 추가 (SNS 공유 미리보기)
4. **링크드인/트위터**에 배포 소식 포스팅
5. **디스콰이엇** (https://disquiet.io)에 제품 등록
