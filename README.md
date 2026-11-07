# bp-portfolio — 문도경 개인 소개 사이트

전북대학교 컴퓨터공학부 문도경의 개인 소개 웹사이트입니다.
Hugo 테마 [hugo-coder](https://github.com/luizdepra/hugo-coder)로 만들고, GitHub Actions로 GitHub Pages에 자동 배포합니다.

| 항목 | 주소 |
|---|---|
| 공개 사이트 | https://do-ttery.github.io/bp-portfolio/ |
| 저장소 | https://github.com/do-ttery/bp-portfolio |
| 배포 기록 (Actions) | https://github.com/do-ttery/bp-portfolio/actions |

사이트 구성: 홈(이름·소개) · [소개](https://do-ttery.github.io/bp-portfolio/about/)(관심 분야·사용 기술·경력) · [활동](https://do-ttery.github.io/bp-portfolio/projects/)(대표 활동 빨래집사)

> 테마 원본 README는 `site/themes/template/README.md`에 그대로 남아 있습니다.

## 폴더 구조

실제로 빌드되는 사이트는 **`site/` 폴더 하나**입니다. 저장소 맨 위의 `archetypes/`, `assets/`, `layouts/`, `exampleSite/` 등은 내려받은 테마 ZIP 원본이며 빌드에 쓰이지 않습니다.

```text
bp-portfolio/
├── site/                         # ← 실제 사이트 (빌드 대상)
│   ├── hugo.toml                 #   사이트 설정: 제목·소개 문구·메뉴·GitHub 링크
│   ├── pages.json                #   배포 주소(baseURL) 등 Pages용 설정 (수업 스크립트 생성)
│   ├── content/
│   │   ├── about.md              #   소개 페이지
│   │   └── projects.md           #   활동 페이지
│   ├── layouts/_partials/        #   테마 일부를 덮어쓴 파일 (아이콘 링크·메뉴·다크모드 버튼 접근성 수정)
│   ├── assets/css/a11y.css       #   접근성 보완 CSS (모바일 메뉴 키보드 조작)
│   ├── static/images/            #   파비콘
│   └── themes/template/          #   hugo-coder 테마 복사본 (빌드에 사용)
├── .github/
│   ├── workflows/hugo-pages.yml  # main에 push하면 빌드·배포하는 GitHub Actions 워크플로
│   └── pages_build.py            # 워크플로가 실행하는 빌드 스크립트 (하위 경로 보정 포함)
├── .github-pages.json            # 빌드 설정 기록 (Hugo 버전·설정 파일·배포 주소)
├── prepare_github_pages.py       # 처음 한 번 site/·워크플로를 만든 수업 제공 스크립트
├── LICENSE.md                    # hugo-coder 테마 MIT 라이선스
└── (archetypes/ assets/ layouts/ exampleSite/ … )  # 테마 ZIP 원본 — 빌드에 미사용
```

## 필요한 도구와 설치

| 도구 | 배포(GitHub Actions)에서 쓰는 버전 | 로컬 확인 환경 (macOS) | 용도 |
|---|---|---|---|
| Hugo **extended** | 0.167.0 | 0.169.0 (Homebrew) | 사이트 빌드·미리보기. 테마가 SCSS를 쓰므로 extended 판이 필요 |
| Python | 3.12 | 3.14.5 | 빌드 스크립트 `.github/pages_build.py` 실행 |
| Git | — | 2.54.0 | 저장소 받기·커밋·push |
| Dart Sass | 1.105.0 | 설치 안 함 | 배포 환경에서만 설치. 로컬은 Hugo extended만으로 빌드됨 |

배포 환경 버전은 `.github/workflows/hugo-pages.yml`(`HUGO_VERSION`)과 `.github-pages.json`(`version`)에 고정되어 있습니다.
로컬 Hugo(0.169.0)는 배포용(0.167.0)보다 새 버전입니다. 로컬 빌드 결과를 공개 사이트와 비교했을 때 홈의 `<meta name=generator>`(Hugo 버전 표시) 한 줄 외에는 같았습니다 (2026-11-07 확인).

**macOS 설치** ([Homebrew](https://brew.sh/) 필요)

```bash
brew install hugo git     # Homebrew의 hugo는 extended 판
hugo version              # 출력에 "+extended"가 있는지 확인
python3 --version         # macOS에 기본 설치된 Python 3 사용
```

## 로컬에서 확인하기

```bash
git clone https://github.com/do-ttery/bp-portfolio.git
cd bp-portfolio
```

**미리보기** — 파일을 저장하면 브라우저에 바로 반영됩니다. 종료는 `Ctrl + C`.

```bash
cd site
hugo server --config hugo.toml,pages.json -b http://localhost:1313/bp-portfolio/ --appendPort=false
```

브라우저에서 http://localhost:1313/bp-portfolio/ 를 엽니다. 공개 사이트와 같은 하위 경로(`/bp-portfolio/`)로 띄워 경로 문제를 미리 볼 수 있습니다.

**배포와 같은 방식으로 빌드** — GitHub Actions가 실행하는 스크립트를 그대로 실행합니다 (저장소 맨 위에서).

```bash
python3 .github/pages_build.py --build
```

결과는 `public/`에 생기며 `빌드 완료. 프로젝트 주소 경로 보정: N 개 파일`이 출력되면 성공입니다. `public/`과 `site/public/`은 `.gitignore`로 제외되어 커밋되지 않습니다.

> 주의: 빌드 스크립트는 Hugo 캐시를 정리(`--gc`)하므로, 미리보기 서버가 켜진 상태에서 실행하면 미리보기가 오류(500)를 낼 수 있습니다. 그때는 미리보기를 `Ctrl + C`로 끄고 다시 실행하면 됩니다.
