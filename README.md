# 베를리너투어 홈페이지

독일·체코 종교개혁 성지순례 상품을 소개하는 원페이지 정적 홈페이지입니다. GitHub → Cloudflare Pages 연동으로, `main`(또는 배포 브랜치)에 push하면 자동 배포됩니다. 별도 빌드 과정 없이 순수 HTML/CSS/JS로 구성되어 있습니다.

## 폴더 구조

```
/
├── index.html                     현재 오픈 상품(독일·체코 성지순례)이 곧 메인 페이지
├── products/
│   └── germany-czech/index.html   index.html과 동일 내용 (상품별 고유 URL)
├── terms.html                     이용약관 (초안 — 게시 전 법률 검토 필요)
├── privacy.html                   개인정보처리방침 (초안 — 게시 전 법률 검토 필요)
├── products.json                  GNB "상품" 드롭다운에 표시되는 상품 목록 데이터
├── robots.txt                     검색엔진 크롤링 허용 범위 + sitemap 위치 안내
├── sitemap.xml                    검색엔진에 제출할 페이지 목록
├── assets/
│   ├── css/style.css
│   └── js/main.js
└── images/
    ├── hero/           1.jpg, 2.jpg
    ├── destinations/   prague/, herrnhut/, dresden/, berlin/, potsdam/, wittenberg/, leipzig/
    ├── reviews/        1.jpg ~ 8.jpg
    ├── guide/          1.jpg
    ├── buttons/        kakao.png (카카오톡 공식 상담 버튼 이미지)
    └── og-image.jpg
```

## 사진 교체 방법 (운영자용)

코드를 건드릴 필요 없이 **`images/` 폴더 안의 파일을 같은 이름으로 덮어쓰고 git push**하면 됩니다.

1. 새 사진을 준비합니다 (권장: 가로형 JPG, 방문지/후기 사진은 4:3 비율, 히어로 사진은 16:9 비율, 가이드 사진은 1:1 비율).
2. 아래처럼 **기존 파일과 정확히 같은 경로·파일명**으로 저장합니다.
   - 예) 프라하 사진 교체 → `images/destinations/prague/1.jpg` 파일을 새 사진으로 덮어쓰기
   - 예) 후기 카드 3번째 사진 교체 → `images/reviews/3.jpg` 덮어쓰기
3. `git add images/... && git commit -m "사진 업데이트" && git push` 하면 Cloudflare Pages가 자동으로 재배포합니다.

> 현재 `images/` 폴더에는 실제 사진이 아직 없어 자리표시용(placeholder) 이미지가 들어가 있습니다. 위 방법대로 실제 사진으로 교체해 주세요.

## 카카오톡 상담 버튼 이미지 교체

카카오는 채널/상담 버튼에 대해 색상·비율·여백 등 엄격한 디자인 가이드를 두고 있어, 코드로 임의 제작하지 않고 **카카오에서 제공하는 공식 버튼 이미지 파일을 그대로 사용**하는 구조로 되어 있습니다.

1. 카카오 비즈니스 채널 관리자센터 등에서 제공하는 공식 버튼 이미지 파일을 준비합니다.
2. `images/buttons/kakao.png` 파일을 그 이미지로 **동일한 파일명으로 덮어쓰기**합니다.
3. `git add images/buttons/kakao.png && git commit -m "카카오 버튼 이미지 교체" && git push`

버튼은 히어로, 문의하기 섹션, 모바일 하단 고정바 총 3곳에서 같은 파일 하나를 공유하므로 **한 번만 교체하면 전체 반영**됩니다. 이미지의 실제 가로세로 비율에 따라 버튼이 자동으로 맞춰지며, 필요시 `assets/css/style.css`의 `.kakao-btn-img` (및 `.contact-cta-row .kakao-btn-img`, `.kakao-btn-link--fab .kakao-btn-img`) 의 `height` 값으로 크기를 조정할 수 있습니다.

> 현재는 실제 파일이 없어 노란색 자리표시용 이미지가 들어가 있습니다.

## 후기(리뷰) 텍스트/문구 수정

`index.html`과 `products/germany-czech/index.html`의 `id="reviews"` 섹션에서 각 `<article class="review-card">` 블록의 문구·별점·태그를 직접 수정합니다. 두 파일이 동일한 내용이므로 **양쪽 파일 모두 수정**해야 합니다.

## 새 상품 추가 방법

1. `products/새상품슬러그/` 폴더를 만들고 그 안에 `index.html`을 작성합니다 (기존 `products/germany-czech/index.html`을 복사해서 내용만 수정하는 방식을 권장합니다).
2. `products.json`에 새 상품 항목을 한 줄 추가합니다.

```json
[
  { "slug": "germany-czech", "name": "독일·체코 성지순례", "url": "/products/germany-czech/", "status": "판매중" },
  { "slug": "새상품슬러그", "name": "새 상품명", "url": "/products/새상품슬러그/", "status": "오픈예정" }
]
```

`products.json`을 수정하면 모든 페이지 상단 GNB "상품" 드롭다운에 자동으로 반영됩니다 (코드 수정 불필요). 단, `fetch`가 실패하는 환경(예: 로컬에서 `file://`로 직접 열었을 때)에서는 각 HTML의 `<ul class="nav-dropdown" data-products-dropdown>` 안에 있는 정적 항목이 대신 표시되므로, 신규 상품 추가 시 이 정적 목록도 함께 갱신해 두는 것을 권장합니다.

## 로컬 미리보기

빌드 과정이 없으므로 정적 서버로 폴더를 열면 됩니다.

```bash
python3 -m http.server 8000
# http://localhost:8000 접속
```

(주의: `fetch('/products.json')`은 `file://`로 직접 여는 경우 동작하지 않으므로, 반드시 로컬 서버를 통해 확인하세요.)

## Cloudflare Pages 배포 설정

- Build command: 없음 (Framework preset: None)
- Build output directory: `/` (저장소 루트)
- 이 저장소를 Cloudflare Pages 프로젝트에 연결하고 배포 브랜치를 지정하면, 이후 해당 브랜치에 push할 때마다 자동으로 재배포됩니다.

## 검색엔진 색인 등록 (네이버 서치어드바이저 / 구글 서치콘솔)

`robots.txt`, `sitemap.xml`이 이미 사이트 루트에 있으니, 아래 순서로 등록하면 됩니다.

1. **구글 서치콘솔** (https://search.google.com/search-console)
   - 속성 추가 → "URL 접두어" 방식으로 `https://berlinertour.pages.dev/` 입력 (또는 실제 커스텀 도메인)
   - 소유권 확인 (HTML 태그 방식이 가장 간단 — 발급받은 메타태그를 `index.html`, `products/germany-czech/index.html`의 `<head>`에 추가해달라고 요청하면 제가 넣어드립니다)
   - 확인 후 왼쪽 메뉴 "Sitemaps" → `sitemap.xml` 입력 후 제출
2. **네이버 서치어드바이저** (https://searchadvisor.naver.com)
   - 사이트 등록 → 위와 동일한 주소 입력
   - 소유확인 (HTML 파일 업로드 또는 메타태그 방식 — 발급받은 값을 알려주시면 적용해드립니다)
   - "요청 → 사이트맵 제출"에서 `sitemap.xml` 제출

> **커스텀 도메인을 나중에 연결하시면**, `robots.txt`의 Sitemap 주소, `sitemap.xml`의 각 `<loc>`, 그리고 모든 페이지의 `<link rel="canonical">`을 새 도메인으로 함께 바꿔야 합니다 — 이때도 말씀해주시면 한 번에 바꿔드립니다.

## 남은 작업 (게시 전 확인사항)

- [ ] `images/` 폴더의 자리표시용 사진을 실제 사진으로 교체
- [ ] `images/buttons/kakao.png`를 카카오 공식 상담 버튼 이미지로 교체
- [ ] `terms.html`, `privacy.html` 내용 법률 검토 (관광진흥법 표준약관 준수 여부 포함)
- [ ] 여행 후기 원본 사진 확보 후 `images/reviews/` 교체, 리뷰 플랫폼 UI가 포함된 캡처본은 크롭 후 사용 권장
- [ ] 상품가 최신 정보로 재확인 (현재 "문의(카카오톡)"로 안내 중, 전화번호는 노출하지 않음)
