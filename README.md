# 대주모 클럽 골프앱

월례 라운딩 관리 웹앱(PWA)입니다. 회원은 https://vincens74.github.io/golf/ 를 열어 홈 화면에 추가하면 앱처럼 쓸 수 있습니다.

- 앱 화면과 동작: `index.html`
- 오프라인·업데이트 처리: `sw.js` (앱을 고치면 `CACHE` 번호를 올려주세요)
- 데이터: Firebase Realtime Database (`members`, `rounds`, `records`, `rules`, `meta`)
- 보안 규칙: `database.rules.json` (누구나 볼 수 있고, 총무 계정만 수정 가능)

## 총무 로그인 구조

1. 앱에서 총무 비밀번호를 입력하면, 코드에 저장된 암호화 값(SHA-256)과 비교합니다.
2. 맞으면 Firebase에 총무 계정(`admin@daejumo.club`, 비밀번호 `daejumo` + 총무 비밀번호)으로 로그인합니다.
3. Firebase 보안 규칙이 이 계정만 저장·삭제를 허용합니다.

## 총무 비밀번호 변경

새 비밀번호가 `NEW`라면:

1. 암호화 값 만들기 — 터미널에서
   `python3 -c "import hashlib;print(hashlib.sha256('daejumo-club:NEW'.encode()).hexdigest())"`
2. `index.html`의 `ADMIN_PW_HASH` 값을 결과로 바꾸기
3. Firebase 콘솔 → Authentication → 사용자 `admin@daejumo.club` → 비밀번호를 `daejumoNEW`로 재설정
4. `sw.js`의 `CACHE` 번호를 올리고 배포
