# 🍶 짠! 소주 따르기

휴대폰 두 대를 이용하는 GitHub Pages + Firebase Realtime Database 게임입니다.

## 1. Firebase 설정

1. Firebase Console에서 프로젝트를 엽니다.
2. **빌드 → Realtime Database → 데이터베이스 만들기**를 선택합니다. (Firestore가 아니라 Realtime Database입니다.)
3. **프로젝트 설정 → 내 앱 → 웹 앱 추가**에서 `firebaseConfig`를 확인합니다.
4. `index.html` 맨 아래쪽의 `firebaseConfig`에서 `apiKey`, `authDomain`, `databaseURL`, `projectId`, `appId`를 실제 값으로 교체합니다. `databaseURL`은 Realtime Database 화면에서 제공하는 정확한 주소를 사용하세요.
5. **빌드 → Authentication → 시작하기 → 로그인 방법**에서 **익명(Anonymous)** 로그인을 사용 설정합니다. 게임은 접속할 때 자동으로 익명 로그인합니다.
6. Realtime Database **규칙** 탭에 (규칙 파일이 바뀔 때마다 다시) [`database.rules.json`](database.rules.json) 내용을 그대로 붙여넣고 게시합니다.

### 보안 규칙이 막는 것

- 로그인하지 않은 요청은 읽기·쓰기 모두 거부되고, 전체 방 목록은 누구도 읽을 수 없습니다.
- 방은 만든 사람(잔, `glassUid`)만 지우거나 잔의 양을 바꿀 수 있고, 이미 있는 방 코드를 덮어쓸 수 없습니다.
- 병 자리는 비어 있을 때 들어온 한 명(`bottleUid`)만 차지하며, 그 사람은 `bottleOnline`·`pouring`·`aim`·`tilt`·`dir`·`poured`만 바꿀 수 있고 잔은 비우기(`amount` = 0)만 할 수 있습니다.
- 정해진 필드와 타입·범위 밖의 값은 저장되지 않습니다.

**주의:** 방 코드를 아는 로그인 사용자는 그 방 데이터를 읽을 수 있습니다. 개인정보를 저장하지 마세요. 스크립트로 익명 로그인해 방을 만드는 것까지 막으려면 Firebase App Check를 추가로 설정하세요.

## 2. GitHub Pages 배포

1. GitHub에서 새 공개 저장소를 만듭니다. 예: `soju-pour`.
2. 수정한 `index.html` 파일을 저장소 최상위에 업로드하고 커밋합니다.
3. **Settings → Pages → Build and deployment → Deploy from a branch → main / (root) → Save**.
4. 잠시 후 `https://사용자명.github.io/soju-pour/`에서 접속합니다.

## 3. 플레이

1. 첫 번째 폰에서 **게임 시작하기**를 누릅니다. 이 폰이 소주잔이 되고 화면에 QR 코드가 뜹니다.
2. 두 번째 폰 카메라로 QR 코드를 찍으면 바로 소주병으로 입장합니다. QR 대신 시작 화면에서 방 코드를 입력해도 됩니다. (잔 화면 왼쪽 위를 누르면 방 번호가 복사됩니다.)
3. 소주병 폰을 똑바로 세우고 **확인 · 센서 설정!**을 눌러 권한을 허용합니다. iPhone은 버튼을 누른 뒤에만 센서 권한을 요청할 수 있습니다.
4. 소주잔 폰을 병을 기울일 쪽에 두고, 잔을 드래그해 물줄기가 잔에 닿도록 맞춥니다.
5. 뚜껑을 열고 소주병 폰을 기울여 잔을 채웁니다. 센서가 없는 기기는 **센서 없이 버튼으로 하기**를 누른 뒤 버튼을 길게 눌러 따릅니다.

## 알아둘 점

- 기울기 센서만으로 **두 기기의 실제 상대 위치를 자동 측정할 수 없습니다.** 위치는 각 폰에서 수동 보정합니다.
- 물줄기와 잔의 충돌은 두 화면에서 공유하는 정규화된 가로 좌표로 판정하는 **게임용 연출**입니다. 실제 공간에서 화면 경계를 정확히 통과하는 물리 시뮬레이션은 아닙니다.
- 병의 기울기는 중력 센서로 계산하며, 좌우 어느 쪽으로 기울여도 그쪽으로 따라집니다. 잔 폰은 병을 기울일 쪽에 직접 두세요. 센서가 없는 기기에서는 버튼 모드를 이용하세요.
- 연결을 위해 인터넷이 필요하며, HTTPS GitHub Pages에서 실행해야 합니다.
- 이 버전은 가벼운 프로토타입이며, 인증, 부정행위 방지, 재접속 처리, 정밀한 센서 보정은 포함하지 않습니다.
## 도파# 브랜드 프레임

`dopasharp/` 폴더는 모든 dopa.shap 게임 저장소에 **똑같이** 복사해 쓰는 공통 디자인 시스템(v1.1)입니다. 게임 화면 색은 각 게임이 유지하고, 시작·결과·공유·다른 게임 이동·하단 브랜드 표시만 통일합니다.

- `tokens.css` — 색상·타이포·여백·모서리·모션 토큰. 브랜드 값은 여기서만 바꿉니다.
- `components.css` — 헤더, 버튼(Primary/Secondary/Tertiary), 게임 카드, 결과 시트, 푸터.
- `dopasharp.js` — 게임 목록(`GAMES`, 허브 `games.js`와 같은 순서 = 게임 번호), 결과 시트 `DopaSharp.showResult()`, "다른 게임 보기" → 허브 /games/ 연결, 공통 분석 이벤트 `DopaSharp.track()`.
- 페이지에서는 `<meta name="dopasharp:game" content="저장소이름">`, `<header data-dopa="header">`, `<footer data-dopa="footer">`만 넣으면 됩니다.

링크 미리보기 이미지는 `og/soju.html`을 고친 뒤 `og/soju.png`로 다시 렌더링합니다 (명령은 파일 맨 위 주석 참고).
