+++
title = "Emacs Org-mode + Hugo + Github-page 로 블로그 만들기"
author = ["KIM TAEHEON"]
date = 2025-03-16
lastmod = 2025-03-16T23:45:29+09:00
tags = ["emamcs", "org-mode", "hugo", "github-page"]
draft = false
+++

emacs org-mode로 글 쓰는 걸 좋아해서 이를 활용한 블로깅 방법에 대해 알아봤다.
org파일을 md파일로 바꾸고 md파일로 정적 사이트를 생성하는 방식이다.

정적 사이트를 생성해주는 툴은 여러가지인걸로 알고 있는데,
org파일을 ox-hugo 패키지로 md파일 변환할 수 있는 장점이 있어 hugo로 선택했다.

무료로 호스팅하기 위한 방법으론 Github-page를 선택했다.
`[깃허브계정].github.io` 도메인으로 서비스 가능하다.


## hugo 설치 {#hugo-설치}

맥환경이라 brew를 이용해 설치했다.

```bash
brew install hugo
```


## hugo 프로젝트 생성 {#hugo-프로젝트-생성}

`[깃허브계정].github.io` 패턴으로 프로젝트를 생성한다.
`jake88dev` 가 깃허브 계정이다.

```bash
hugo new site jake88dev.github.io --format yaml
cd jake88dev.github.io
```

`--format yaml` 옵션은 [PaperMod 테마의 깃허브 페이지 &gt; 위키](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation)에 설치법대로 적용한 것이다.
저 옵션을 안주면 `hugo server -D` 로 로컬 서버를 실행했을 때 `Page not found` 가 뜬다.


## 테마 적용 {#테마-적용}

ChatGPT에게 ox-hugo와 호환이 잘되는 테마를 추천해달라고 질문하니, `PaperMod` 를 추천했다.
아래 코드로 설치.

```bash
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

`hugo.yaml` 파일에 아래 설정 추가.

```yaml
theme: ["PaperMod"]
```


## 포스트 파일 추가. {#포스트-파일-추가-dot}

`content` 폴더 하위에 `org` 폴더를 만들고 블로그 글을 추가한다.
header에는 다음과 같은 내용을 적어준다.
각 역할은 주석을 참고.

```text
#+hugo_base_dir: ../..       # Hugo 프로젝트의 루트 디렉토리를 설정. org파일을 md파일로 변환시 유용.
#+HUGO_SECTION: ./posts      # 변환된 md파일이 저정될 폴더.
#+HUGO_WEIGHT: auto          # 포스트 정렬순서를 결정하는 가중치. auto이면 최신글이 우선 정렬됨.
#+HUGO_AUTO_SET_LASTMOD: t   # org파일 수정시 Last Modified(lastmod)값이 자동으로 설정됨.
#+HUGO_TAGS: emamcs hugo     # 태그 적용
#+TITLE: Emacs Org-mode...   # 포스트 제목
#+AUTHOR: KIM TAEHEON        # 저자
#+DATE: 2025-03-16           # 포스팅 날짜
```

그리고 하위에 org모드로 글쓰듯이 내용을 적어주면 된다.


## md파일 변환 {#md파일-변환}

ox-hugo 패키지가 필요하다. emacs 사용자가 아니면 이해가 어려운 내용.
config 파일에 아래 내용을 추가했다.

```elisp
(use-package ox-hugo
  :ensure t
  :after ox
  :config
  (setq org-hugo-default-section-directory "posts"))
```

그리고 변환할 파일에서 `C-c C-e H H` 를 실행하면 md파일로 변환된다.

{{< figure src="/images/20250316-210810_screenshot.png" >}}


## hugo.yaml 수정 {#hugo-dot-yaml-수정}

[PaperMod 테마의 깃허브 페이지 &gt; 위키](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation)에 `hugo.yml` 샘플이 있는데,
뭘 설정한건지 잘 모르겠고 필수가 아닌것도 있는거 같아서
ChatGPT에게 필요한 설정만 골라달라고 했다.

```yaml
baseURL: https://jake88dev.github.io
languageCode: en-us
title: My New Hugo Site
theme: ["PaperMod"]

enableRobotsTXT: true # 검색 엔진 크롤링 허용

params:
  mainSections: ["posts"] # 홈 페이지에서 표시할 기본 섹션
  defaultTheme: auto  # 다크 모드 자동 설정
  ShowBreadCrumbs: true  # 페이지 상단에 탐색 경로 표시
  ShowCodeCopyButtons: true  # 코드 블록에 복사 버튼 추가
  ShowToc: true  # 본문에 목차 표시

outputs:
  home:
    - HTML
    - RSS
    - JSON

markup:
  goldmark:
    renderer:
      unsafe: true  # HTML 태그가 포함된 마크다운을 허용
  highlight:
    noClasses: false  # 코드 하이라이팅 설정

```


## 실행화면 {#실행화면}

`hugo server -D` 명령어를 실해하면 <http://localhost:1313> 으로 로컬서버를 띄울 수 있다.

-   첫페이지

{{< figure src="/images/20250316-234231_screenshot.png" >}}

-   블로그 페이지

{{< figure src="/images/20250316-234330_screenshot.png" >}}


## 참고 {#참고}

-   <https://yeongcheon.github.io/posts/2019-03-03-github+hugo+orgmode_setup/>
-   <https://github.com/kaushalmodi/ox-hugo/>
-   <https://github.com/adityatelange/hugo-PaperMod/wiki/Installation>
