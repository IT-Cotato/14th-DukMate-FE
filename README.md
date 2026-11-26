# 덕질메이트 - Frontend

> 덕질과 일상을 함께 지키는 덕질비 관리 앱

2026-2 코테이토(Cotato) 14기 프로젝트 **덕질메이트**의 프론트엔드 저장소입니다.

## 프로젝트 소개

덕질은 줄이지 않고, 생활은 지키는 대학생 덕질러의 덕질비 관리 메이트입니다.

- **한 달력에서 보는 돈** — 덕질 지출과 생활비 지출을 한 화면에서 보고, 돈 나갈 날을 미리 확인합니다.
- **나만의 소비 패턴** — 덕질비가 어디에, 얼마나 빨리 쓰이는지 숫자와 그림으로 한눈에 보여 줍니다.
- **후회 없는 굿즈 선택** — 예산 안에서 무엇을 살지, 나만의 선택 기준을 학습한 AI와 함께 고릅니다.

## 기술 스택

| 분류 | 기술 |
| --- | --- |
| 프레임워크 | React Native, Expo |
| 언어 | TypeScript |
| 라우팅 | Expo Router |
| 상태 관리 | TanStack Query (서버 상태), Zustand (전역 상태) |
| OCR | Google ML Kit (기기 내 처리) |
| 코드 품질 | ESLint, Prettier, GitHub Actions CI |

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

> ⚠️ Google ML Kit(OCR)는 네이티브 모듈이라 Expo Go에서 동작하지 않습니다. ML Kit를 추가한 뒤부터는 Expo Go 대신 개발 빌드(development build)로 실행합니다. 방법은 그때 이 문서에 추가합니다.

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
├── app/                # 화면(라우트)과 _layout 파일만 둡니다 (Expo Router: 파일 하나 = 화면 하나)
│   ├── (auth)/         # 로그인
│   ├── (onboarding)/   # 덕후 자기소개서 · 최애 선택 · 예산 설정
│   └── (tabs)/         # 하단 탭: 홈 · 통계 · 위시 · 마이
├── features/           # 기능별 코드 (화면에 들어가는 컴포넌트 · 훅 · API 호출)
│   ├── ledger/         # MVP01 통합 가계부: 달력 홈, 지출 입력(OCR · 문자 · 직접), 일정
│   ├── goods/          # MVP02 굿즈 선택 도우미: 위시 보석함, 덕질 도감, 굿즈 월드컵, AI 상담
│   ├── stats/          # MVP03 소비 통계 · 결산
│   └── account/        # 로그인 · 온보딩 · 마이페이지
├── components/         # 여러 기능에서 함께 쓰는 공통 UI (버튼, 모달, 헤더 등)
├── theme/              # 디자인 토큰: 컬러 · 타이포그래피 · 간격
├── stores/             # Zustand 전역 상태 (예: 생활비/덕질비 토글)
├── api/                # API 클라이언트, TanStack Query 설정
├── types/              # 여러 기능에서 쓰는 공통 타입
└── utils/              # 금액 포맷 등 순수 함수
```

- `src/app`의 화면 파일은 `features/`의 컴포넌트를 가져와 배치만 합니다. 로직은 `features/`에 둡니다.
- `features/<기능>/` 안에는 필요할 때 `components/`, `hooks/`, `api/`를 만듭니다.
- 한 기능에서만 쓰는 코드는 그 기능 폴더에, 두 기능 이상에서 쓰게 되면 공통 폴더(`components/`, `utils/` 등)로 옮깁니다.
- 화면 색상 · 글꼴 크기는 직접 값을 쓰지 않고 `theme/`의 토큰을 씁니다.

## 코드 컨벤션

포맷(따옴표, 세미콜론, 줄바꿈 등)은 Prettier가 정하므로 따로 신경 쓰지 않습니다. 저장 시 자동 정리되도록 에디터의 Prettier 확장을 켜 두는 것을 권장합니다.

| 대상 | 규칙 | 예시 |
| --- | --- | --- |
| 파일·폴더 이름 | kebab-case | `calendar-day.tsx`, `use-budget.ts` |
| 컴포넌트 | PascalCase | `CalendarDay`, `BudgetCard` |
| 훅 | `use`로 시작하는 camelCase | `useBudget` |
| 함수·변수 | camelCase | `formatPrice`, `totalSpent` |
| 상수 | UPPER_SNAKE_CASE | `MAX_BUDGET` |
| 타입·인터페이스 | PascalCase | `Expense`, `GoodsItem` |

## 브랜치 전략

- `main`: 데모·배포용 안정 브랜치
- `develop`: 개발 통합 브랜치 (기본 브랜치)
- 작업 브랜치: `develop`에서 `타입/기능명`으로 따서 PR로 합칩니다. 예) `feat/calendar-home`, `fix/budget-input`

## 커밋 컨벤션

형식: `타입: 제목` (본문은 필요할 때만)

| 타입 | 사용 상황 | 예시 |
| --- | --- | --- |
| feat | 새로운 기능 추가·수정 | `feat: 달력 홈 화면 구현` |
| fix | 버그 수정 | `fix: 예산 입력 시 음수 허용 문제 수정` |
| refactor | 기능 변화 없는 코드 개선 | `refactor: 지출 목록 컴포넌트 분리` |
| design | UI 스타일·레이아웃 수정 | `design: 홈 카드 여백 조정` |
| chore | 설정·패키지·빌드 작업 | `chore: 차트 라이브러리 추가` |
| docs | 문서 수정 | `docs: README 실행 방법 추가` |
| test | 테스트 추가·수정 | `test: 금액 포맷 함수 테스트 추가` |

브랜치 이름의 타입도 위 표와 같은 것을 씁니다.

## PR 및 코드 리뷰 규칙

- PR 제목은 커밋과 같은 `타입: 작업 내용` 형식으로 씁니다.
- PR 본문은 템플릿(`.github/pull_request_template.md`)이 자동으로 채워집니다.
- CI(lint, typecheck)를 통과하고, 프론트엔드 파트원 1명 이상이 승인해야 머지할 수 있습니다.
- 리뷰 코멘트에는 중요도를 붙입니다.
  - **P1**: 반드시 반영 (머지 전 해결)
  - **P2**: 적극적으로 고려·논의
  - **P3**: 가벼운 의견, 사소한 제안
- 코드 리뷰봇(CodeRabbit)은 PR에 `@coderabbitai review` 댓글을 달면 리뷰합니다. 리뷰봇 코멘트는 참고용이며 머지 조건이 아닙니다.

## 환경변수 주의

`EXPO_PUBLIC_`로 시작하는 값은 앱 안에 그대로 들어가 누구나 꺼내 볼 수 있습니다. LLM API 키 같은 비밀 값은 앱에 넣지 않고 백엔드에서만 사용합니다.
