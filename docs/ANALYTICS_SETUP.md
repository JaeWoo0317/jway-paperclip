# 📊 Google Analytics 설치 가이드

**소요 시간:** 10분
**비용:** 무료
**효과:** 방문자 수, 체류 시간, 전환율 실시간 추적

---

## Step 1: Google Analytics 계정 생성

1. **https://analytics.google.com** 접속 (Google 계정 로그인)
2. 왼쪽 하단 **관리** (톱니바퀴 아이콘) 클릭
3. **+ 속성 만들기** 클릭
4. 속성 이름: `JWAY Tools` 입력
5. 보고 시간대: `대한민국`, 통화: `대한민국 원(KRW)`
6. **비즈니스 목표** 선택 (여러 개 체크 가능):
   - ✅ 리드 생성
   - ✅ 사용자 행동 이해
   - ✅ 온라인 판매 촉진
7. 계속 → 데이터 수집 시작 → **웹** 선택

---

## Step 2: 웹 스트림 설정

1. 웹사이트 URL: `https://jwaytools.com`
2. 스트림 이름: `JWAY Tools Web`
3. **스트림 만들기** 클릭
4. **측정 ID** 복사 (형식: `G-XXXXXXXXXX`)

---

## Step 3: 코드 설치

### index.html (랜딩페이지 + 메인)

`<head>` 태그 안에 다음 추가:

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

**`G-XXXXXXXXXX`를 본인 측정 ID로 교체하세요!**

### 설치 위치

- `JaeWoo0317.github.io/index.html` (메인)
- `JaeWoo0317.github.io/paperclip/index.html` (랜딩)
- `JaeWoo0317.github.io/paperclip/app.html` (앱)

---

## Step 4: 이벤트 추적 (선택)

특정 행동을 추적하려면 JavaScript에서:

```javascript
// 데모 시작 버튼 클릭
gtag('event', 'demo_start', {
  'event_category': 'engagement'
});

// 정식 시작 버튼 클릭
gtag('event', 'app_launch', {
  'event_category': 'conversion'
});

// 대기자 등록
gtag('event', 'waitlist_signup', {
  'event_category': 'conversion',
  'value': 1
});
```

---

## Step 5: 확인

배포 후 2~5분 뒤:
1. Google Analytics **실시간** 보고서 열기
2. 본인이 사이트 방문
3. 실시간 사용자 수 `1` 표시되면 **설치 성공**

---

## 추적할 주요 지표

| 지표 | 의미 | 목표 |
|------|------|------|
| 방문자 수 | 일일 UV | 월 1,000명+ |
| 체류 시간 | 관심도 | 평균 2분+ |
| 이탈률 | 단일 페이지 이탈 | 60% 이하 |
| 데모 → 앱 전환율 | 핵심 지표 | 30%+ |
| 대기자 등록율 | 신뢰도 | 5%+ |

---

## 대안: Plausible.io (개인정보 친화적)

Google Analytics가 부담스럽다면:
- **Plausible.io**: 월 $9, 개인정보 수집 최소화, 쿠키 없음
- **Fathom**: 월 $14, 비슷한 철학
- **Microsoft Clarity**: 무료, 녹화 기능 포함

```html
<!-- Plausible 예시 -->
<script defer data-domain="jwaytools.com" src="https://plausible.io/js/script.js"></script>
```
