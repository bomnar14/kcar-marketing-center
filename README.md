# K Car 고객 데이터 커맨드 센터 (외부 공유용)

Salesforce Data 360 · Headless360 조회 결과로 만든 정적 대시보드입니다. 로그인 없이 브라우저에서 열립니다.

## 구성

```
index.html        화면 (탭, 차트, 시뮬레이터)
data/data.json    숫자와 문구 (갱신 시 이 파일만 바뀌는 경우가 대부분)
vendor/d3.min.js  차트 라이브러리 (d3 v7.9.0, ISC 라이선스)
```

- 외부 네트워크가 필요한 것은 웹폰트(Pretendard, jsDelivr)뿐이며, 막혀 있으면 시스템 한글 글꼴로 표시됩니다.
- 개인 단위 데이터(이름·전화번호·고객키)는 포함하지 않습니다. 집계값과 Claude가 작성한 캠페인 초안만 들어 있습니다.

## 내 컴퓨터에서 확인

`data.json`을 불러오기 때문에 파일을 더블클릭하면 열리지 않습니다. 이 폴더에서:

```
python3 -m http.server 8000
```

실행 후 http://localhost:8000 으로 접속합니다.

## 배포

### GitHub Pages (가장 간단)
1. 이 폴더를 저장소 루트에 올립니다.
2. Settings → Pages → Source: `Deploy from a branch`, Branch: `main` / `/ (root)`.
3. 주의: 저장소가 비공개여도 Pages 사이트는 URL을 아는 누구나 볼 수 있습니다 (Enterprise의 비공개 Pages 제외).

### 접근 제한이 필요하면
- AWS S3 + CloudFront (서명 URL 또는 Lambda@Edge 인증)
- Netlify / Cloudflare Pages (비밀번호 · Access 정책)

## 갱신 절차

1. Claude에게 "커맨드 센터 갱신해줘"라고 요청합니다 (조회 계획 → 승인 → 실행).
2. Claude가 Claude 아티팩트와 이 저장소용 `data/data.json`(필요 시 `index.html`)을 함께 만듭니다.
3. 받은 파일로 교체 후 commit · push 하면 Pages가 자동 배포합니다.
   GitHub 커넥터가 연결되면 2~3단계는 Claude가 승인 후 직접 commit 합니다.

> 운영 규칙: 수정은 Claude 요청으로만 합니다. Claude 아티팩트나 이 저장소를 손으로 고치면 두 곳이 서로 달라집니다.
