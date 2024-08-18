# render 함수 사용법 (render hook)

```typescript
function render(
  component: React.Element<any>,
  options?: RenderOptions
): RenderResult
```

`render` 함수는 React Native Testing Library 테스트를 작성하기 위한 진입점입니다. 이 함수는 주어진 React 요소를 깊이 렌더링하고, 출력된 구성 요소의 구조를 쿼리할 수 있는
헬퍼들을
반환합니다.

```Javascript
import { render } from '@testing-library/react-native';

test('basic test', () => {
  render(<MyApp />);
  expect(screen.getAllByRole('button', { name: 'start' })).toBeOnTheScreen();
});
```

React Context 제공자(예: Redux Provider)를 사용할 때는, 렌더링된 구성 요소를 해당 제공자와 함께 감싸는 것이 일반적입니다. 이런 경우, 사용자 정의 렌더 메서드를 만들어 사용하는 것이
편리합니다. 이를 설정하는 방법에 대한 훌륭한 가이드를 참고하세요.

## 옵션

`render` 메서드의 동작은 `RenderOptions` 타입의 두 번째 인수로 다양한 옵션을 전달하여 사용자 정의할 수 있습니다.

### wrapper 옵션

```Typescript
wrapper?: React.ComponentType<any>,
```

이 옵션을 사용하면 `render()` 함수의 첫 번째 인수로 전달된 테스트 대상 구성 요소를 추가적인 래퍼 구성 요소로 감쌀 수 있습니다. 이는 공통된 React Context 제공자에 대해 재사용 가능한 사용자
정의
렌더 함수들을 만들 때 유용합니다.

### createNodeMock 옵션

```Typescript
createNodeMock?: (element: React.Element) => unknown,
```

이 옵션을 통해 `ReactTestRenderer.create()` 메서드에 `createNodeMock` 옵션을 전달하여 사용자 정의 모크 참조를 허용할 수 있습니다. 이 옵션에 대한 자세한 내용은 React
Test
Renderer 문서에서 확인할 수 있습니다.

### unstable_validateStringsRenderedWithinText 옵션

```Typescript
unstable_validateStringsRenderedWithinText?: boolean;
```

> 참고:
> 이 옵션은 실험적 기능입니다. 일부 경우에는 의도한 대로 작동하지 않을 수 있으며, 이 기능의 동작은 SemVer 규칙을 준수하지 않고 변경될 수 있습니다.

이 실험적 옵션은 `<Text>` 구성 요소가 아닌 다른 구성 요소(예: `<View>`)에 문자열 값을 렌더링하려고 할 때, React Native가 발생시키는 "Text strings must be
rendered
within a `<Text>` component" 오류를 복제할 수 있게 해줍니다.

React Test Renderer는 기본적으로 이 검사를 강제하지 않으므로, React Native Testing Library도 기본적으로 이 검사를 수행하지 않습니다. 이는 테스트에서는 오류 없이 작동하지만,
실제 기기에서 코드를 실행할 때 런타임 오류가 발생할 수 있습니다.

## 결과

`render` 함수는 `screen` 객체와 동일한 쿼리와 유틸리티를 반환합니다. 개발자 친화적인 방법으로 `screen` 객체를 사용하는 것을 권장합니다.

더 자세한 내용은 Kent C. Dodds의 [이 기사](https://kentcdodds.com/)를 참고하세요.