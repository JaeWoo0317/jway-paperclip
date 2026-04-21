# 💰 Gumroad 결제 연결 가이드

**소요 시간:** 30분
**비용:** Gumroad 수수료 10%
**목적:** 팀 템플릿 실제 판매 → 실제 수익 창출

---

## Step 1: Gumroad 계정 만들기

1. **https://gumroad.com** 접속
2. **Start selling** 클릭
3. 이메일 가입 (또는 Google 로그인)
4. 프로필 설정:
   - 이름: `JWAY Tools` 또는 본인 이름
   - URL: `jwaytools` (고유 URL로 사용)
   - 프로필 사진: 📎 이모지 또는 로고

---

## Step 2: 상품 등록 (3개)

### 상품 1: 스타트업 MVP 팀 템플릿 ($15)

1. 대시보드 → **+ New product** 클릭
2. 선택: **Digital product** (디지털 제품)
3. 정보 입력:

```
Name: 스타트업 MVP 팀 템플릿 (6인)
URL: startup-mvp-team
Price: $15 USD (또는 ₩19,000)
```

**Description:**
```markdown
# 🚀 스타트업 MVP 팀 (6인) 템플릿

JWAY Paperclip용 검증된 에이전트 팀 구성 JSON

## 구성
- 👔 김비전 (CEO) - 비즈니스 모델 + 로드맵
- ⚙️ 이기술 (CTO) - 풀스택 아키텍처
- 📱 박피엠 (PM) - 페르소나 + MVP 스코프
- 💻 최개발 (Engineer) - 실제 코드 + API 스펙
- 📢 강마케팅 (Marketer) - 그로스 + 커뮤니티
- 🎨 서디자인 (Designer) - HTML 랜딩페이지

## 활용
1. JWAY Paperclip 접속
2. 설정 → 📦 템플릿 가져오기
3. 이 JSON 파일 업로드
4. 🤝 협업 Heartbeat 실행

## 포함 파일
- startup-mvp-team.json (에이전트/이슈/목표 구성)
- 사용 가이드 PDF
- 1시간 이메일 지원

## 결과물 예시
[샘플 보기] https://github.com/JaeWoo0317/jway-paperclip/tree/main/examples
```

4. **Content** 탭:
   - 업로드: `templates/01-startup-mvp-team.json`
5. **Settings** 탭:
   - Cover image: 업로드
   - Call to action: "Buy now"
6. **Publish** 클릭

### 상품 2: 이커머스 그로스 팀 ($12)

동일한 방식으로:
- URL: `ecommerce-growth-team`
- Price: $12 (또는 ₩15,000)
- 파일: `templates/02-ecommerce-growth-team.json`

### 상품 3: 콘텐츠 크리에이터 팀 ($10)

- URL: `content-creator-team`
- Price: $10 (또는 ₩12,000)
- 파일: `templates/03-content-creator-team.json`

---

## Step 3: 결제 정보 연결

1. **Settings → Payouts**
2. **Payout method** 선택:
   - 한국: Payoneer 또는 Wise 권장 (Stripe 미지원)
   - Payoneer: 무료, 한국 은행 출금 가능
   - Wise: 낮은 수수료
3. 본인 확인 서류 제출 (신분증)
4. 세금 정보 입력

---

## Step 4: 랜딩페이지 링크 교체

현재 `index.html`의 가격 섹션:

```html
<!-- 변경 전 -->
<a href="https://github.com/JaeWoo0317/jway-paperclip/tree/main/templates" class="cta">템플릿 보기</a>

<!-- 변경 후 -->
<a href="https://jwaytools.gumroad.com/l/startup-mvp-team" class="cta">구매하기 ($15)</a>
```

각 상품별 URL 형식:
```
https://jwaytools.gumroad.com/l/startup-mvp-team
https://jwaytools.gumroad.com/l/ecommerce-growth-team
https://jwaytools.gumroad.com/l/content-creator-team
```

---

## Step 5: 임베드 (선택)

랜딩페이지에 직접 결제 버튼 삽입:

```html
<script src="https://gumroad.com/js/gumroad.js"></script>
<a class="gumroad-button" href="https://jwaytools.gumroad.com/l/startup-mvp-team">
  Buy Now
</a>
```

---

## 💡 가격 전략

### 제안 가격

| 상품 | 제안 | 이유 |
|------|------|------|
| 스타트업 MVP 팀 | $15 | 가장 범용적, 높은 가치 |
| 이커머스 그로스 팀 | $12 | 특정 업종, 중간 가격 |
| 콘텐츠 크리에이터 | $10 | 진입 가격, 구독 유도 |
| **번들 (3개 묶음)** | **$29** | 30% 할인, 평균 객단가↑ |

### Early bird 할인

출시 첫 30일: **50% 할인** 쿠폰
- `EARLYBIRD50` 쿠폰 코드 생성
- 50명 한정 또는 30일 한정
- 가격 페이지에 눈에 띄게 배치

### Gumroad 수수료 계산

```
상품 가격 $15 × 수수료 10% = $1.50
결제 수수료 ~3.5% = $0.50
실 수익: $13
```

---

## 추적 및 최적화

### Gumroad 자체 분석

- **Audience** 탭: 구매자 이메일 리스트
- **Analytics** 탭: 조회/전환/매출 추적
- **Reviews**: 구매자 리뷰 관리

### 외부 링크 UTM 추가

```
https://jwaytools.gumroad.com/l/startup-mvp-team?utm_source=landing&utm_campaign=launch
```

---

## 📝 체크리스트

- [ ] Gumroad 계정 생성
- [ ] 프로필 URL을 jwaytools로 확보
- [ ] 상품 3개 등록
- [ ] 커버 이미지 준비 (템플릿별)
- [ ] Payout 설정 (Payoneer 권장)
- [ ] 본인 확인 완료
- [ ] 테스트 구매 (본인 카드로 $1 상품)
- [ ] 랜딩페이지 링크 교체
- [ ] Early bird 쿠폰 생성
- [ ] 링크드인/트위터에 론칭 공지
