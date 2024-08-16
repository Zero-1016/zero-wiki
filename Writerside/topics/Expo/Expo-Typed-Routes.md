# Expo Typed Routes

[원본](https://docs.expo.dev/router/reference/typed-routes/)

Expo Router는 Expo CLI를 통해 TypeScript 타입을 자동으로 생성하는 기능을 지원합니다. 이를 통해 `<Link>`와 hooks API를 정적으로 타입 지정할 수 있습니다. 이 기능은 현재 베타
버전이며 기본적으로 활성화되어 있지 않습니다.

## Get started

### Quick start

[Expo Router 빠른 시작 가이드](https://chatgpt.com/router/installation/#quick-start)를 사용하여 프로젝트를 생성한 경우, 프로젝트가 이미 타입이 지정된 경로를
사용하도록 구성되어 있습니다. 처음 `npx expo start`를 실행하면 Expo CLI가 필요한 타입
파일을 생성합니다. 그런 다음, .tsx 파일에서 Expo Router `<Link>` 컴포넌트를 사용할 때 `href` 속성에 대한 자동 완성을 사용할 수 있습니다.

### Manual configuration

이 기능이 베타 버전인 동안, `app.json`에서 `experiments.typedRoutes`를 `true`로 설정하여 활성화할 수 있습니다.

```json
{
  "expo": {
    "experiments": {
      "typedRoutes": true
    }
  }
}
```

필요한 includes 필드를 추가하기 위해 `npx expo customize tsconfig.json` 명령어를 실행하여 `tsconfig.json` 파일을 설정하세요.

그런 다음, `npx expo start`를 실행하여 개발 서버를 시작하세요. 이제 Expo Router `<Link>` 컴포넌트의 `href` 속성에서 자동 완성을 사용할 수 있습니다.

## Type generation

Expo Router에서 타입이 지정된 경로는 개발 서버가 시작될 때 자동으로 생성됩니다. 기본적으로 생성된 타입은 Git에 의해 추적되지 않으며 로컬 .gitignore 파일에 추가됩니다. 이를 통해 자동 생성된
파일이 버전 관리 시스템을 어지럽히지 않도록 합니다.

개발 서버를 시작하지 않고도 이러한 타입을 생성해야 하는 상황이 발생할 수 있습니다. 예를 들어, Continuous Integration(CI) 서버에서 타입 검사를 수행할 때, `npx expo customize
tsconfig.json` 명령어를 CI에서 실행하여 타입을 생성할 수 있습니다.

## Statically typed routes

이제 `Href<T>`를 사용하는 컴포넌트와 함수는 정적으로 타입이 지정되며, 더욱 엄격한 정의를 가지게 됩니다.

예를 들어.

```Typescript
✅ <Link href="/about" />
✅ <Link href="/user/1" />
✅ <Link href={`/user/${id}`} />
✅ <Link href={("/user" + id) as Href} />
// TypeScript 오류: href가 유효한 경로가 아닌 경우
❌ <Link href="/usser/1" />
```

동적 경로의 경우, `Href`는 객체여야 하며 매개변수는 엄격하게 타입이 지정됩니다.

```Typescript
✅ <Link href={{ pathname: "/user/[id]", params: { id: 1 }}} />
// TypeScript 오류: href는 유효하지만, 매개변수가 있는 HrefObject여야 함
❌ <Link href="/user/[id]" />
// TypeScript 오류: 매개변수에 잘못된 키가 포함된 경우
❌ <Link href={{ pathname: "/user/[id]", params: { _id: 1 }}} />
// TypeScript 오류: 매개변수에 알 수 없는 키가 포함된 경우
❌ <Link href={{ pathname: "/user/[id]", params: { id: 1, id2: 2 }}} />
```

### Relative paths

정적으로 타입이 지정된 경로는 상대 경로를 지원하지 않습니다. 모든 경로에 절대 경로를 사용해야 합니다.

```Typescript
✅ <Link href="/about" />

// 상대 경로는 지원되지 않음
❌ <Link href="./about" />
```

expo-router의 `useSegments()` 훅을 활용하여 복잡한 상대 경로를 생성할 수 있습니다. 다음과 같은 구조를 고려해보세요.

```Text
🗂️ app
    🗂️ (feed)
        ⚛️ _layout.tsx
        ⚛️ feed.tsx
        ⚛️ search.tsx
        ⚛️ profile.tsx
    🗂️ (search)
        ⚛️ profile.tsx
🗂️ components
    ⚛️ button.tsx
```

현재 경로의 첫 번째 세그먼트를 얻기 위해 `useSegments()` 훅을 사용하여 동일한 탭으로 푸시할 수 있습니다.

```Typescript
import { Link, useSegments } from 'expo-router';

export function Button() {
  const [
    // 현재 탭에 따라 `(feed)` 또는 `(search)`가 됩니다.
    first,
  ] = useSegments();

  return <Link href={`/${first}/profile`}>Push profile</Link>;
}
```

## Imperative navigation

타입이 지정된 `router` 객체를 사용하여 명령형으로 탐색할 수 있습니다.

```Typescript
import { router } from 'expo-router';

router.push('/about');
```

또는 타입이 지정된 useRouter() 훅을 사용하세요.

```Typescript
import { useRouter } from 'expo-router';

function Page() {
  const router = useRouter();

  router.push('/about');

  // ...
}
```

## Query parameters

대부분의 쿼리 매개변수는 파일 시스템에 표시되지 않으므로 자동으로 타입이 지정되지 않습니다. `useLocalSearchParams` 및 `useGlobalSearchParams` 훅에 제네릭을 전달하여 쿼리
매개변수를 수동으로 타입 지정할 수 있습니다.

예를 들어.

```Typescript
import { Text } from 'react-native';
import { useLocalSearchParams } from 'expo-router';

export default function Page() {
  const { query } = useLocalSearchParams<{ query?: string }>();

  return <Text>Search: {query ?? 'unset'}</Text>;
}

```

## Changes made to the environment

타입이 지정된 경로가 활성화되면 Expo CLI는 프로젝트의 루트 디렉토리에 Git에서 무시된 `expo-env.d.ts` 파일을 생성하고, `.gitignore`를 업데이트하여 새 루트 `expo-env.d.ts`
파일을
무시하며, `tsconfig.json`을 수정하여 새 `expo-env.d.ts` 파일을 포함하도록 설정합니다.

`tsconfig.json`의 includes 필드가 `expo-env.d.ts` 및 숨겨진 `.expo` 디렉토리를 포함하도록 업데이트됩니다. 이러한 항목은 필수이며 파일에서 제거되지 않아야 합니다.

생성된 `expo-env.d.ts`는 언제든지 삭제하거나 변경해서는 안 됩니다. 이 파일은 커밋되지 않아야 하며, 버전 관리 시스템에서 무시되어야 합니다.

### Global types

타입이 지정된 경로가 활성화되면 Expo CLI는 프로젝트에 다음 전역 타입을 추가합니다.
- `process.env.NODE_ENV = "development" | "production" | "test"`로 설정
- `.[css|sass|scss]` 파일의 임포트를 허용
- `*.module.[css|sass|scss]`의 내보내기를 Record<string, string>으로 설정
- Metro의 `require.context`에 대한 타입 추가. 이는 `expo/metro-config`에 의해 활성화되며, 정적 경로 생성에 사용됩니다.

### React Native Web

타입이 지정된 경로가 활성화되면 Expo CLI는 `react-native` 타입을 보완하여 `React Native Web`을 지원합니다. 다음과 같은 변경 사항이 적용됩니다.

- `ViewStyle`, `TextStyle`, `ImageStyle`에 웹 전용 스타일 추가
- `TextProps`에 `tabIndex`, `aria-level`, `lang` 추가
- Pressable의 `children` 및 `style` 상태 콜백 함수에 `hovered` 추가
- `className` 요소 추가