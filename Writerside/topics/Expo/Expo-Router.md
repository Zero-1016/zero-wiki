# Expo Router

- [Expo Router란? 원본](https://docs.expo.dev/router/introduction/)
- [빠른 시작 원본](https://docs.expo.dev/router/installation/#quick-start)
- [페이지 생성하기 원본](https://docs.expo.dev/router/create-pages/)

## Expo Router란?

Expo Router는 React Native 및 웹 애플리케이션을 위한 파일 기반 라우터입니다. 이를 통해 앱 내 화면 간 내비게이션을 관리하고 사용자가 앱의 UI의 다양한 부분을 원활하게 이동할 수 있으며, 여러
플랫폼(Android, iOS, 웹)에서 동일한 컴포넌트를 사용할 수 있습니다.

이 라우터는 웹의 최상의 파일 시스템 라우팅 개념을 애플리케이션으로 가져와 모든 플랫폼에서 라우팅이 작동하도록 합니다. `app` 디렉토리에 파일을 추가하면, 그 파일은 자동으로 내비게이션의 라우트가 됩니다.

## 특징

- **Native(네이티브)**: 강력한 [React Navigation 제품군](https://reactnavigation.org/)을 기반으로 구축된 Expo Router 내비게이션은 기본적으로 진정한 네이티브
  플랫폼 최적화를
  제공합니다.
- **Shareable(공유 가능)**: 앱의 모든 화면이 자동으로 **딥 링크 가능**하여, **앱의 모든 라우트를 링크로 공유**할 수 있습니다.
- **Offline-first(오프라인 우선)**: 앱이 **캐시**되어 **오프라인 우선으로 실행**되며, 새로운 버전을 게시하면 **자동으로 업데이트**됩니다. 네트워크 연결이나 서버 없이 모든 수신을 네이티브
  URL을 처리합니다.
- **Optimized(최적화)**: 프로덕션에서는 **지연 평가(lazy-evaluation)** 로, 개발 중에는 **지연 번들링(deferred bundling)** 으로 라우트가 **자동으로 최적화**
  됩니다.
- **Iteration(반복)**: Android, iOS, 웹 전반에 걸쳐 유니버설 Fast Refresh를 제공하며, 번들러 내에서 **아티팩트 메모이제이션**을 통해 대규모에서도 **빠르게 작업**을 진행할
  수 있습니다.
- **Universal(범용)**: Android, iOS, 웹이 **통합된 내비게이션 구조를 공유**하며, 라우트 수준에서 **플랫폼별 API로 드롭다운할 수 있는 기능을 제공**합니다.
- **Discoverable(발견 가능)**: **Expo Router는 웹에서 빌드 타임 정적 렌더링을 가능**하게 하고, 네이티브로 **유니버설 링크를 제공**합니다. 즉, 앱 콘텐츠가 **검색 엔진**에 의해
  인덱싱될 수 있습니다.

## 빠른 시작

다음 단계에 따라 Expo Router 라이브러리를 사용하여 새 프로젝트를 만들거나 기존 프로젝트에 추가하세요.

1. Expo Router 라이브러리가 이미 설치되고 구성된 프로젝트를 만들려면 `create-expo-app`을 사용하여 새 Expo 앱을 만드는 것을 권장합니다.

```Shell
npx create-expo-app@latest
```

2. 이제 다음 명령어를 실행하여 프로젝트를 시작할 수 있습니다.

```Shell
npx expo start
```

- 모바일 기기에서 앱을 보려면 [Expo Go](https://docs.expo.dev/get-started/set-up-your-environment/#how-would-you-like-to-develop)로
  시작하는 것이 좋습니다. 애플리케이션이
  복잡해지고 더 많은 제어가 필요해지면 [개발 빌드](https://docs.expo.dev/develop/development-builds/introduction/)를 만들 수 있습니다.
- 터미널 UI에서 <kbd>w</kbd>를 눌러 웹 브라우저에서 프로젝트를 엽니다. <kbd>a</kbd>를 눌러 Android에서 실행(안드로이드 스튜디오 필요), <kbd>i</kbd>를 눌러 iOS에서 실행(
  macOS 및 Xcode 필요).

## 수동 설치

이전에 Expo로 생성되었지만 Expo Router 라이브러리가 설치되지 않은 프로젝트가 있는 경우 아래 단계를 따르세요.

### 필수 조건

컴퓨터가 [Expo 앱을 실행할 수 있도록 설정되었는지](https://docs.expo.dev/get-started/create-a-project/) 확인하세요.

#### **1. 종속성 설치**

다음 종속성을 설치해야 합니다:

<tabs>
    <tab id="종속성-설치-50이상" title="SDK 50 이상">
      <code-block lang="shell">
        npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar
      </code-block>
      위 명령어는 프로젝트에서 사용하는 Expo SDK 버전에 호환되는 버전의 라이브러리를 설치합니다.
    </tab>
    <tab id="종속성-설치-49이하" title="SDK 49 이하">
        <code-block lang="shell">
        npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar react-native-gesture-handler        
        </code-block>
        위 명령어는 프로젝트에서 사용하는 Expo SDK 버전에 호환되는 버전의 라이브러리를 설치합니다.
    </tab>
</tabs>

#### **2. 진입점 설정**

**package.json** 파일에서 `main` 속성의 값을 `expo-router/entry`로 설정합니다. 초기 클라이언트 파일은 [**`app/_layout.js
`**](https://docs.expo.dev/router/advanced/root-layout/)입니다.

```json src="package.json"
{
  "main": "expo-router/entry"
}
```

#### **3. 프로젝트 구성 수정**

[앱 구성](https://docs.expo.dev/workflow/configuration/)에 딥 링크 scheme을 추가하세요:

```json src="app.json"
{
  "scheme": "your-app-scheme"
}
```

웹용으로 앱을 개발 중인 경우 다음 종속성을 설치하세요:

```Shell
npx expo install react-native-web react-dom
```

그런 다음, [앱 구성](https://docs.expo.dev/workflow/configuration/)에 다음을
추가하여 [Metro 웹](https://docs.expo.dev/guides/customizing-metro/#adding-web-support-to-metro) 지원을 활성화하세요.

```json src="app.json"
{
  "web": {
    "bundler": "metro"
  }
}
```

#### **4. babel.config.js 수정**

<tabs>
    <tab id="babel-config-수정-50이상" title="SDK 50 이상">
      babel.config.js 파일에서 <code>preset</code> 으로 <code>babel-preset-expo</code> 를 사용하거나 파일을 삭제하세요.
      <code-block lang="javascript">
        module.exports = function (api) {
          api.cache(true);
          return {
            presets: ['babel-preset-expo'],
          };
        };
      </code-block>
      Expo Router v3 이전 버전에서 업그레이드하는 경우 <code>plugins: ['expo-router/babel']</code> 을 제거하세요. <code>expo-router/babel</code> 은 SDK 50 (Expo Router v3)에서 <code>babel-preset-expo</code> 로 통합되었습니다.
    </tab>
    <tab id="babel-config-수정-49이하" title="SDK 49 이하">
        프로젝트의 babel.config.js 파일의 <code>plugins</code> 배열 마지막에 <code>expo-router/babel</code> 플러그인을 추가하세요.
        <code-block lang="javascript">
          module.exports = function (api) {
            api.cache(true);
            return {
              presets: ['babel-preset-expo'],
              plugins: ['expo-router/babel'],
            };
          };
        </code-block>
    </tab>
</tabs>

#### **5.번들러 캐시 지우기**

Babel 설정 파일을 업데이트한 후 다음 명령어를 실행하여 번들러 캐시를 지우세요.

```shell
npx expo start -c
```

#### **6. 결의안 업데이트**

<tabs>
    <tab id="결의안-50이상" title="SDK 50 이상">
      이전 버전의 Expo Router에서 업그레이드하는 경우, <code>package.json</code> 의 모든 오래된 Yarn resolutions 또는 npm overrides를 제거하세요. 특히 <code>metro</code>, <code>metro-resolver</code>, <code>react-refresh</code> resolutions를 <code> package.json</code> 에서 제거하세요.
    </tab>
    <tab id="결의안-49이하" title="SDK 49 이하">
        Expo Router는 최소 <code>react-refresh@0.14.0</code> 이 필요합니다. SDK 49 및 Expo Router v2에서는 React Native가 아직 업그레이드되지 않았으므로 Yarn resolution 또는 npm override를 설정하여 <code>react-refresh</code> 버전을 강제로 업그레이드해야 합니다.
        <tabs>
          <tab id="결의안-Yarn" title="Yarn">
        <code-block lang="json">
          {
            "resolutions": {
              "react-refresh": "~0.14.0"
            }
          }
        </code-block>
          </tab>
          <tab id="결의안-npm" title="npm">
        <code-block lang="json">
          {
            ... 
            "overrides": {
              "react-refresh": "~0.14.0"
            }
          }
        </code-block>
          </tab>
      </tabs>
    </tab>
</tabs>

## 페이지 생성하기

**app** 디렉토리에 파일을 생성하면, 해당 파일은 자동으로 앱의 라우트가 됩니다. 예를 들어, 다음 파일들은 다음 라우트를 생성합니다.

```Text
🗂️ app
    ⚛️ index.tsx ('/' 경로로 접근합니다.)
    ⚛️ home.tsx ('/home' 경로로 접근합니다.)
    ⚛️ [user].tsx ('/expo' 또는 '/evanbacon' 과 같이 동적 경로로 접근합니다.)
    🗂️ settings
        ⚛️ index.tsx  ('/setting' 경로로 접근합니다.)
```

### **페이지**

페이지는 **app** 디렉토리의 파일에서 React 컴포넌트를 기본 값으로 내보내 정의됩니다. 이 파일은 `.js`, `.jsx`, `.tsx`, `.ts` 확장자 중 하나를 사용해야 합니다.

예를 들어, 프로젝트에 **app** 디렉토리를 생성한 다음 그 안에 **index.tsx** 파일을 생성하세요. 그런 다음, 다음 코드를 추가합니다.

#### **모든 플랫폼에서 사용할 페이지의 경우**

React Native의 `<Text>` 컴포넌트를 사용하여 모든 플랫폼에서 텍스트를 렌더링합니다.

```Typescript
import { Text } from 'react-native';

export default function Page() {
  return <Text>Top-level page</Text>;
}
```

#### **웹 플랫폼에서만 사용할 페이지의 경우**

또는, `<div>`, `<p>`와 같은 웹 전용 React 컴포넌트를 작성할 수 있습니다. 하지만, 이러한 컴포넌트는 네이티브 플랫폼에서는 렌더링되지 않습니다.

```Typescript
export default function Page() {
  return <p>Top-level page</p>;
}
```

위 예제는 앱과 브라우저에서 `/` 라우트와 일치합니다. **index**로 명명된 파일은 상위 디렉토리와 일치하며 경로 세그먼트를 추가하지 않습니다. 예를 들어, **app/settings/index.tsx**는
앱에서 `/settings`와 일치합니다.

### 플랫폼별 확장

> **경고** 플랫폼별 확장자는 Expo Router `3.5.x`에 추가되었습니다. 이전 버전의 라이브러리를 사용하는
> 경우 [Platform-specific modules](https://docs.expo.dev/router/advanced/platform-specific-modules/)의 지침을 따르세요.

Metro 번들의 플랫폼별 확장자(예: **.ios.tsx** 또는 **.native.tsx**)는 **비플랫폼 버전**이 존재하는 경우에만 **app** 디렉토리에서 지원됩니다. 이는 딥 링크를 위한 모든
플랫폼에서 라우트가 보편적으로 작동하도록 보장합니다.

다음의 프로젝트 구조를 고려해보세요.

```Text
🗂️ app
    ⚛️ _layout.tsx
    ⚛️ _layout.web.tsx
    ⚛️ index.tsx
    ⚛️ about.tsx
    ⚛️ about.web.tsx
```

위의 파일 구조에서 다음과 같이 사용됩니다.

- _layout.web.tsx 파일은 웹에서는 레이아웃으로 사용되고, _layout.tsx 는 다른 모든 플랫폼에서 사용됩니다.
- index.tsx 파일은 모든 플랫폼의 홈페이지로 사용됩니다.
- about.web.tsx 파일은 웹의 정보 페이지로 사용되고, about.tsx 파일은 다른 모든 플랫폼에서 사용됩니다.

> 플랫폼별 확장자가 없는 라우트 파일을 제공해야 모든 플랫폼에서 기본 구현이 보장됩니다.

## 동적 경로

동적 라우트는 주어진 세그먼트 수준에서 일치하지 않는 모든 경로와 일치합니다.

| Route                      | Matched URL          |
|----------------------------|----------------------|
| **app/blog/[slug].tsx**    | `/blog/123`          |
| **app/blog/[...rest].tsx** | `/blog/123/settings` |

더 높은 특이성을 가진 라우트가 동적 라우트보다 먼저 일치합니다. 예를 들어, `/blog/bacon`은 **blog/bacon.tsx**가 **blog/[id].tsx**보다 먼저 일치합니다.

여러 슬러그는 rest 구문(`...`)을 사용하여 단일 라우트에서 일치시킬 수 있습니다. 예를 들어, **app/blog/[...id].tsx**는 `/blog/123/settings`와 일치합니다.

동적 세그먼트는 페이지 컴포넌트에서 [route parameters](https://docs.expo.dev/router/reference/url-parameters/)로 접근할 수 있습니다.

```Typescript
import { useLocalSearchParams } from 'expo-router';
import { Text } from 'react-native';

export default function Page() {
  const { slug } = useLocalSearchParams();

  return <Text>Blog post: {slug}</Text>;
}
```

## 페이지간 탐색

> 페이지 사이를 이동할 수 있는 링크를 만드는 법을 알아보세요.

Expo Router는 앱 내에서 페이지 간 이동을 위해 `<Link>`를 사용합니다. 이는 웹에서 `<a>` 태그와 `href` 속성을 사용하는 것과 개념적으로 유사합니다.

```Text
🗂️ app
    ⚛️ index.tsx
    ⚛️ about.tsx
    🗂️ user
        ⚛️ [id].tsx
```

다음 예제에서는 두 개의 `<Link />` 컴포넌트가 서로 다른 라우트로 이동합니다.

```Typescript
import { View } from 'react-native';
import { Link } from 'expo-router';

export default function Page() {
  return (
    <View>
      /* 이것을 탭하면 about 페이지로 링크됩니다 */
      <Link href="/about">About</Link>
      /* 이것을 탭하면 동적 라우트 /user/[id] 로 이동되며 /user/bacon 으로 링크됩니다.*/
      <Link href="/user/bacon">View user</Link>
    </View>
  );
}
```

### 버튼

Link 컴포넌트는 기본적으로 자식 요소를 `<Text>` 컴포넌트로 감쌉니다. 이는 접근성에 유용하지만 항상 원하는 것은 아닙니다. `asChild` 속성을 전달하여 컴포넌트를 사용자 정의할 수 있습니다. 이
속성은
Link 컴포넌트의 첫 번째 자식에게 모든 속성을 전달합니다. 자식 컴포넌트는 `onPress`와 `onClick` 속성을 지원해야 하며, `href`와 `role`도 전달됩니다.

```Typescript
import { Pressable, Text } from 'react-native';
import { Link } from 'expo-router';

export default function Page() {
  return (
    <Link href="/other" asChild>
      <Pressable>
        <Text>Home</Text>
      </Pressable>
    </Link>
  );
}
```

### 네이티브 탐색 이해

Expo Router는 스택 기반의 내비게이션 접근 방식을 사용합니다. 이동할 때마다 새로운 라우트가 스택에 추가됩니다. 스택에 이미 있는 라우트로 이동하면 스택이 해당 라우트로 돌아갑니다.

예를 들어, `/feed`에서 `/profile`로 이동하면 스택에는 `/feed`와 `/profile`이 있습니다. 그런 다음 `/settings`로 이동하면 스택에는 `/feed`, `/profile`,
그리고 `/settings`가
있습니다. 그런 다음 `/feed`로 돌아가면 스택이 `/feed`로 돌아갑니다.

스택이 돌아가지 않도록 라우트로 이동하려면 `<Link>` 컴포넌트에 `push` 속성을 사용할 수 있습니다. 이는 라우트가 이미 존재하더라도 항상 스택에 라우트를 추가합니다.

반면, `replace` 메서드는 내비게이션 스택의 현재 라우트를 새로운 라우트로 대체하여, 현재 화면을 스택에 추가하지 않고 새로운 화면으로 대체합니다.

이동하려면 전체 경로(`/profile/settings`), 상대 경로(`../settings`), 또는 객체(`{ pathname: 'profile', params: { id: '123' } }`)를 제공할 수
있습니다.

| 이름         | 설명                                                                                                                                                                  |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `navigate` | 내비게이션 상태에서 가장 가까운 라우트로 이동합니다. `navigate`는 새로운 라우트가 다른 경우(검색 매개변수나 해시 제외)에만 새로운 화면을 추가합니다. 그렇지 않으면 현재 화면이 새로운 매개변수로 다시 렌더링됩니다. 히스토리에 있는 라우트로 이동하면 스택이 해당 라우트로 돌아갑니다. |
| `push`     | 항상 새로운 라우트를 추가하고, 기존 라우트를 팝하거나 대체하지 않습니다. 현재 라우트를 여러 번 푸시하거나 새로운 매개변수로 푸시할 수 있습니다.                                                                                  |
| `replace`  | 히스토리에서 현재 라우트를 제거하고 지정된 URL로 대체합니다. 이는 리디렉션에 유용합니다.                                                                                                                 |

### 동적 경로에 연결

동적 경로와 쿼리 매개변수는 정적으로 제공되거나 편리한 `Href` 객체를 사용하여 제공될 수 있습니다.

```Typescript
import { Link } from 'expo-router';
import { View } from 'react-native';

export default function Page() {
  return (
    <View>
      <Link
        href={{
          pathname: '/user/[id]',
          params: { id: 'bacon' }
        }}>
          View user
        </Link>
    </View>
  );
}
```

### 스크린을 추가

기본적으로 링크는 내비게이션 스택에서 가장 가까운 라우트로 이동하여 새로운 라우트를 추가하거나 기존 라우트로 돌아갑니다. `push` 속성을 사용하여 항상 라우트를 스택에 추가할 수 있습니다.

```Typescript
import { Link } from 'expo-router';

export default function Page() {
  return (
    <View>
      /* /feed</strong>로 이동하고 항상 스택에 추가 */
      <Link push href="/feed">Login</Link>
    </View>
  );
}
```

### 스크린을 교체

기본적으로 링크는 라우트를 내비게이션 스택에 "푸시"합니다. 이는 `navigation.navigate()`와 동일한 규칙을 따릅니다. 즉, 사용자가 뒤로 이동할 때 이전 화면이 제공됩니다. 새로운 화면을 추가하는
대신
현재 화면을 대체하려면 `replace` 속성을 사용할 수 있습니다.

```Typescript
import { Link } from 'expo-router';

export default function Page() {
  return (
    <View>
      <Link replace href="/feed">Login</Link>
    </View>
  );
}
```

`router.replace()`를 사용하여 현재 화면을 명령형으로 대체하세요.

네이티브 내비게이션은 항상 `replace`를 지원하지 않습니다. 예를 들어 X에서는 프로필에서 트윗으로 직접 "대체"할 수 없습니다. 이는 UI가 피드 또는 다른 상위 탭 화면으로 돌아가는 뒤로 가기 버튼을 필요로
하기 때문입니다. 이 경우, `replace`는 피드 탭으로 전환하고 트윗 라우트를 그 위에 푸시하거나, 피드 탭 내의 다른 트윗에 있는 경우 현재 트윗을 새로운 트윗으로 대체합니다. 이 정확한 동작은
[`unstable_settings`](https://chatgpt.com/router/advanced/router-settings)를 사용하여 Expo Router에서 얻을 수 있습니다.

### 객체의 값으로 교체

`router` 객체를 사용하여 명령형으로 내비게이션할 수도 있습니다. 이는 이벤트 핸들러나 유틸리티 함수와 같이 React 컴포넌트 외부에서 내비게이션 작업을 수행해야 할 때 유용합니다.

```Typescript
import { router } from 'expo-router';

export function logout() {
  router.replace('/login');
}
```

**router 객체는 불변이며 다음 함수들을 포함합니다.**

- navigate: (href: Href) => void. navigate 작업을 수행합니다.
- push: (href: Href) => void. push 작업을 수행합니다.
- replace: (href: Href) => void. replace 작업을 수행합니다.
- back: () => void. 이전 라우트로 돌아갑니다.
- canGoBack: () => boolean 유효한 히스토리 스택이 존재하고 back() 함수가 뒤로 이동할 수 있는 경우 true를 반환합니다.
- setParams: (params: Record<string, string>) => void 현재 선택된 라우트의 쿼리 매개변수를 업데이트합니다.

### 자동완성

Expo Router는 앱의 모든 라우트에 대해 정적 TypeScript 타입을 자동으로 생성할 수 있습니다. 이를 통해 `href`의 자동 완성 기능을 사용할 수 있으며 잘못된 링크가 사용될 때 경고를 받을 수
있습니다. [참고 자료](https://chatgpt.com/router/reference/typed-routes)

### 웹 동작

Expo Router는 웹에서 실행할 때 표준 `<a>` 요소를 지원하지만, 이는 전체 페이지 서버 내비게이션을 수행합니다. 이는 느리고 React의 이점을 충분히 활용하지 않습니다. 대신, Expo Router의 `<Link>` 컴포넌트는 클라이언트 측 내비게이션을 수행하여 웹사이트 상태를 유지하고 더 빠르게 이동합니다.

웹 전용 속성인 `target`, `rel`, `download`도 지원됩니다. 이러한 속성은 웹에서 실행할 때 `<a>` 요소에 전달됩니다.

클라이언트 측 내비게이션은 단일 페이지 앱과 [정적 렌더링](https://docs.expo.dev/router/reference/static-rendering/) 모두에서 작동합니다.

### 시뮬레이터에서 사용법

Android 에뮬레이터와 iOS 시뮬레이터에서 딥 링크를 에뮬레이트하는 방법을 알아보려면 [가이드](https://docs.expo.dev/guides/linking/#testing-urls)를 참조하세요.