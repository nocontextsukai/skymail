# 📬 Mailbox (정적 메일함 뷰어)

EML 백업 메일 684통을 iOS 메일앱 스타일의 정적 웹 페이지로 모아둔 프로젝트입니다.
서버 없이 `index.html` 하나로 동작하며, 자료는 `data/`, 이미지는 `assets/` 폴더에 들어있습니다.

## 📁 폴더 구조

```
mailbox/
├── index.html              ← 메인 페이지 (열면 바로 inbox)
├── data/
│   ├── index.json          ← 메일 목록 (제목/보낸이/날짜/미리보기)
│   └── mails/{id}.json     ← 메일별 본문(HTML)·첨부 정보
├── assets/{id}/...         ← 메일별 인라인 이미지·첨부파일
└── README.md
```

- 총 메일: **684통**
- 기간: **2022-09-11 ~ 2026-05-12**
- 인라인 이미지 등 자산: 약 **116 MB**

## 🔒 개인정보 처리

- 수신자(내) 이메일 `rascosmic@naver.com` → `****@****`로 마스킹
- `Return-Path`, `Received`, `DKIM`, `Message-ID` 등 기술 헤더 모두 제거
- 본문 안 `<script>`·`<iframe>`·`on*=` 인라인 이벤트·`javascript:` 링크 제거
- 본문은 sandboxed `<iframe>` 안에서 렌더링 (호스트 페이지 스타일/스크립트 격리)

## 🚀 로컬에서 미리보기

브라우저는 보안상 로컬 파일에서 fetch()를 막을 때가 많아서, 그냥 `index.html`을 더블클릭하면 빈 화면이 나올 수 있습니다. 간단한 로컬 서버로 띄워주세요.

```bash
# 이 폴더 안에서 (Python 3 필요)
python -m http.server 8000
```

브라우저에서 `http://localhost:8000/` 열면 끝.

## 🌐 GitHub Pages 배포

### 1) 새 저장소 만들기

GitHub에서 `Repositories → New` → 이름 예: `mailbox` → **Public** → Create.

### 2) 이 폴더를 통째로 업로드

`mailbox` 폴더 안의 모든 파일(`index.html`, `data/`, `assets/`, `README.md`)을 저장소 루트에 업로드합니다. 웹 UI에서 드래그-앤-드롭으로 올리거나, Git을 쓴다면:

```bash
cd mailbox
git init
git add .
git commit -m "initial mailbox snapshot"
git branch -M main
git remote add origin https://github.com/<USER>/<REPO>.git
git push -u origin main
```

> 자산 폴더가 약 116MB라 일반 푸시에는 문제없지만, 100MB 넘는 단일 파일은 없도록 신경 써주세요 (현재 가장 큰 단일 파일은 ~5MB 수준).

### 3) Pages 설정

저장소 페이지 → `Settings → Pages` → **Source: Deploy from a branch** → **Branch: `main` / `/ (root)`** → Save.

1~2분 뒤 `https://<USER>.github.io/<REPO>/` 로 접속하면 공개됩니다.

### 4) URL 공유

iOS Safari로 열면 진짜 메일앱 느낌이 가장 잘 살아납니다 (다크모드도 자동).

## ⚠️ 주의

- 본문 안에 원본 메일이 외부 이미지(트래커)를 참조하는 경우, 페이지를 열 때 해당 외부 서버에 요청이 갑니다. 추적·로깅 가능성 있음 (`referrerpolicy="no-referrer"`로 referer는 보내지 않음).
- 메일 발신자(보낸이) 이름·이메일·본문은 원본 그대로 남아있습니다. 공개 시점에 민감 정보가 있는지 한 번 더 확인하세요.
- 본 페이지는 **읽기 전용 백업 뷰**이며, 답장·삭제·검색 외의 기능은 동작하지 않습니다.
