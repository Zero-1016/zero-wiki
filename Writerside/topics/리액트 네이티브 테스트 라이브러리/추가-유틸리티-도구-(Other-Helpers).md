# 추가 유틸리티 도구 (Other Helpers)

## `within` 및 `getQueriesForElement`

```typescript
function within(element: ReactTestInstance): Queries {}

function getQueriesForElement(element: ReactTestInstance): Queries {}
```

`within` 함수(또는 React Testing Library와의 호환성을 위한 `getQueriesForElement` 별칭)는 주어진 요소에 대한 쿼리를 제한하여 수행합니다.

> 참고
>
> `update`, `unmount`, `debug`, `toJSON`과 같은 추가적인 렌더링 관련 작업은 포함되지 않습니다.

### 예시

```Typescript
const detailsScreen = within(screen.getByA11yHint('Details Screen'));
expect(detailsScreen.getByText('Some Text')).toBeOnTheScreen();
expect(detailsScreen.getByDisplayValue('Some Value')).toBeOnTheScreen();
expect(detailsScreen.queryByLabelText('Some Label')).toBeOnTheScreen();
await expect(detailsScreen.findByA11yHint('Some Label')).resolves.toBeOnTheScreen();
```

### 스코프가 지정된 쿼리 사용 사례

- 많은 항목을 포함하는 `FlatList` 내에서 단일 항목에 대한 쿼리
- 화면 전환을 포함하는 테스트에서 단일 화면에 대한 쿼리 (예: `react-navigation` 사용 시)

## `act`

`act` 함수는 훅 API를 사용하는 컴포넌트를 테스트하는 데 유용합니다. 기본적으로 `render`, `update`, `fireEvent`, `waitFor` 호출은 이 함수로 감싸져 있어 수동으로 감쌀 필요가
없습니다. 이 메서드는 react-test-renderer에서 재출력됩니다.

`act` 함수에 대한 더 자세한
내용은 [Undestanding Act function 문서](https://callstack.github.io/react-native-testing-library/docs/advanced/understanding-act)
를 참조하세요.

## `cleanup`

```Typescript
const cleanup: () => void;
```

`cleanup` 함수는 `render`로 마운트된 React 트리를 언마운트하고, 마지막 렌더 출력물을 보유하는 `screen` 변수를 정리합니다.

> 참고
> 
> 사용 중인 테스트 프레임워크가 `afterEach` 글로벌 훅을 지원하는 경우(예: mocha, Jest, Jasmine) 자동으로 수행됩니다. 그렇지 않은 경우 각 테스트 후 수동으로 정리해야 합니다.

### 만약 Jest 테스트 프레임워크를 사용하고 있다면, `afterEach` 훅을 다음과 같이 사용해야 합니다.
```Javascript
import { cleanup, render } from '@testing-library/react-native/pure';
import { View } from 'react-native';

afterEach(cleanup);

it('renders a view', () => {
  render(<View />);
  // ...
});
```

또한, `afterEach(cleanup)` 호출은 `describe` 블록 내에서도 작동합니다.

```Javascript
describe('when logged in', () => {
  afterEach(cleanup);

  it('renders the user', () => {
    render(<SiteHeader />);
    // ...
  });
});
```

`render`를 호출한 후 `cleanup`을 호출하지 않으면 메모리 누수 및 테스트가 "멱등성"을 갖지 않게 되어 테스트에서 디버그하기 어려운 오류가 발생할 수 있습니다.