# Screen 객체 활용법 (Screen Object)

```typescript
let screen: {
  ...queries;
  rerender(element: React.Element<unknown>): void;
  unmount(): void;
  debug(options?: DebugOptions): void;
  toJSON(): ReactTestRendererJSON | null;
  root: ReactTestInstance;
  UNSAFE_root: ReactTestInstance;
};
```

`screen` 객체는 현재 렌더링된 UI에 대한 쿼리와 유틸리티에 접근하는 권장 방법을 제공합니다.

이 객체는 render 호출 후에 할당되며, 각 테스트 후 `cleanup` 호출을 통해 초기화됩니다. 특정 테스트에서 `render` 호출이 이루어지지 않은 경우, `screen` 객체는 특별한 객체를 보유하며,
각 속성 및 메서드 접근 시 유용한 오류를 발생시킵니다.

## ...queries

`screen`의 가장 중요한 기능은 보기 계층 구조에서 특정 요소를 찾을 수 있는 유용한 쿼리 집합을 제공하는 것입니다.

전체 목록은 [Queries](https://chatgpt.com/c/65f48d77-6c53-43a8-ad3c-ec8a0d50ad44#)에서 확인할 수 있습니다.

```Javascript
import { render, screen } from '@testing-library/react-native';

render(<MyComponent />);
const buttonStart = screen.getByRole('button', { name: 'start' });
```

## rerender

별칭 `update`로도 사용 가능

```Typescript
function rerender(element: React.Element<unknown>): void;
```

새 루트 요소로 메모리 내 트리를 다시 렌더링합니다. 이는 루트에서 React 업데이트 렌더링을 시뮬레이트합니다. 새 요소가 이전 요소와 동일한 타입(및 키)을 가진 경우 트리가 업데이트되고, 그렇지 않은 경우 새
트리가 다시 마운트되며, 두 경우 모두 적절한 생명주기 이벤트가 발생합니다.

## unmount

```Typescript
function unmount(): void;
```

메모리 내 트리를 언마운트하며, 적절한 생명주기 이벤트를 트리거합니다.

> 참고:
> 보통은 unmount를 직접 호출할 필요가 없습니다. 테스트 러너가 afterEach 훅을 지원하는 경우(Jest, Mocha, Jasmine 등), 자동으로 수행됩니다.

## debug

```Typescript
function debug(options?: { message?: string; mapProps?: MapPropsFunction }): void;
```

렌더된 구성 요소를 깊이 프린트합니다.

## message 옵션

화면 상단에 출력할 메시지를 제공할 수 있습니다.

```Javascript
render(<Component />);
screen.debug({ message: 'optional message' });
```

로그 출력 예시:

```PHP
optional message

<View
  onPress={[Function bound fn]}
>
  <Text>Press me</Text>
</View>
```

## mapProps 옵션

```Javascript
function debug({ mapProps: (props) => ({}) });
```

`mapProps` 옵션을 사용하여 출력할 속성을 변환할 수 있습니다.

```Javascript
render(<View style={{ backgroundColor: 'red' }} />);
screen.debug({ mapProps: ({ style, ...props }) => ({ props }) });
```

이 코드는 스타일 속성 없이 렌더된 JSX를 로그에 출력합니다.

`children` 속성은 필터링할 수 없으므로, 모든 렌더된 구성 요소가 `children` 속성을 제외한 모든 속성으로 출력됩니다.

이 옵션은 쿼리를 디버깅할 때 특정 속성을 타겟으로 할 수 있도록 도와줍니다(예: `getByText` 쿼리를 디버깅할 때 `children` 속성만 유지).

속성 값을 더 읽기 쉽게 변환할 수도 있습니다(예: 스타일을 평평하게 만듭니다).

```Javascript
import { StyleSheet } from 'react-native';

screen.debug({ mapProps: ({ style, ...props }) => ({ style: StyleSheet.flatten(style), ...props }) });
```

또는 디버깅할 때 거의 가치가 없는 속성을 제거할 수도 있습니다(예: SVG의 `path` 속성).

```Javascript
screen.debug({ mapProps: ({ path, ...props }) => ({ ...props }) });
```

## toJSON

```Javascript
function toJSON(): ReactTestRendererJSON | null;
```

렌더된 구성 요소의 JSON 표현을 가져옵니다. 예: 스냅샷 테스트에 사용.

## root

```Javascript
const root: ReactTestInstance;
```

렌더된 루트 호스트 요소를 반환합니다.

이 API는 주로 구성 요소 테스트에 유용하며, `*ByTestId` 쿼리나 유사한 방법을 사용하지 않고도 루트 호스트 보기에 접근할 수 있게 해줍니다.

## UNSAFE_root

> 주의:
> 이 API는 일반적으로 컴포지트 뷰를 반환하며, 이는 권장되는 테스트 관행에 어긋납니다. 이 API는 주로 이러한 테스트에 의존하는 레거시 테스트 스위트를 위해 제공됩니다.

```Typescript
const UNSAFE_root: ReactTestInstance;
```

렌더된 컴포지트 루트 요소를 반환합니다.

> 참고:
> 이 API는 이전에 React Testing Library와의 호환성을 위해 container로 명명되었습니다. 하지만 동일한 이름에도 불구하고 실제 동작은 상당히 달랐기 때문에, 이름을 UNSAFE_root로
> 변경했습니다.