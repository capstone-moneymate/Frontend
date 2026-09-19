# 컨벤션

> 이 문서는 팀의 협업 규칙(커밋·브랜치·네이밍·PR)을 한곳에 모은 가이드입니다.
> 스택: **React + TypeScript + Vite + Tailwind CSS (pnpm)**
>
> 규칙마다 검사 방식이 다릅니다.
>
> - **자동 검사**: 커밋 메시지 형식(`commitlint` + `husky`), 파일·폴더·식별자 네이밍(ESLint)
> - **권장 규칙** (자동 검사 없음): 브랜치 이름, Export 규칙, PR 제목·규칙
>
> `pnpm install`을 하면 커밋 검사용 husky hook이 자동으로 설치됩니다.

---

## 0. 워크플로우

```
이슈 생성 → 브랜치 생성 → 작업 & 커밋 → PR 생성 → 리뷰 → develop 머지
```

1. GitHub에서 **이슈를 생성**합니다. (템플릿 선택)
   - `feat` / `fix` / `design` / `refactor`는 각각의 템플릿을 씁니다.
   - 문서·설정·CI 등 그 외 작업은 **Chore / Docs** 템플릿을 쓰고, 제목의 타입(`chore:`)을 작업에 맞게(`docs:`, `ci:` 등) 바꿉니다.
2. `develop`을 최신으로 받은 뒤 **작업 브랜치를 만듭니다.**
   ```bash
   git checkout develop
   git pull
   git checkout -b feat/login-12
   ```
3. 작업하고 **커밋**합니다.
4. `develop`을 대상으로 **PR을 생성**합니다. 본문에 `close #이슈번호`를 적습니다.
5. **팀원 승인 1명 + CI(`build-and-test`) 통과** 후 **Squash and merge** 합니다.
6. 머지된 작업 브랜치는 삭제합니다.

> `main`과 `develop`은 보호되어 있어 직접 push할 수 없습니다. 반드시 PR을 통해 머지합니다.

---

## 1. 커밋 컨벤션

```
type: 메시지
```

예시: `feat: 로그인 기능 구현`

- 타입은 **소문자만** 사용합니다.
- 메시지는 **한글**로, 무엇을 했는지 한 줄로 작성합니다.
- 한 커밋에는 한 가지 의미 단위만 담습니다.
- PR 제목도 같은 형식(`type: 메시지`)으로 씁니다. Squash and merge 시 PR 제목이 커밋 제목이 됩니다.

> 커밋 메시지는 `commitlint` + `.husky/commit-msg`로 **커밋할 때 자동 검사**되며, 규칙에 맞지 않으면 커밋이 거절됩니다. PR 제목은 자동 검사되지 않으니 직접 지켜 주세요.

### 허용 타입 (12종)

| 타입       | 의미                      | 예시                    |
| :--------- | :------------------------ | :---------------------- |
| `feat`     | 새로운 기능 추가          | 컴포넌트 개발, API 연동 |
| `fix`      | 버그 수정                 | 로직 오류, 크래시 수정  |
| `docs`     | 문서 수정                 | README, 명세서          |
| `style`    | 코드 스타일 (의미 변화 X) | 포매팅, 세미콜론        |
| `design`   | UI 디자인 변경            | CSS, 레이아웃 마크업    |
| `test`     | 테스트 코드               | 테스트 추가/수정        |
| `refactor` | 리팩토링 (기능 변화 X)    | 구조 개선               |
| `ci`       | CI 설정 수정              | GitHub Actions workflow |
| `perf`     | 성능 개선                 | 최적화, 렌더링 속도     |
| `chore`    | 자잘한 수정/설정          | 의존성(pnpm), 도구 설정 |
| `rename`   | 파일/폴더명 변경          | 이름·경로 수정          |
| `remove`   | 파일 삭제                 | 미사용 파일 제거        |

---

## 2. 브랜치 컨벤션

```
type/설명-이슈번호
```

예시: `feat/login-12`, `fix/button-click-34`

- 설명은 **영어 소문자 + kebab-case**로 씁니다.
- 타입은 커밋 컨벤션의 12종과 동일합니다.

| 브랜치      | 역할                                          |
| :---------- | :-------------------------------------------- |
| `main`      | 배포되는 안정 버전. `develop`에서만 머지      |
| `develop`   | 기능이 모이는 통합 브랜치. 평소 작업의 기준점 |
| 작업 브랜치 | 기능/수정 단위로 `develop`에서 분기           |

---

## 3. 네이밍 컨벤션

> 파일·폴더·식별자 네이밍은 ESLint(`check-file`, `@typescript-eslint/naming-convention`)로 검사됩니다. `pnpm lint`에서 확인할 수 있고, CI에서 실패하면 머지할 수 없습니다.
> ESLint는 명백한 위반만 거르는 안전망이며, 정밀한 의도는 아래 표를 따릅니다.

### 파일 / 폴더

| 대상               | 규칙         | 예시                                       |
| :----------------- | :----------- | :----------------------------------------- |
| `src` 하위 폴더    | `kebab-case` | `navigation-bar`                           |
| 컴포넌트 (.tsx)    | `PascalCase` | `Button.tsx`                               |
| 일반 파일 (.ts)    | `camelCase`  | `apiClient.ts`                             |
| 커스텀 훅          | `useXxx`     | `useAuth.ts`                               |
| 테스트 파일        | 면제         | `Xxx.test.tsx`                             |
| 진입점 / 설정 파일 | 면제         | `main.tsx`, `router.tsx`, `vite.config.ts` |

### 코드 식별자

| 대상                   | 규칙                                      | 예시                      |
| :--------------------- | :---------------------------------------- | :------------------------ |
| 변수                   | `camelCase` / `UPPER_CASE` / `PascalCase` | `userName`, `MAX_COUNT`   |
| 함수                   | `camelCase` / `PascalCase`                | `fetchUser()`             |
| 타입 / 인터페이스      | `PascalCase`                              | `UserProps`               |
| enum 멤버              | `UPPER_CASE`                              | `Role.ADMIN`              |
| 컴포넌트               | `PascalCase`                              | `LoginForm`               |
| 클래스                 | `PascalCase`                              | `AuthStore`               |
| 클래스 메서드/프로퍼티 | `camelCase`                               | `fetchUser()`, `userName` |
| private 필드           | `camelCase` + `_` 허용                    | `_token`                  |

### Export 규칙

- **`named export`로 통일**합니다. (`default export` 지양)
- Barrel Export 사용을 **권장**합니다. (필수는 아닙니다.)

```tsx
// src/components/ui/ProductList.tsx
export const ProductList = () => {
  return <div>상품 리스트</div>
}
```

```ts
// src/components/ui/index.ts
export { Button } from './Button'
export { Modal } from './Modal'
```

```ts
// 사용처 — 폴더 단위로 import
import { Button, Modal } from '@/components/ui'
```

### Import 경로

- `src` 안의 파일은 상대 경로(`../../`) 대신 **`@/` 별칭**을 씁니다.
  ```ts
  import { apiClient } from '@/api/client'
  ```

---

## 4. 코드 스타일

- 포맷은 **Prettier**, 규칙 검사는 **ESLint**를 따릅니다. (설정 파일: `.prettierrc`, `eslint.config.js`)
- 스타일링은 **Tailwind 클래스**로 하고, 별도 CSS 파일은 만들지 않습니다.
- 환경변수는 `VITE_` 접두사를 붙이고, 추가하면 `.env.example`과 `src/types/env.d.ts`에도 반영합니다.
- PR을 올리기 전에 로컬에서 아래 세 가지를 확인합니다.
  ```bash
  pnpm lint
  pnpm format:check
  pnpm build
  ```

---

## 5. PR 규칙

- PR 템플릿의 항목을 채웁니다. 관련 이슈는 `close #이슈번호`로 연결합니다.
- 대상 브랜치는 `develop`입니다. (`main`으로의 PR은 배포 시에만)
- **승인 1명 + CI 통과**가 있어야 머지할 수 있습니다.
- 리뷰는 **Approve**로 승인해야 인원으로 집계됩니다. (코멘트만 남기면 집계되지 않습니다.)
- 머지 방식은 **Squash and merge**입니다.
