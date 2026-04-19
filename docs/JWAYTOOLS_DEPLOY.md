# 🚀 jwaytools.com/paperclip/ 배포 가이드

**기존 웹 호스팅 (카페24/가비아 등)의 `/paperclip/` 하위 경로에 배포하는 방법**

## ✅ 배포 후 결과

```
jwaytools.com/                 ← 기존 도구 모음 사이트 (그대로)
jwaytools.com/paperclip/       ← 랜딩페이지 (index.html)
jwaytools.com/paperclip/app.html  ← JWAY Paperclip 앱
jwaytools.com/paperclip/templates/ ← 팀 템플릿
```

기존 사이트는 **전혀 건드리지 않고** 새 폴더만 추가합니다.

---

## 🎯 Step 1: 업로드할 파일 준비

로컬 컴퓨터에서 **`jway-paperclip`** 폴더를 열고, 다음 파일/폴더만 복사합니다:

```
복사할 것:
├── index.html          ← 랜딩페이지 (필수)
├── app.html            ← 앱 (필수)
├── templates/          ← 팀 템플릿 폴더 (필수)
└── examples/           ← 결과물 샘플 (선택)

복사 안 해도 됨:
├── .git/               (GitHub 용)
├── .claude/            (개발 도구 용)
├── docs/               (문서, 굳이 공개할 필요 없음)
├── .gitignore
```

가장 쉬운 방법: **전체 폴더를 복사해서 불필요한 것만 지우기**

---

## 🎯 Step 2: FTP 프로그램 설치

**FileZilla** (무료, 추천)
1. https://filezilla-project.org/ 접속
2. **Download FileZilla Client** 클릭
3. 운영체제에 맞게 설치

---

## 🎯 Step 3: FTP 정보 확인

### 카페24 사용자
1. 카페24 관리자 페이지 (`https://hosting.cafe24.com/`) 로그인
2. **나의 서비스 관리** → **FTP 관리**
3. 정보 확인:
   - FTP 호스트: `본인아이디.cafe24.com`
   - FTP 아이디: 카페24 아이디
   - FTP 비밀번호: 설정한 FTP 비밀번호

### 가비아 사용자
1. 가비아 My가비아 (`https://my.gabia.com/`) 로그인
2. **서비스 관리** → **웹호스팅**
3. 해당 호스팅 **관리** 버튼
4. **FTP 정보** 탭에서 정보 확인

---

## 🎯 Step 4: FileZilla로 접속

1. FileZilla 실행
2. 상단 입력란:
   - **호스트**: FTP 호스트 주소
   - **사용자명**: FTP 아이디
   - **비밀번호**: FTP 비밀번호
   - **포트**: `21` (기본)
3. **빠른 연결** 클릭

접속 성공 시:
- 왼쪽 = 내 컴퓨터
- 오른쪽 = 웹 호스팅 서버

---

## 🎯 Step 5: /paperclip/ 폴더 생성 및 업로드

1. 오른쪽(서버)에서 웹 루트 폴더로 이동
   - 카페24: `/public_html/` 또는 `/www/`
   - 가비아: `/www/` 또는 `/html/`

2. **오른쪽 빈 공간에 우클릭 → 디렉토리 생성** → 이름: `paperclip`

3. 오른쪽에서 `paperclip` 폴더 더블클릭하여 진입

4. 왼쪽(내 컴퓨터)에서 Step 1에서 준비한 파일들을 **드래그 앤 드롭**으로 오른쪽에 옮기기:
   - `index.html`
   - `app.html`
   - `templates/` 폴더 전체
   - `examples/` 폴더 전체 (선택)

5. 업로드 완료 대기 (하단 큐 창에서 확인)

---

## 🎯 Step 6: 접속 확인

브라우저에서:
- `https://jwaytools.com/paperclip/` → **랜딩페이지**
- `https://jwaytools.com/paperclip/app.html` → **앱**
- `https://jwaytools.com/paperclip/templates/README.md` → **템플릿 안내**

모두 정상 표시되면 **배포 완료!** 🎉

---

## 🔧 기존 jwaytools.com 메인 페이지에 링크 추가

기존 메인 사이트에서 JWAY Paperclip을 소개하려면:

```html
<!-- 기존 도구 모음 사이트에 추가 -->
<div class="tool-card">
  <h3>📎 JWAY Paperclip</h3>
  <p>6명의 AI 에이전트로 실제 사업 결과물 만들기</p>
  <a href="/paperclip/">시작하기 →</a>
</div>
```

---

## 🐛 문제 해결

### "404 Not Found" 에러
- 파일명 확인 (대소문자 주의: `index.html` vs `Index.html`)
- 폴더 이름 확인 (`/paperclip/`)

### CSS/이미지 안 보임
- 상대 경로 문제 — 이 프로젝트는 모든 링크가 상대 경로라 문제없음
- 브라우저 캐시 비우기 (Ctrl+F5)

### "403 Forbidden"
- 파일 권한 문제 — FileZilla에서 파일 선택 후 우클릭 → **파일 권한**
- 숫자값 `644` 입력 (파일), `755` (폴더)

### 한글 파일명 깨짐
- 이 프로젝트는 한글 파일명 없음. 괜찮음.

---

## 📝 업데이트 방법 (나중에)

JWAY Paperclip을 업데이트하려면:
1. GitHub에서 최신 코드 받기 (`git pull`)
2. FileZilla로 변경된 파일만 다시 업로드
3. 브라우저 새로고침 (Ctrl+F5)

`index.html`과 `app.html` 2개만 바뀌는 경우가 대부분입니다.

---

## 🎉 배포 완료 후 할 일

1. **기존 jwaytools.com 메인에 "NEW" 배지로 링크** 추가
2. **링크드인/트위터**에 "새 도구 추가했어요" 공유
3. **지인 5명**에게 카톡으로 URL 공유 → 피드백 받기
4. **구글 Search Console** 등록 (`jwaytools.com/paperclip/` sitemap)
5. 1주일 동안 피드백 수집 → 개선

축하합니다! 🚀
