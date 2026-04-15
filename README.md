# Simple Scoreboard Android App

GitHub에 업로드한 뒤 GitHub Actions로 서명된 APK를 자동 생성할 수 있는 프로젝트입니다.

## 포함 내용
- WebView 기반 간단 스코어보드 앱
- 세로 모드 고정
- 좌우 팀 점수 터치 증가
- 각 팀별 되돌리기 버튼
- 전체 초기화 / 좌우 바꾸기
- GitHub Actions 서명 APK 자동 빌드

## 1) GitHub 업로드
1. 새 GitHub 저장소 생성
2. 이 프로젝트 파일 전체 업로드
3. 기본 브랜치를 `main`으로 사용

## 2) keystore 만들기
로컬 PC 또는 온라인 Linux 환경에서 아래 명령으로 생성

```bash
keytool -genkeypair -v \
  -keystore release-keystore.jks \
  -keyalg RSA \
  -keysize 2048 \
  -validity 10000 \
  -alias scoreboardkey
```

생성 시 아래 3가지를 꼭 기억하세요.
- keystore 비밀번호
- key alias
- key 비밀번호

## 3) keystore base64 만들기

### Windows PowerShell
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes(".\release-keystore.jks")) > keystore.txt
```

### macOS / Linux
```bash
base64 release-keystore.jks > keystore.txt
```

`keystore.txt` 안의 긴 문자열 전체를 복사합니다.

## 4) GitHub Secrets 등록
저장소의 `Settings > Secrets and variables > Actions` 에서 아래 4개를 추가

- `KEYSTORE_BASE64`
- `KEYSTORE_PASSWORD`
- `KEY_ALIAS`
- `KEY_PASSWORD`

## 5) APK 빌드
1. GitHub 저장소에서 `Actions` 탭 이동
2. `Build Signed APK` 워크플로 선택
3. `Run workflow` 클릭
4. 완료 후 `Artifacts`에서 `signed-release-apk` 다운로드

## 참고
- 처음에는 빌드에 몇 분 걸릴 수 있습니다.
- keystore를 잃어버리면 같은 앱 업데이트가 어려울 수 있으니 안전하게 보관하세요.
