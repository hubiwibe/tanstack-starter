---
allowed-tools: Read, Bash, Grep, Glob, WebFetch
argument-hint: [file-path] | [pattern] | --full
description: 프론트엔드 전문 코드 리뷰 - React/TanStack 성능, 접근성, UI/UX 가이드라인 준수 검사
---

# Frontend Code Quality Review

프론트엔드 코드 품질 리뷰 수행: $ARGUMENTS

## Current State

- Git status: !`git status --porcelain`
- Recent changes: !`git diff --stat HEAD~5`
- Build status: !`pnpm build --dry-run 2>/dev/null || echo "No build script"`

## Task

다음 단계에 따라 프론트엔드 코드 리뷰를 수행하세요:

---

## 1. 프로젝트 구조 분석

- `src/routes/`, `src/components/`, `src/lib/` 구조 파악
- TanStack Start/Router 사용 패턴 확인
- 의존성 및 번들 구성 검토

---

## 2. 성능 최적화 (Vercel React Best Practices)

### 2.1 워터폴 제거 (CRITICAL)

- [ ] `await`가 실제 필요한 분기에서만 사용되는지 확인
- [ ] 독립적인 async 작업에 `Promise.all()` 적용 여부
- [ ] API 라우트에서 Promise 병렬화 패턴 검토
- [ ] Suspense 경계를 활용한 스트리밍 렌더링

```typescript
// ❌ 잘못된 예시: 순차 실행
const user = await fetchUser()
const config = await fetchConfig()

// ✅ 올바른 예시: 병렬 실행
const [user, config] = await Promise.all([fetchUser(), fetchConfig()])
```

### 2.2 번들 사이즈 최적화 (CRITICAL)

- [ ] Barrel 파일 import 회피 (직접 import 사용)
- [ ] `next/dynamic` 또는 동적 import로 대용량 컴포넌트 lazy-load
- [ ] 분석/로깅 라이브러리는 hydration 후 로드
- [ ] hover/focus 시 preload 패턴 적용

```typescript
// ❌ 전체 라이브러리 import
import { Check, X } from "lucide-react"

// ✅ 직접 import
import Check from "lucide-react/dist/esm/icons/check"
```

### 2.3 서버사이드 성능 (HIGH)

- [ ] `React.cache()` 활용한 요청 내 중복 제거
- [ ] LRU 캐시로 교차 요청 캐싱
- [ ] RSC 경계에서 직렬화 데이터 최소화
- [ ] 컴포넌트 구성으로 병렬 데이터 페칭

### 2.4 클라이언트 데이터 페칭 (MEDIUM-HIGH)

- [ ] TanStack Query/SWR로 자동 중복 요청 제거
- [ ] 전역 이벤트 리스너 중복 방지
- [ ] passive 이벤트 리스너 (scroll, touch)
- [ ] localStorage 데이터 버전 관리

### 2.5 리렌더 최적화 (MEDIUM)

- [ ] 콜백에서만 사용하는 상태는 구독하지 않기
- [ ] `memo()`로 비용 높은 컴포넌트 추출
- [ ] effect 의존성을 primitive로 좁히기
- [ ] 파생 상태(boolean) 구독으로 리렌더 빈도 감소
- [ ] 함수형 setState 사용 (`setItems(curr => [...curr, item])`)
- [ ] lazy state 초기화 (`useState(() => expensiveInit())`)
- [ ] `startTransition`으로 비긴급 업데이트 처리

### 2.6 렌더링 성능 (MEDIUM)

- [ ] SVG 애니메이션은 wrapper div에 적용
- [ ] 긴 리스트에 `content-visibility: auto` 적용
- [ ] 정적 JSX는 컴포넌트 외부로 hoist
- [ ] hydration mismatch 방지 (inline script 패턴)
- [ ] 조건부 렌더링에 삼항 연산자 사용 (`&&` 대신)

### 2.7 JavaScript 성능 (LOW-MEDIUM)

- [ ] DOM CSS 변경 일괄 처리 (class 토글)
- [ ] 반복 조회에 Map 인덱스 구축
- [ ] 루프에서 property 접근 캐싱
- [ ] RegExp 생성을 컴포넌트 외부로 hoist
- [ ] `toSorted()` 사용 (immutability)
- [ ] Set/Map으로 O(1) 조회

---

## 3. Web Interface Guidelines 준수

### 3.1 접근성 (Accessibility)

- [ ] 아이콘 버튼에 `aria-label` 필수
- [ ] 폼 컨트롤에 `<label>` 또는 `aria-label`
- [ ] 인터랙티브 요소에 키보드 핸들러 (`onKeyDown`/`onKeyUp`)
- [ ] 액션은 `<button>`, 네비게이션은 `<a>`/`<Link>` 사용
- [ ] 이미지에 `alt` 속성 (장식용은 `alt=""`)
- [ ] 장식 아이콘에 `aria-hidden="true"`
- [ ] 비동기 업데이트에 `aria-live="polite"`
- [ ] 시맨틱 HTML 우선 사용

### 3.2 포커스 상태 (Focus States)

- [ ] 인터랙티브 요소에 visible focus 스타일
- [ ] `outline-none` 시 대체 포커스 스타일 필수
- [ ] `:focus` 대신 `:focus-visible` 사용
- [ ] 복합 컨트롤에 `:focus-within` 그룹화

### 3.3 폼 (Forms)

- [ ] `autocomplete`와 의미 있는 `name` 속성
- [ ] 올바른 `type` (`email`, `tel`, `url`) 및 `inputmode`
- [ ] paste 차단 금지 (`onPaste` + `preventDefault`)
- [ ] 라벨 클릭 가능 (`htmlFor` 또는 wrapping)
- [ ] 이메일/코드에 `spellCheck={false}`
- [ ] 에러는 필드 옆 인라인 표시, 제출 시 첫 에러로 focus
- [ ] placeholder는 `…`으로 끝내고 예시 패턴 표시
- [ ] 저장되지 않은 변경사항 경고 (`beforeunload`)

### 3.4 애니메이션 (Animation)

- [ ] `prefers-reduced-motion` 존중
- [ ] `transform`/`opacity`만 애니메이션 (compositor 친화적)
- [ ] `transition: all` 금지 - 속성 명시
- [ ] 올바른 `transform-origin` 설정
- [ ] 애니메이션 중단 가능 (사용자 입력 반응)

### 3.5 타이포그래피 (Typography)

- [ ] `...` 대신 `…` 사용
- [ ] 직선 따옴표 대신 곡선 따옴표 `"` `"`
- [ ] 깨지지 않는 공백: `10&nbsp;MB`, `⌘&nbsp;K`
- [ ] 로딩 상태는 `…`으로 끝남: `"Loading…"`
- [ ] 숫자 열에 `font-variant-numeric: tabular-nums`
- [ ] 제목에 `text-wrap: balance` 또는 `text-pretty`

### 3.6 콘텐츠 처리 (Content Handling)

- [ ] 긴 텍스트 처리: `truncate`, `line-clamp-*`, `break-words`
- [ ] Flex 자식에 `min-w-0` (텍스트 truncation 허용)
- [ ] 빈 상태 처리 - 빈 문자열/배열에 깨진 UI 방지
- [ ] 사용자 입력: 짧은/평균/매우 긴 케이스 고려

### 3.7 이미지 (Images)

- [ ] `<img>`에 명시적 `width`와 `height` (CLS 방지)
- [ ] 하단 이미지: `loading="lazy"`
- [ ] 중요 이미지: `priority` 또는 `fetchpriority="high"`

### 3.8 성능 (Performance)

- [ ] 50개 이상 리스트: 가상화 (`virtua`, `content-visibility`)
- [ ] 렌더에서 레이아웃 읽기 금지 (`getBoundingClientRect`, `offsetHeight`)
- [ ] DOM 읽기/쓰기 일괄 처리
- [ ] uncontrolled input 선호; controlled는 키스트로크당 저비용
- [ ] CDN/에셋 도메인에 `<link rel="preconnect">`
- [ ] 중요 폰트에 `<link rel="preload" as="font">`

### 3.9 네비게이션 & 상태 (Navigation & State)

- [ ] URL에 상태 반영 (필터, 탭, 페이지네이션)
- [ ] 링크는 `<a>`/`<Link>` (Cmd+클릭, 중간클릭 지원)
- [ ] stateful UI 딥링크 (nuqs 또는 유사 라이브러리)
- [ ] 파괴적 액션에 확인 모달 또는 undo 창

### 3.10 터치 & 인터랙션 (Touch & Interaction)

- [ ] `touch-action: manipulation` (더블탭 줌 딜레이 방지)
- [ ] 모달/드로어에 `overscroll-behavior: contain`
- [ ] 드래그 중: 텍스트 선택 비활성화
- [ ] `autoFocus`는 데스크톱에서만, 단일 주요 입력에

### 3.11 다크 모드 & 테마 (Dark Mode)

- [ ] 다크 테마에 `color-scheme: dark` on `<html>`
- [ ] `<meta name="theme-color">` 페이지 배경과 일치
- [ ] 네이티브 `<select>`에 명시적 배경색과 글자색

### 3.12 국제화 (i18n)

- [ ] 날짜/시간: `Intl.DateTimeFormat` 사용 (하드코딩 금지)
- [ ] 숫자/통화: `Intl.NumberFormat` 사용
- [ ] 언어 감지: `Accept-Language` / `navigator.languages` (IP 금지)

---

## 4. Anti-Patterns 검출 (반드시 플래그)

다음 패턴이 발견되면 반드시 보고:

- [ ] `user-scalable=no` 또는 `maximum-scale=1` (줌 비활성화)
- [ ] `onPaste` with `preventDefault` (paste 차단)
- [ ] `transition: all` (성능 저하)
- [ ] `outline-none` without focus-visible 대체
- [ ] `<a>` 없이 인라인 `onClick` 네비게이션
- [ ] `<div>` 또는 `<span>`에 클릭 핸들러 (`<button>` 이어야 함)
- [ ] 치수 없는 이미지
- [ ] 가상화 없는 대용량 배열 `.map()`
- [ ] 라벨 없는 폼 입력
- [ ] `aria-label` 없는 아이콘 버튼
- [ ] 하드코딩된 날짜/숫자 포맷 (`Intl.*` 사용해야 함)
- [ ] 명확한 사유 없는 `autoFocus`

---

## 5. TanStack 특화 검토

### TanStack Router

- [ ] `createFileRoute()` 올바른 사용
- [ ] 로더에서 데이터 프리페칭
- [ ] 타입 안전 라우트 파라미터

### TanStack Query

- [ ] 쿼리 키 일관성
- [ ] staleTime/gcTime 적절한 설정
- [ ] useMutation 에러/성공 핸들링
- [ ] 낙관적 업데이트 패턴

### TanStack Form

- [ ] 필드 레벨 vs 폼 레벨 유효성 검사 선택
- [ ] 비동기 유효성 검사 처리
- [ ] 폼 상태 관리

---

## 6. 보안 검토

- [ ] XSS 취약점 (dangerouslySetInnerHTML 사용 시 sanitize)
- [ ] 민감 데이터 클라이언트 노출 방지
- [ ] CSRF 토큰 적용 (서버 액션)
- [ ] 환경 변수 적절한 prefix 사용 (VITE_*, NEXT_PUBLIC_*)

---

## 7. 테스트 커버리지

- [ ] 컴포넌트 테스트 존재 여부
- [ ] 사용자 인터랙션 테스트
- [ ] 접근성 테스트 (axe-core 등)
- [ ] 에러 경계 테스트

---

## Output Format

파일별로 그룹화. `file:line` 형식 (VS Code 클릭 가능). 간결한 findings.

```text
## src/components/Button.tsx

src/components/Button.tsx:42 - 아이콘 버튼 aria-label 누락
src/components/Button.tsx:18 - 입력 라벨 누락
src/components/Button.tsx:55 - prefers-reduced-motion 미적용
src/components/Button.tsx:67 - transition: all → 속성 명시 필요

## src/routes/index.tsx

src/routes/index.tsx:12 - 순차 await → Promise.all 사용 권장
src/routes/index.tsx:34 - barrel import → 직접 import 권장

## src/components/Card.tsx

✓ 통과
```

이슈 + 위치 명시. 수정 방법이 명확하지 않은 경우에만 설명 추가. 서문 없이 바로 시작.

---

## 우선순위 가이드

1. **CRITICAL**: 워터폴 제거, 번들 최적화, 접근성 필수 사항
2. **HIGH**: 서버 성능, 포커스 상태, 보안
3. **MEDIUM**: 클라이언트 페칭, 리렌더 최적화, 폼
4. **LOW**: JS 마이크로 최적화, 타이포그래피

심각도에 따라 이슈를 우선순위화하고 구체적인 개선 제안을 제공하세요.
