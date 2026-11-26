# 14th-DukMate-FE

덕질메이트 프론트엔드 (React Native + Expo, TypeScript, Expo Router)

## 실행 방법

1. Node 22를 사용합니다. (`.nvmrc` 참고, nvm 사용 시 `nvm use`)
2. 패키지 설치
   ```bash
   npm install
   ```
3. 환경변수 파일 만들기 — `.env.example`을 복사해 `.env`를 만들고 값을 채웁니다.
   ```bash
   cp .env.example .env
   ```
4. 개발 서버 실행 후 휴대폰의 Expo Go로 QR 코드를 찍습니다.
   ```bash
   npx expo start
   ```

## 스크립트

| 명령 | 설명 |
| --- | --- |
| `npm run lint` | ESLint + Prettier 검사 (CI에서도 실행) |
| `npm run typecheck` | TypeScript 타입 검사 (CI에서도 실행) |
| `npm run format` | Prettier로 전체 파일 정리 |

패키지는 `npm install` 대신 `npx expo install <패키지>`로 추가합니다. 현재 Expo SDK와 맞는 버전으로 설치됩니다.

## 폴더 구조

```
src/
  app/    # 화면과 _layout 파일만 둡니다 (Expo Router: 파일 하나 = 화면 하나)
```

컴포넌트·훅·API 코드 등은 `src/app` 밖(`src/components`, `src/hooks` 등)에 둡니다.

## 브랜치·커밋 규칙

- 기본 브랜치는 `develop`입니다. 작업은 `develop`에서 브랜치를 따서 PR로 합칩니다.
- 브랜치 이름: `feat/기능명`, `fix/버그명`, `chore/작업명`
- 커밋 메시지: `feat: 달력 홈 화면 추가`, `fix: 예산 입력 오류 수정`, `chore: 패키지 업데이트`
- PR은 CI(lint, typecheck)를 통과해야 머지할 수 있습니다.
- 코드 리뷰봇(CodeRabbit)은 PR에 `@coderabbitai review` 댓글을 달면 리뷰합니다.

## 환경변수 주의

`EXPO_PUBLIC_`로 시작하는 값은 앱 안에 그대로 들어가 누구나 꺼내 볼 수 있습니다. LLM API 키 같은 비밀 값은 앱에 넣지 않고 백엔드에서만 사용합니다.
