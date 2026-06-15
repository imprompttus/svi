# SVI 측정툴 배포 안내

## 배포 파일

- `index.html`

이 사이트는 HTML 파일 하나로 실행됩니다. GitHub 저장소에는 우선 `index.html`만 올려도 됩니다.

## 추천 배포 방식

1. GitHub에 새 저장소를 만듭니다.
2. `index.html`을 업로드합니다.
3. Cloudflare Pages에서 GitHub 저장소를 연결합니다.
4. Build command는 비워둡니다.
5. Output directory는 `/`로 설정합니다.
6. Deploy를 누릅니다.

## 배포 후 주소

Cloudflare Pages는 보통 아래와 같은 주소를 제공합니다.

`https://프로젝트명.pages.dev/`

## 수정 후 재배포

나중에 내용을 고치면 GitHub에서 기존 `index.html`을 새 파일로 다시 업로드하고 Commit 하면 Cloudflare Pages가 자동으로 다시 배포합니다.
