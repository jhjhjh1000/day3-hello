# 영어학원 홍보 사이트 구현 계획

> **에이전트 작업자용:** 필수 하위 스킬: superpowers:subagent-driven-development(권장) 또는 superpowers:executing-plans를 사용해 이 계획을 작업 단위로 구현하세요. 진행 추적은 체크박스(`- [ ]`) 문법을 사용합니다.

**목표:** 초/중/고 학생 및 학부모의 카카오톡 오픈채팅 문의를 유도하는 1페이지 정적 영어학원 홍보 사이트를 구축한다.

**아키텍처:** 현재의 최소 구성 `index.html`을 모바일 우선 단일 파일 시맨틱 HTML + 내장 CSS 랜딩 페이지로 교체한다. 승인된 섹션 구조로 콘텐츠를 구성하고, 전환을 위해 CTA를 반복 배치하며, 외부 의존성 없이 데모 데이터를 인라인으로 유지한다.

**기술 스택:** HTML5, CSS3(내장 `<style>`), 앵커 네비게이션, 정적 배포

---

## 파일 구조

- 수정: `index.html` — 단일 페이지 마크업, 스타일, 내비게이션, CTA 링크, 전체 섹션 콘텐츠 완성
- 실행 시점 신규 파일 없음(현재 범위에서는 YAGNI)
- 향후 실제 전환을 위한 치환 포인트(학원명/연락처/오픈채팅 URL)는 콘텐츠 내부에 유지

### 작업 1: 기준 파일을 시맨틱 골격 + 네비게이션 스켈레톤으로 교체

**파일:**
- 수정: `index.html`
- 테스트: `index.html` (브라우저 수동 검증)

- [ ] **Step 1: 실패 테스트 작성(콘텐츠 존재 여부로 구조 기대치 확인)**

```bash
grep -n "<header\|<main\|id=\"hero\"\|id=\"faq\"" index.html
```

기대 결과: 현재 기준 파일에는 필수 구조 매칭이 없어야 한다.

- [ ] **Step 2: 테스트 실행으로 실패 확인**

실행: `grep -n "<header\|<main\|id=\"hero\"\|id=\"faq\"" index.html`
기대 결과: 매칭 라인 없음(요구 구조 기준 실패)

- [ ] **Step 3: 최소 구현 작성(시맨틱 페이지 골격 + 네비 앵커)**

`index.html` 전체를 아래로 교체:

```html
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>프라임 영어학원 | 초중고 맞춤 영어관리</title>
    <style>
      :root {
        --bg: #f7f8fc;
        --surface: #ffffff;
        --text: #131722;
        --muted: #5e667a;
        --primary: #1f3cff;
        --primary-strong: #1128c9;
        --line: #e4e7f0;
        --success: #0e9f6e;
        --max: 1080px;
      }

      * { box-sizing: border-box; }
      html { scroll-behavior: smooth; }
      body {
        margin: 0;
        font-family: "Pretendard", "Noto Sans KR", system-ui, -apple-system, sans-serif;
        background: var(--bg);
        color: var(--text);
        line-height: 1.6;
      }

      .container {
        width: min(100% - 2rem, var(--max));
        margin: 0 auto;
      }

      .site-header {
        position: sticky;
        top: 0;
        z-index: 20;
        background: rgba(255, 255, 255, 0.92);
        border-bottom: 1px solid var(--line);
        backdrop-filter: blur(8px);
      }

      .site-header-inner {
        min-height: 64px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 1rem;
      }

      .brand {
        margin: 0;
        font-size: 1rem;
        font-weight: 800;
      }

      .nav {
        display: none;
        gap: 0.75rem;
      }

      .nav a {
        text-decoration: none;
        color: var(--muted);
        font-size: 0.9rem;
        font-weight: 600;
      }

      .cta {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        border-radius: 12px;
        padding: 0.75rem 1.1rem;
        text-decoration: none;
        font-weight: 700;
        font-size: 0.95rem;
        border: 1px solid transparent;
      }

      .cta-primary {
        background: var(--primary);
        color: #fff;
      }

      .cta-primary:hover { background: var(--primary-strong); }

      .section {
        padding: 4.5rem 0;
      }

      .section-title {
        margin: 0 0 0.5rem;
        font-size: clamp(1.45rem, 2.2vw, 2rem);
        line-height: 1.3;
      }

      .section-desc {
        margin: 0;
        color: var(--muted);
      }

      @media (min-width: 920px) {
        .nav { display: flex; }
      }
    </style>
  </head>
  <body>
    <header class="site-header">
      <div class="container site-header-inner">
        <p class="brand">프라임 영어학원</p>
        <nav class="nav" aria-label="페이지 섹션">
          <a href="#hero">홈</a>
          <a href="#strengths">강점</a>
          <a href="#results">성적 사례</a>
          <a href="#reviews">학부모 후기</a>
          <a href="#system">관리 방식</a>
          <a href="#programs">프로그램</a>
          <a href="#location">위치</a>
          <a href="#faq">FAQ</a>
        </nav>
        <a class="cta cta-primary" href="#cta-final">카카오톡 문의</a>
      </div>
    </header>

    <main>
      <section id="hero" class="section">
        <div class="container">
          <h1 class="section-title">학부모가 안심하고 맡길 수 있는 영어관리</h1>
          <p class="section-desc">섹션 콘텐츠를 다음 작업에서 채웁니다.</p>
        </div>
      </section>

      <section id="strengths" class="section"><div class="container"><h2 class="section-title">학원 강점</h2></div></section>
      <section id="results" class="section"><div class="container"><h2 class="section-title">성적 향상 사례</h2></div></section>
      <section id="reviews" class="section"><div class="container"><h2 class="section-title">학부모 후기</h2></div></section>
      <section id="system" class="section"><div class="container"><h2 class="section-title">수업/관리 방식</h2></div></section>
      <section id="programs" class="section"><div class="container"><h2 class="section-title">프로그램 안내</h2></div></section>
      <section id="location" class="section"><div class="container"><h2 class="section-title">위치/오시는 길</h2></div></section>
      <section id="faq" class="section"><div class="container"><h2 class="section-title">FAQ</h2></div></section>
      <section id="cta-final" class="section"><div class="container"><h2 class="section-title">문의하기</h2></div></section>
    </main>
  </body>
</html>
```

- [ ] **Step 4: 테스트 재실행으로 통과 확인**

실행: `grep -n "<header\|<main\|id=\"hero\"\|id=\"faq\"" index.html`
기대 결과: 시맨틱 골격과 필수 섹션 ID가 매칭되어야 한다.

- [ ] **Step 5: 커밋**

```bash
git add index.html
git commit -m "feat: scaffold single-page academy landing structure"
```

### 작업 2: 히어로, 강점, 전환 중심 CTA 블록 구현

**파일:**
- 수정: `index.html`
- 테스트: `index.html`

- [ ] **Step 1: 실패 테스트 작성(승인된 히어로/강점 키워드 누락 확인)**

```bash
grep -n "학부모가 안심\|성적 향상 실적\|소수정예\|연계 커리큘럼\|카카오톡 오픈채팅" index.html
```

기대 결과: 일부 키워드만 있거나 핵심 키워드가 누락되어야 한다.

- [ ] **Step 2: 테스트 실행으로 실패 확인**

실행: `grep -n "학부모가 안심\|성적 향상 실적\|소수정예\|연계 커리큘럼\|카카오톡 오픈채팅" index.html`
기대 결과: 필수 콘텐츠가 모두 존재하지 않아야 한다.

- [ ] **Step 3: 최소 구현 작성(히어로 카피, 지표 스트립, 강점 카드, CTA 반복)**

`#hero`와 `#strengths`를 아래 내용으로 구현:

```html
<section id="hero" class="section">
  <div class="container">
    <p style="margin:0 0 .6rem;color:var(--primary);font-weight:800;">초·중·고 맞춤 영어관리</p>
    <h1 class="section-title">학부모가 안심하고 맡길 수 있는 영어학원</h1>
    <p class="section-desc" style="max-width:680px;">
      진단부터 피드백까지 한 흐름으로 관리합니다. 내신·수능 성과를 데이터로 점검하고,
      학생별 학습 설계로 흔들림 없이 성장시킵니다.
    </p>
    <div style="display:flex;flex-wrap:wrap;gap:.75rem;margin-top:1.25rem;">
      <a class="cta cta-primary" href="https://open.kakao.com/o/demo-link" target="_blank" rel="noopener">카카오톡 오픈채팅 문의</a>
      <a class="cta" style="border-color:var(--line);color:var(--text);background:var(--surface);" href="#results">성적 사례 보기</a>
    </div>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:.75rem;margin-top:1.25rem;">
      <div style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>내신 A등급 비율</strong><br />최근 1년 78%</div>
      <div style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>학부모 만족도</strong><br />정기 설문 4.8/5</div>
      <div style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>관리 리포트</strong><br />주 1회 제공</div>
    </div>
  </div>
</section>

<section id="strengths" class="section">
  <div class="container">
    <h2 class="section-title">선택 이유가 분명한 3가지 강점</h2>
    <p class="section-desc">성과, 밀착관리, 연계 커리큘럼을 한곳에서 제공합니다.</p>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:1rem;margin-top:1.2rem;">
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;">
        <h3 style="margin:0 0 .45rem;">성적 향상 실적</h3>
        <p style="margin:0;color:var(--muted);">내신·모의고사 지표를 수치로 점검하고 단계별 목표를 관리합니다.</p>
      </article>
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;">
        <h3 style="margin:0 0 .45rem;">소수정예 맞춤관리</h3>
        <p style="margin:0;color:var(--muted);">개인별 학습 성향과 약점에 맞춘 과제·피드백으로 학습 효율을 높입니다.</p>
      </article>
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;">
        <h3 style="margin:0 0 .45rem;">초중고 연계 커리큘럼</h3>
        <p style="margin:0;color:var(--muted);">학년 전환 시기에도 끊김 없는 학습 로드맵으로 안정적으로 실력을 확장합니다.</p>
      </article>
    </div>
  </div>
</section>
```

- [ ] **Step 4: 테스트 재실행으로 통과 확인**

실행: `grep -n "학부모가 안심\|성적 향상 실적\|소수정예\|연계 커리큘럼\|카카오톡 오픈채팅" index.html`
기대 결과: 히어로/강점 섹션에 목표 문구가 모두 포함되어야 한다.

- [ ] **Step 5: 커밋**

```bash
git add index.html
git commit -m "feat: add hero messaging and strengths with primary CTA"
```

### 작업 3: 성적 사례, 학부모 후기, 학습관리 시스템 섹션 구현

**파일:**
- 수정: `index.html`
- 테스트: `index.html`

- [ ] **Step 1: 실패 테스트 작성(섹션 상세 콘텐츠 누락 확인)**

```bash
grep -n "성적 향상 사례\|학부모 후기\|진단\|학습설계\|점검\|피드백" index.html
```

기대 결과: 플레이스홀더 수준이거나 상세 요소가 누락되어 있어야 한다.

- [ ] **Step 2: 테스트 실행으로 실패 확인**

실행: `grep -n "성적 향상 사례\|학부모 후기\|진단\|학습설계\|점검\|피드백" index.html`
기대 결과: 필수 상세 블록이 모두 존재하지 않아야 한다.

- [ ] **Step 3: 최소 구현 작성(성과/후기 카드 + 5단계 관리 흐름)**

`#results`, `#reviews`, `#system`을 아래 내용으로 구현:

```html
<section id="results" class="section">
  <div class="container">
    <h2 class="section-title">성적 향상 사례</h2>
    <p class="section-desc">학생 상황에 맞춘 설계로 실제 변화를 만들었습니다.</p>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:1rem;margin-top:1.2rem;">
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;"><h3 style="margin:0 0 .45rem;">중2 내신</h3><p style="margin:0;color:var(--muted);">12주 관리 후 72점 → 91점</p></article>
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;"><h3 style="margin:0 0 .45rem;">고1 모의고사</h3><p style="margin:0;color:var(--muted);">4개월 후 3등급 → 1등급</p></article>
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;"><h3 style="margin:0 0 .45rem;">초6 기초독해</h3><p style="margin:0;color:var(--muted);">8주 후 독해 정확도 63% → 88%</p></article>
    </div>
    <div style="margin-top:1rem;">
      <a class="cta cta-primary" href="https://open.kakao.com/o/demo-link" target="_blank" rel="noopener">우리 아이 케이스 상담받기</a>
    </div>
  </div>
</section>

<section id="reviews" class="section">
  <div class="container">
    <h2 class="section-title">학부모 후기</h2>
    <p class="section-desc">관리 체감과 성과를 동시에 경험한 실제 피드백입니다.</p>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:1rem;margin-top:1.2rem;">
      <blockquote style="margin:0;background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;">"아이 학습 습관이 잡히고 시험 전 불안이 줄었습니다."<br /><small style="color:var(--muted);">중3 학부모</small></blockquote>
      <blockquote style="margin:0;background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;">"점수뿐 아니라 공부 계획을 스스로 세우게 된 점이 가장 좋았습니다."<br /><small style="color:var(--muted);">고1 학부모</small></blockquote>
      <blockquote style="margin:0;background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;">"매주 받는 리포트 덕분에 학습 상태를 정확히 알 수 있어 안심됩니다."<br /><small style="color:var(--muted);">초5 학부모</small></blockquote>
    </div>
  </div>
</section>

<section id="system" class="section">
  <div class="container">
    <h2 class="section-title">수업/관리 방식</h2>
    <p class="section-desc">진단부터 피드백까지 관리가 이어집니다.</p>
    <ol style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:.8rem;margin:1.2rem 0 0;padding:0;list-style:none;">
      <li style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>1. 진단</strong><br /><span style="color:var(--muted);">현재 실력·약점 분석</span></li>
      <li style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>2. 학습설계</strong><br /><span style="color:var(--muted);">개인 목표 기반 로드맵</span></li>
      <li style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>3. 수업</strong><br /><span style="color:var(--muted);">개념-적용-점검 루프</span></li>
      <li style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>4. 점검</strong><br /><span style="color:var(--muted);">주간 진도/과제 관리</span></li>
      <li style="background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:.9rem;"><strong>5. 피드백</strong><br /><span style="color:var(--muted);">학부모 공유 리포트 제공</span></li>
    </ol>
  </div>
</section>
```

- [ ] **Step 4: 테스트 재실행으로 통과 확인**

실행: `grep -n "성적 향상 사례\|학부모 후기\|진단\|학습설계\|점검\|피드백" index.html`
기대 결과: 섹션 제목 및 프로세스 키워드가 모두 포함되어야 한다.

- [ ] **Step 5: 커밋**

```bash
git add index.html
git commit -m "feat: add results, parent reviews, and management flow"
```

### 작업 4: 프로그램, 위치, FAQ, 최종 상시 전환 패턴 구현

**파일:**
- 수정: `index.html`
- 테스트: `index.html`

- [ ] **Step 1: 실패 테스트 작성(남은 필수 섹션 + 반복 CTA 훅 확인)**

```bash
grep -n "프로그램 안내\|초등\|중등\|고등\|오시는 길\|FAQ\|카카오톡 오픈채팅 문의" index.html
```

기대 결과: 최종 콘텐츠 기준으로 일부 문구가 누락되어야 한다.

- [ ] **Step 2: 테스트 실행으로 실패 확인**

실행: `grep -n "프로그램 안내\|초등\|중등\|고등\|오시는 길\|FAQ\|카카오톡 오픈채팅 문의" index.html`
기대 결과: 섹션 상세나 CTA 배치가 완성되지 않은 상태여야 한다.

- [ ] **Step 3: 최소 구현 작성(프로그램 그리드, 위치 정보, FAQ, 모바일 고정 CTA)**

`#programs`, `#location`, `#faq`, `#cta-final`을 업데이트하고 `<body>` 끝에 고정 CTA 추가:

```html
<section id="programs" class="section">
  <div class="container">
    <h2 class="section-title">프로그램 안내</h2>
    <p class="section-desc">학년별 목표에 맞춰 연계형으로 운영합니다.</p>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:1rem;margin-top:1.2rem;">
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;"><h3 style="margin:0 0 .45rem;">초등</h3><p style="margin:0;color:var(--muted);">기초 문해력·어휘·말하기 습관 형성</p></article>
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;"><h3 style="margin:0 0 .45rem;">중등</h3><p style="margin:0;color:var(--muted);">내신 문법·독해·서술형 대응 강화</p></article>
      <article style="background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;"><h3 style="margin:0 0 .45rem;">고등</h3><p style="margin:0;color:var(--muted);">내신+수능 전략 학습 및 실전 점검</p></article>
    </div>
  </div>
</section>

<section id="location" class="section">
  <div class="container">
    <h2 class="section-title">위치 / 오시는 길</h2>
    <p class="section-desc">방문 전 카카오톡으로 예약 문의를 남겨주세요.</p>
    <div style="margin-top:1rem;background:var(--surface);border:1px solid var(--line);border-radius:16px;padding:1rem;">
      <p style="margin:.2rem 0;"><strong>학원명</strong> 프라임 영어학원(데모)</p>
      <p style="margin:.2rem 0;"><strong>주소</strong> 서울시 강남구 테헤란로 00 (데모)</p>
      <p style="margin:.2rem 0;"><strong>운영시간</strong> 평일 14:00-22:00 / 토 10:00-16:00</p>
    </div>
  </div>
</section>

<section id="faq" class="section">
  <div class="container">
    <h2 class="section-title">FAQ</h2>
    <div style="display:grid;gap:.8rem;margin-top:1rem;">
      <details style="background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:.8rem;"><summary>상담은 어떻게 진행되나요?</summary><p style="margin:.6rem 0 0;color:var(--muted);">오픈채팅으로 일정 조율 후 방문 또는 비대면 상담을 진행합니다.</p></details>
      <details style="background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:.8rem;"><summary>반편성 기준이 있나요?</summary><p style="margin:.6rem 0 0;color:var(--muted);">초기 진단 결과와 목표를 기준으로 적합한 반을 안내합니다.</p></details>
      <details style="background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:.8rem;"><summary>보강/결석 관리는 어떻게 하나요?</summary><p style="margin:.6rem 0 0;color:var(--muted);">결석 사유 확인 후 보강 가능한 시간을 조율합니다.</p></details>
      <details style="background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:.8rem;"><summary>학부모 피드백은 주기적으로 받을 수 있나요?</summary><p style="margin:.6rem 0 0;color:var(--muted);">주간 학습 리포트로 과제·진도·개선 포인트를 공유합니다.</p></details>
    </div>
  </div>
</section>

<section id="cta-final" class="section">
  <div class="container" style="background:var(--surface);border:1px solid var(--line);border-radius:18px;padding:1.25rem;">
    <h2 class="section-title" style="margin-bottom:.35rem;">지금, 우리 아이 영어 학습 상담 받아보세요</h2>
    <p class="section-desc">카카오톡 오픈채팅으로 문의를 남기면 빠르게 답변드립니다.</p>
    <div style="margin-top:1rem;">
      <a class="cta cta-primary" href="https://open.kakao.com/o/demo-link" target="_blank" rel="noopener">카카오톡 오픈채팅 문의</a>
    </div>
  </div>
</section>

<a
  href="https://open.kakao.com/o/demo-link"
  target="_blank"
  rel="noopener"
  style="position:fixed;right:1rem;bottom:1rem;z-index:40;background:var(--success);color:#fff;text-decoration:none;padding:.9rem 1rem;border-radius:999px;font-weight:800;box-shadow:0 8px 20px rgba(0,0,0,.15);"
>
  카카오톡 오픈채팅 문의
</a>
```

- [ ] **Step 4: 테스트 재실행으로 통과 확인**

실행: `grep -n "프로그램 안내\|초등\|중등\|고등\|오시는 길\|FAQ\|카카오톡 오픈채팅 문의" index.html`
기대 결과: 필수 섹션과 반복 CTA 문구가 모두 존재해야 한다.

- [ ] **Step 5: 커밋**

```bash
git add index.html
git commit -m "feat: complete programs, FAQ, location, and final CTAs"
```

### 작업 5: 완료 전 반응형 동작 및 앵커/CTA 기능 검증

**파일:**
- 수정: `index.html` (수정이 필요할 때만)
- 테스트: `index.html`

- [ ] **Step 1: 실패 테스트 작성(명시적 합/불 기준의 QA 체크리스트)**

아래 수동 체크리스트를 전체 통과 전까지 실패 게이트로 사용:

```text
[ ] 모바일 뷰포트(~390px): 가로 스크롤 없음
[ ] 데스크톱 뷰포트(~1280px): 섹션 간격/정렬 일관성 유지
[ ] 헤더 네비 앵커가 정확한 섹션으로 이동
[ ] 모든 "카카오톡 오픈채팅 문의" 버튼이 링크를 연다
[ ] 첫 화면 메시지가 안심 + 성과를 명확히 전달한다
```

- [ ] **Step 2: 테스트 실행으로 실패 확인(또는 이슈 식별)**

실행: 브라우저에서 `index.html` 열고 체크리스트 수행.
기대 결과: 최종 다듬기 전 최소 1개 항목은 개선 포인트가 드러날 수 있다.

- [ ] **Step 3: 최소 구현 작성(관측된 이슈만 보정)**

이슈가 있으면 `index.html`에 국소 수정 적용:

```css
/* 예시 보정 패턴 (필요 시에만 적용) */
body { overflow-x: clip; }
section { scroll-margin-top: 80px; }
```

```html
<!-- 예시 앵커/라벨 보정 (필요 시에만 적용) -->
<a href="#programs">프로그램</a>
```

- [ ] **Step 4: 테스트 재실행으로 통과 확인**

실행: 브라우저를 다시 열고 체크리스트 재수행.
기대 결과: 체크리스트 전체 항목 통과.

- [ ] **Step 5: 커밋**

```bash
git add index.html
git commit -m "fix: finalize responsive layout and inquiry flow behavior"
```

## 자기 검토

### 1) 스펙 커버리지 점검

- 1페이지 IA 및 필수 섹션: 작업 1~4에서 반영
- 톤(프리미엄 + 성과 중심): 작업 2~4의 카피/카드 레이아웃으로 반영
- 핵심 강점(1/2/4): 작업 2에서 반영
- 신뢰 요소(성적/후기/관리 방식): 작업 3에서 반영
- 프로그램/위치/FAQ: 작업 4에서 반영
- 카카오 오픈채팅 CTA 반복 배치: 작업 2~4에서 반영
- 모바일 우선 + 검증 기준: 작업 5에서 반영

누락된 스펙 요구사항 없음.

### 2) 플레이스홀더 스캔

- 작업 지시문에 TBD/TODO 없음.
- 코드 변경 단계에 모두 구체 코드 블록 포함.
- 실행 명령과 기대 결과를 명시함.

### 3) 타입/명명 일관성 점검

- 섹션 ID 일관성 유지(`hero`, `strengths`, `results`, `reviews`, `system`, `programs`, `location`, `faq`, `cta-final`).
- CTA 문구 일관성 유지(`카카오톡 오픈채팅 문의`).
- 단일 타깃 파일(`index.html`) 일관성 유지.
