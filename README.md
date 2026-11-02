# 하이런치 2.0 — GitHub 직원 참여 화면

하이런치 2.0 직원 참여 화면의 **정적 프런트엔드** 저장소입니다. 서버 데이터는 GitHub에 저장하지 않습니다.

## 구성

- `index.html`: 직원용 운영 화면. **운영용** Cloudflare Worker `hilunch-v2-live-api`를 호출합니다.
- `production-staging.html`: 운영 화면의 동일 버전 원본(비교·복구 참고용).
- `connected-test.html`: **테스트용** Cloudflare Worker 및 테스트DB 연결 페이지.
- `api-check.html` / `api-save-test.html`: 테스트용 통신·저장 검사 페이지.

운영 데이터 흐름: **직원 브라우저 → GitHub Pages 정적 화면 → Cloudflare Worker(운영 API) → Apps Script → Google Sheets 운영DB**

GitHub 저장소에는 직원 개인 링크, 토큰, Apps Script 배포 URL, API 보안키, 실제 응답 데이터를 넣지 않습니다.

## 배포 확인

1. GitHub **Settings → Pages**에서 `main` 브랜치 루트(`/`)를 배포 소스로 설정하고 공개 여부 및 배포 상태를 확인합니다. (이미 설정되어 있으면 재설정 불필요)
2. 배포 화면: `https://hj112411.github.io/hilunch-v2/` (**GitHub Pages가 활성화된 경우**).
3. Cloudflare 운영용 Worker의 `GAS_WEBAPP_URL`, `HILUNCH_API_SECRET`이 **운영용 Apps Script**를 가리키는지 확인합니다. 설정값을 저장소에 넣거나 화면에 출력하지 않습니다.
4. 허용된 브라우저 출처에 GitHub Pages의 도메인이 포함되어 있어야 합니다. 새 버전 배포 뒤 실제 브라우저의 API 연결을 확인합니다.

## 직원별 새 링크 (비공개 배포)

`https://hj112411.github.io/hilunch-v2/#g=<그룹토큰>&p=<개인토큰>`

- 직원 개인토큰은 URL **해시(#)** 뒤에 둡니다. 브라우저는 이 부분을 GitHub Pages 서버에 요청 경로로 보내지 않습니다.
- **기존 Apps Script 개인 QR 링크는 자동으로 GitHub 주소로 바뀌지 않습니다.** 기존 링크·QR은 계속 그대로 두고, 새 링크의 운영DB 읽기·저장·수정·확정 기능이 확인된 뒤 새 QR을 개별 전달해야 합니다.
- 링크 전체를 공개 이슈, README, 스크린샷, 단체 채팅에 게시하지 마세요.
- 앱 화면에는 기존 Apps Script 개인 링크를 입력해 여는 대체 진입 방식도 있습니다.

## 운영 전 확인 (실제 데이터는 최소한으로 테스트)

- [ ] GitHub Pages에서 `index.html`이 열리고 테스트용 안내 문구가 표시되지 않는지
- [ ] 운영 Worker가 배포되어 있고 API 호출에 CORS·네트워크 오류가 없는지
- [ ] 본인 전용 개인 링크로 정확한 그룹·이름·역할이 열리는지
- [ ] 읽기·응답 저장·새로고침 후 재조회·수정·집계가 동일한 운영DB에 반영되는지
- [ ] 신청자 최종 일정 확정/수정과 권한 제한이 동작하는지 (실제 업무 진행 단계에 맞춰 확인)
- [ ] 테스트DB에 운영 직원 데이터가 기록되지 않는지
- [ ] 실제 참여자 공지 및 새 QR 배포 전에 기존 URL의 지속 동작 여부를 재확인했는지

**중요:** 코드가 운영 API 주소를 호출하도록 변경되었다는 사실은 운영 Worker/DB가 실제 동작한다는 검증을 대신하지 않습니다. 새 QR은 검증 후 배포합니다.

## 되돌리기

이전 `index.html`은 GitHub 커밋 이력에 남아 있어 되돌릴 수 있습니다. 운영 장애 시 QR 재배포를 중단하고, 운영 API 및 변경 커밋을 점검합니다. 기존 Apps Script 링크는 이 저장소 변경만으로 영향을 받지 않습니다.
