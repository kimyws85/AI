# 사이트 구조 (순수 HTML/CSS/JS)

빌드 도구, 프레임워크, npm 없이 브라우저에서 바로 열리는 정적 사이트 기준 구조.
페이지 1개짜리든 여러 개짜리든 이 구조를 기본 골격으로 쓰고, 필요 없는 부분은 빼면 된다.

## 폴더/파일 구조

```
web/
├── <사이트명>/
│   ├── index.html          # 첫 페이지 (홈)
│   ├── about.html          # 페이지가 여러 개면 이런 식으로 추가
│   ├── css/
│   │   ├── reset.css       # 브라우저 기본 스타일 초기화 (선택)
│   │   └── style.css       # 실제 디자인. :root 변수로 색상/폰트 모아두기
│   ├── js/
│   │   └── script.js       # 페이지 동작 (메뉴 토글, 스크롤 등)
│   └── assets/
│       ├── images/
│       └── fonts/
```

- 페이지가 하나뿐이면 `about.html` 같은 추가 페이지 없이 `index.html` 하나로 충분하다.
- CSS/JS가 아주 작으면 (한 페이지에 50줄 미만) 굳이 폴더로 안 쪼개고 `style.css`, `script.js`를 루트에 바로 둬도 된다. 페이지가 여러 개거나 규모가 커질 때만 `css/`, `js/`로 분리.

## HTML 페이지 기본 골격

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[페이지 제목]</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

<header>...</header>

<main>
  <!-- 섹션 단위로 구성. 페이지 성격에 따라 section 개수는 자유 -->
  <section id="...">...</section>
</main>

<footer>...</footer>

<script src="js/script.js"></script>
</body>
</html>
```

여러 페이지를 만들 때는 `header`/`footer`는 페이지마다 똑같이 복사해서 넣는다 (빌드 도구가 없으니 컴포넌트화는 불가능 — 대신 구조를 동일하게 유지해서 나중에 통째로 바꾸기 쉽게 관리).

## CSS 작성 원칙

- 상단에 `:root { --color-*, --font-*, --max-width, ... }`로 디자인 토큰을 모아두고, 본문에서는 값 대신 변수를 참조한다. (색상 하나 바꿀 때 전체 파일을 뒤지지 않게)
- 반응형은 모바일 폭(~400px) 기준으로 한 번은 반드시 확인. 미디어쿼리는 필요한 곳에만 최소로.
- 페이지 전체 배경/글자색은 `body`에 명시적으로 지정 (브라우저 기본값에 기대지 않기).

## JS 작성 원칙

- 빌드 도구가 없으므로 `import`/`export` 모듈 문법 대신 `<script>` 태그로 파일을 순서대로 로드하거나, 꼭 모듈이 필요하면 `<script type="module">`을 쓴다.
- DOM 조작은 `DOMContentLoaded` 이후에 실행되도록 하거나, `<script>` 태그를 `</body>` 직전에 둬서 별도 이벤트 리스너 없이도 DOM이 준비된 상태로 실행되게 한다.
- 외부 라이브러리(CDN)는 오프라인에서도 열어볼 수 있어야 하면 지양. 꼭 필요하면 최소한으로만.

## 체크리스트 (사이트 완성 전 확인)

- [ ] 모바일 폭(~400px)에서 레이아웃이 깨지지 않는지 확인
- [ ] 모든 링크/버튼이 실제로 동작하는지 (placeholder 링크는 `#`으로 남겨두고 표시)
- [ ] `<title>`, `<meta viewport>`가 모든 페이지에 있는지
- [ ] 이미지에 `alt` 속성이 있는지
- [ ] 콘솔 에러 없이 로드되는지

## 배포

정적 사이트라 GitHub Pages로 바로 배포 가능. 절차는 [[github-cli]] 스킬 참고 (레포 생성 → 푸시 → `gh api repos/<계정>/<레포>/pages`로 Pages 활성화).
