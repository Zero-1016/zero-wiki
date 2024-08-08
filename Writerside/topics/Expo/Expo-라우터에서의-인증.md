# Expo 라우터에서의 인증

[원본](https://docs.expo.dev/router/reference/authentication/)

Expo Router에서는 모든 라우트가 항상 정의되고 접근할 수 있습니다. 사용자가 인증되었는지 여부에 따라 특정 화면에서 사용자를 리디렉션하기 위해 런타임 로직을 사용할 수 있습니다.

라우트 내에서 사용자를 인증하는 두 가지 기술이 있습니다. 이 가이드는 표준 네이티브 앱의 기능을 보여주는 예제를 제공합니다.

## Using `React Context` and `Route Groups`

특정 라우트를 인증되지 않은 사용자에게 제한하는 것이 일반적입니다. 이는 React Context와 Route Groups를 사용하여 체계적으로 달성할 수 있습니다.
항상 접근 가능한 `/sign-in` 라우트와 인증이 필요한 `(app)` 그룹이 있는 다음 프로젝트 구조를 고려해 보세요.

```Text
🗂️ app
    ⚛️ _layout.tsx
    ⚛️ sign-in.tsx (항상 액세스 가능합니다. 🙌)
    🗂️ (app)
        ⚛️ _layout.tsx (자식 경로를 보호합니다.)
        ⚛️ index.tsx  (액세스를 위해서는 승인이 필요합니다. 🔏)
    
```

위 예제를 따르려면, 전체 앱에 인증 세션을 노출할 수 있는 [React Context provider](https://react.dev/reference/react/createContext)를 설정하세요.

[예제코드](https://docs.expo.dev/router/reference/authentication/#using-react-context-and-route-groups)

```Typescript
import { useContext, createContext, type PropsWithChildren } from 'react';
import { useStorageState } from './useStorageState';

const AuthContext = createContext<{
  signIn: () => void;
  signOut: () => void;
  session?: string | null;
  isLoading: boolean;
}>({
  signIn: () => null,
  signOut: () => null,
  session: null,
  isLoading: false,
});

// 이 훅은 사용자 정보를 액세스하는 데 사용할 수 있습니다.
export function useSession() {
  const value = useContext(AuthContext);
  if (process.env.NODE_ENV !== 'production') {
    if (!value) {
      throw new Error('useSession must be wrapped in a <SessionProvider />');
    }
  }

  return value;
}

export function SessionProvider({ children }: PropsWithChildren) {
  const [[isLoading, session], setSession] = useStorageState('session');

  return (
    <AuthContext.Provider
      value={{
        signIn: () => {
          // 여기서 로그인 로직을 수행하세요
          setSession('xxx');
        },
        signOut: () => {
          setSession(null);
        },
        session,
        isLoading,
      }}>
      {children}
    </AuthContext.Provider>
  );
}
```

다음 코드 스니펫은 네이티브에서 [`expo-secure-store`](https://docs.expo.dev/versions/latest/sdk/securestore/)를 사용하여 토큰을 안전하게 유지하고 웹에서는
로컬 스토리지에 저장하는 기본 훅입니다.

```Typescript
import * as SecureStore from 'expo-secure-store';
import * as React from 'react';
import { Platform } from 'react-native';

type UseStateHook<T> = [[boolean, T | null], (value: T | null) => void];

function useAsyncState<T>(
  initialValue: [boolean, T | null] = [true, null],
): UseStateHook<T> {
  return React.useReducer(
    (state: [boolean, T | null], action: T | null = null): [boolean, T | null] => [false, action],
    initialValue
  ) as UseStateHook<T>;
}

export async function setStorageItemAsync(key: string, value: string | null) {
  if (Platform.OS === 'web') {
    try {
      if (value === null) {
        localStorage.removeItem(key);
      } else {
        localStorage.setItem(key, value);
      }
    } catch (e) {
      console.error('Local storage is unavailable:', e);
    }
  } else {
    if (value == null) {
      await SecureStore.deleteItemAsync(key);
    } else {
      await SecureStore.setItemAsync(key, value);
    }
  }
}

export function useStorageState(key: string): UseStateHook<string> {
  // Public
  const [state, setState] = useAsyncState<string>();

  // Get
  React.useEffect(() => {
    if (Platform.OS === 'web') {
      try {
        if (typeof localStorage !== 'undefined') {
          setState(localStorage.getItem(key));
        }
      } catch (e) {
        console.error('Local storage is unavailable:', e);
      }
    } else {
      SecureStore.getItemAsync(key).then(value => {
        setState(value);
      });
    }
  }, [key]);

  // Set
  const setValue = React.useCallback(
    (value: string | null) => {
      setState(value);
      setStorageItemAsync(key, value);
    },
    [key]
  );

  return [state, setValue];
}
```

인증 컨텍스트를 전체 앱에 제공하려면 루트 레이아웃에서 SessionProvider를 사용하세요. `<Slot />`이 탐색 이벤트가 발생하기 전에 보여져야 합니다. 그렇지 않으면 런타임 오류가 발생합니다.

```Typescript
import { Slot } from 'expo-router';
import { SessionProvider } from '../ctx';

export default function Root() {
  // 인증 컨텍스트를 설정하고 그 안에 레이아웃을 렌더링합니다.
  return (
    <SessionProvider>
      <Slot />
    </SessionProvider>
  );
}
```

사용자가 인증되었는지 확인한 후 자식 라우트 컴포넌트를 렌더링하는 중첩 레이아웃 라우트를 생성하세요. 이 레이아웃 라우트는 인증되지 않은 사용자를 로그인 화면으로 리디렉션합니다.

```Typescript
import { Text } from 'react-native';
import { Redirect, Stack } from 'expo-router';

import { useSession } from '../../ctx';

export default function AppLayout() {
  const { session, isLoading } = useSession();

  // 스플래시 화면을 열어두거나 여기에서 렌더링하는 것처럼 로딩 화면을 렌더링할 수 있습니다.
  if (isLoading) {
    return <Text>Loading...</Text>;
  }

  // (app) 그룹의 레이아웃 내에서만 인증이 필요합니다. 사용자는 (auth) 그룹에 접근하고 다시 로그인할 수 있어야 합니다.
  if (!session) {
    // 웹에서는 페이지가 렌더링되는 헤드리스 노드 프로세스에서 사용자가 인증되지 않았기 때문에 정적 렌더링이 여기서 중지됩니다.
    return <Redirect href="/sign-in" />;
  }

  // 이 레이아웃은 루트 레이아웃이 아니기 때문에 연기할 수 있습니다.
  return <Stack />;
}
```

`/sign-in` 화면을 생성합니다. 이 화면은 `signIn()`을 사용하여 인증을 토글할 수 있습니다. 이 화면은 `(app)` 그룹 외부에 있기 때문에, 그룹의 레이아웃과 인증 검사가 이 화면을 렌더링할 때
실행되지 않습니다. 이를 통해 로그아웃된 사용자가 이 화면을 볼 수 있습니다.

```Typescript
import { router } from 'expo-router';
import { Text, View } from 'react-native';

import { useSession } from '../ctx';

export default function SignIn() {
  const { signIn } = useSession();
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text
        onPress={() => {
          signIn();
          // 로그인 후 이동합니다. 로그인 성공을 보장하기 위해 이를 조정할 수 있습니다.
          router.replace('/');
        }}>
        Sign In
      </Text>
    </View>
  );
}
```

사용자가 로그아웃할 수 있는 인증된 화면을 구현합니다.

```Typescript
import { Text, View } from 'react-native';

import { useSession } from '../../ctx';

export default function Index() {
  const { signOut } = useSession();
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text
        onPress={() => {
          // `app/(app)/_layout.tsx`가 로그인 화면으로 리디렉션합니다.
          signOut();
        }}>
        Sign Out
      </Text>
    </View>
  );
}
```

이제 사용자가 초기 인증 상태를 확인하는 동안 로딩 상태를 표시하고, 사용자가 인증되지 않은 경우 로그인 화면으로 리디렉션하는 앱이 완성되었습니다. 사용자가 인증 검사가 있는 라우트로 깊이 링크를 방문하면 로그인
화면으로 리디렉션됩니다.

## Alternative loading states

Expo Router에서는 초기 인증 상태를 로드하는 동안 화면에 무언가를 렌더링해야 합니다. 위 예제에서는 앱 레이아웃이 로딩 메시지를 렌더링합니다. 또는 `index` 라우트를 로딩 상태로 만들고 초기
라우트를 `/home`과 같이 이동할 수 있습니다.

## Modals and per-route authentication

다른 일반적인 패턴은 앱 상단에 로그인 모달을 렌더링하는 것입니다. 이를 통해 인증이 완료되면 깊이 링크를 부분적으로 유지하고 해제할 수 있습니다. 그러나 이 패턴은 이러한 라우트가 인증 없이 데이터 로드를 처리해야
하기 때문에 백그라운드에서 라우트를 렌더링해야 합니다.

```Text
🗂️ app
    ⚛️ _layout.tsx (최상단에서 세션을 관리합니다.)
    🗂️ (app)
        ⚛️ _layout.tsx
        ⚛️ sign-in.tsx (최상단에 모달을 제공합니다. 🙌)
        🗂️ (root)
            ⚛️ _layout.tsx (자식 경로를 보호합니다.)
            ⚛️ index.tsx  (액세스를 위해서는 승인이 필요합니다. 🔏)
```

```Typescript
import { Stack } from 'expo-router';

export const unstable_settings = {
  initialRouteName: '(root)',
};

export default function AppLayout() {
  return (
    <Stack>
      <Stack.Screen name="(root)" />
      <Stack.Screen
        name="sign-in"
        options={{
          presentation: 'modal',
        }}
      />
    </Stack>
  );
}
```

## Navigating without navigation

앱이 최상단 layout에서 내비게이터가 마운트되지 않은 상태에서 내비게이션을 시도할 때 다음 오류가 발생할 수 있습니다.

> 오류: 루트 레이아웃 구성 요소를 마운트하기 전에 탐색을 시도했습니다. 루트 레이아웃 구성 요소가 첫 번째 렌더링에서 슬롯 또는 다른 탐색기를 렌더링하고 있는지 확인합니다.
> *Error: Attempted to navigate before mounting the Root Layout component. Ensure the Root Layout component is rendering
> a Slot, or other navigator on the first render.

이를 수정하려면 그룹을 추가하고 조건부 로직을 한 단계 아래로 이동하세요.

### Before

```Text
🗂️ app
    ⚛️ _layout.tsx 
    ⚛️ about.tsx
```

```Typescript
export default function RootLayout() {
  React.useEffect(() => {
    // 이 내비게이션 이벤트는 위의 오류를 발생시킵니다.
    router.push('/about');
  }, []);

  // 이 조건문은 루트 레이아웃의 콘텐츠(Slot)가 내비게이션 이벤트가 발생하기 전에 마운트되어야 하므로 문제가 됩니다.
  if (isLoading) {
    return <Text>Loading...</Text>;
  }

  return <Slot />;
}

```

### After

```Text
🗂️ app
    ⚛️ _layout.tsx
    🗂️ (app)
        ⚛️ _layout.tsx
        ⚛️ about.tsx
```

```Typescript
// app/_layout.tsx
export default function RootLayout() {
  return <Slot />;
}
```

```Typescript
// app/(app)/_layout.tsx
export default function RootLayout() {
  React.useEffect(() => {
    router.push('/about');
  }, []);

  // 이 중첩된 레이아웃의 콘텐츠 렌더링을 연기할 수 있습니다. 루트 레이아웃의 콘텐츠가 마운트되기 전에 내비게이션 이벤트
  // (리디렉션)가 발생했을 것이므로 루트 레이아웃의 콘텐츠 렌더링은 연기할 수 없었습니다.
  if (isLoading) {
    return <Text>Loading...</Text>;
  }

  return <Slot />;
}
```

## Middleware

전통적으로 웹사이트는 경로를 보호하기 위해 서버 측 리디렉션을 활용할 수 있습니다. 웹의 Expo Router는 현재 빌드 타임 정적 생성만 지원하며 사용자 정의 미들웨어 또는 서빙을 지원하지 않습니다. 이는 향후
더 최적화된 웹 경험을 제공하기 위해 추가될 수 있습니다. 그동안 클라이언트 측 리디렉션과 로딩 상태를 사용하여 인증을 구현할 수 있습니다.