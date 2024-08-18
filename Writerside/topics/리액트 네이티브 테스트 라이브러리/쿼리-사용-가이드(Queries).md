# 쿼리 사용 가이드(Queries)

Queries는 React Native Testing Library의 핵심 구성 요소 중 하나로, 테스트 중 애플리케이션의 사용자 인터페이스를 나타내는 요소 트리에서 관련 요소를 찾을 수 있게 해줍니다.

## 쿼리 접근 방법

쿼리는 두 가지 주요 방법으로 접근할 수 있습니다: `screen` 객체를 통해 또는 `render` 함수의 결과로 접근할 수 있습니다.

### screen 객체 사용

```javascript
import { render, screen } from '@testing-library/react-native';

test('accessing queries using "screen" object', () => {
  render(...);

  screen.getByRole("button", { name: "Start" });
});
```

현대적인 방식이자 권장되는 방식은 `@testing-library/react-native`에서 제공하는 `screen` 객체를 사용하는 것입니다. 이 객체는 가장 최근에 렌더링된 UI와 관련된 모든 쿼리 메서드를
포함합니다.

### render 결과 사용

```Javascript
import { render } from '@testing-library/react-native';

test('accessing queries using "render" result', () => {
  const { getByRole } = render(...);
  getByRole("button", { name: "Start" });
});
```

고전적인 방식은 `render` 함수 호출에서 반환된 쿼리 함수를 사용하는 것입니다. `screen` 객체를 사용할 때와 동일한 함수에 접근할 수 있습니다.

## 쿼리의 구성 요소

각 쿼리는 두 가지 요소로 구성됩니다: 변형(variant)과 조건(predicate), 그리고 두 요소는 이름의 가운데에 `by`라는 단어로 구분됩니다.

```Javascript
getByRole()
```

이 쿼리에서 `getBy*`는 쿼리 변형이고, `*ByRole`은 조건입니다.

## 쿼리 변형

쿼리 변형은 일치하는 요소의 개수와 타이밍을 설명하며, 반환 타입이 다릅니다.

| 변형          | Assertion         | 반환 타입                               | 비동기 여부 |
|-------------|-------------------|-------------------------------------|--------|
| getBy*      | 정확히 하나의 일치하는 요소   | `ReactTestInstance`                 | 아니오    |
| getAllBy*   | 하나 이상의 일치하는 요소    | `Array<ReactTestInstance>`          | 아니오    |
| queryBy*    | 0개 또는 1개의 일치하는 요소 | `ReactTestInstance`	                | `null` |
| queryAllBy* | Assertion 없음      | `Array<ReactTestInstance>`          | 아니오    |
| findBy*     | 정확히 하나의 일치하는 요소   | `Promise<ReactTestInstance>`        | 예      |    
| findAllBy*  | 하나 이상의 일치하는 요소    | `Promise<Array<ReactTestInstance>>` | 예      |  

쿼리는 암시적으로 일치하는 요소의 개수를 기반으로 작동하며, 실패 시 오류를 발생시킵니다.

### getBy* 쿼리

```Typescript
getByX(...): ReactTestInstance
```

`getBy*` 쿼리는 쿼리에 일치하는 단일 요소를 반환하며, 일치하는 요소가 없거나 둘 이상의 요소가 일치하면 오류를 발생시킵니다. 여러 개의 요소를 찾아야 한다면 `getAllBy`를 사용하세요.

### getAllBy* 쿼리

```Typescript
getAllByX(...): ReactTestInstance[]
```

`getAllBy*` 쿼리는 쿼리에 일치하는 모든 요소의 배열을 반환하며, 일치하는 요소가 없으면 오류를 발생시킵니다.

### queryBy* 쿼리

```Typescript
queryByX(...): ReactTestInstance | null
```

`queryBy*` 쿼리는 쿼리에 일치하는 첫 번째 노드를 반환하며, 일치하는 요소가 없으면 `null`을 반환합니다. 이는 요소가 존재하지 않는지 확인할 때 유용합니다. 둘 이상의 요소가 일치하면 오류를
발생시키므로,
여러 개의 요소를 찾으려면 `queryAllBy`를 사용하세요.

### queryAllBy* 쿼리

```Typescript
queryAllByX(...): ReactTestInstance[]
```

`queryAllBy*` 쿼리는 쿼리에 일치하는 모든 노드의 배열을 반환하며, 일치하는 요소가 없으면 빈 배열(`[]`)을 반환합니다.

### findBy* 쿼리

```Typescript
findByX(
  ...,
  waitForOptions?: {
    timeout?: number,
    interval?: number,
  },
): Promise<ReactTestInstance>
```

`findBy*` 쿼리는 일치하는 요소를 찾으면 해결되는 프라미스를 반환합니다. 일치하는 요소가 없거나 둘 이상의 요소가 기본 1000ms 타임아웃 내에 일치하면 프라미스가 거부됩니다. 여러 요소를 찾아야 하는 경우
`findAllBy*`를 사용하세요.

### findAllBy* 쿼리

```Typescript
findAllByX(
  ...,
  waitForOptions?: {
    timeout?: number,
    interval?: number,
  },
): Promise<ReactTestInstance[]>
```

`findAllBy*` 쿼리는 일치하는 요소 배열로 해결되는 프라미스를 반환합니다. 일치하는 요소가 없으면 기본 1000ms 타임아웃 내에 프라미스가 거부됩니다.

> 참고:
> `findBy*`와 `findAllBy*` 쿼리는 선택적으로 `waitForOptions` 객체 인수를 받아 `timeout`, `interval` 및 `onTimeout` 속성을 설정할 수 있습니다. 이
> 값들은 `waitFor` 함수의
> 옵션과 동일한 의미를 가집니다.

> 참고
`findBy*` 및 `findAllBy*` 쿼리가 일치하는 요소를 찾지 못해 예외를 발생시킬 때, `waitForOptions` 매개변수를
> 사용하여 `onTimeout: () => { screen.debug(); }` 콜백을 전달하는 것이 유용합니다.

## 쿼리 조건 (Query predicates)

참고: 대부분의 메서드는 다음과 같은 속성을 가진 `ReactTestInstance`를 반환합니다.

```typescript
type ReactTestInstance = {
  type: string | Function;
  props: { [propName: string]: any };
  parent: ReactTestInstance | null;
  children: Array<ReactTestInstance | string>;
};
```

## *ByRole

> getByRole, getAllByRole, queryByRole, queryAllByRole, findByRole, findAllByRole

```Typescript
getByRole(
  role: TextMatch,
  options?: {
    name?: TextMatch;
    disabled?: boolean;
    selected?: boolean;
    checked?: boolean | 'mixed';
    busy?: boolean;
    expanded?: boolean;
    value: {
      min?: number;
      max?: number;
      now?: number;
      text?: TextMatch;
    },
    includeHiddenElements?: boolean;
  }
): ReactTestInstance;
```

해당 역할(role) 또는 `accessibilityRole` 속성과 일치하는 `ReactTestInstance`를 반환합니다.

> 참고:
*ByRole 쿼리가 요소를 일치시키려면 해당 요소가 접근성 요소로 간주되어야 합니다:

기본적으로 Text, TextInput, Switch 호스트 요소는 접근성 요소입니다.
View 호스트 요소는 `accessible` 속성이 `true`로 설정되어야 합니다.
Pressable 및 TouchableOpacity와 같은 일부 React Native 컴포지트 구성 요소는 `accessible` 속성이 이미 설정된 호스트 View 요소를 렌더링합니다.

```Javascript
import { render, screen } from '@testing-library/react-native';

render(
  <Pressable accessibilityRole="button" disabled>
    <Text>Hello</Text>
  </Pressable>
);
const element = screen.getByRole('button');
const element2 = screen.getByRole('button', { name: 'Hello' });
const element3 = screen.getByRole('button', { name: 'Hello', disabled: true });
```

### 옵션

`name`: 주어진 `role`/`accessibilityRole`과 접근성 이름(= 접근성 레이블 또는 텍스트 내용)과 일치하는 요소를 찾습니다.

`disabled`:요소의 비활성화 상태(`aria-disabled` 속성 또는 `accessibilityState.disabled` 속성에서 가져옴)를 기준으로 필터링할 수 있습니다. 가능한 값은 `true`
또는 `false`
입니다. `disabled: false`를 쿼리하면 `disabled: undefined` 상태의 요소도 일치시킵니다. 자세한
내용은 [위키](https://github.com/callstack/react-native-testing-library/wiki/Accessibility:-State)를 참조하세요.

- 이 옵션은 `toBeEnabled()` / `toBeDisabled()` Jest 매처를 사용하여 표현할 수도 있습니다.

`selected`: 선택된 상태(`aria-selected` 속성 또는 `accessibilityState.selected` 속성에서 가져옴)를 기준으로 요소를 필터링할 수 있습니다. 가능한 값은 `true`
또는 `false`
입니다. `selected: false`를 쿼리하면 `selected: undefined` 상태의 요소도 일치시킵니다. 자세한
내용은 [위키](https://github.com/callstack/react-native-testing-library/wiki/Accessibility:-State)를 참조하세요.

- 이 옵션은 `toBeSelected()` Jest 매처를 사용하여 표현할 수도 있습니다.

`checked`:체크된 상태(`aria-checked` 속성 또는 `accessibilityState.checked` 속성에서 가져옴)를 기준으로 요소를 필터링할 수 있습니다. 가능한
값은 `true`, `false`,
또는 `"mixed"`입니다.

- 이 옵션은 `toBeChecked()` / `toBePartiallyChecked()` Jest 매처를 사용하여 표현할 수도 있습니다.

`busy`: 바쁜 상태(`aria-busy` 속성 또는 `accessibilityState.busy` 속성에서 가져옴)를 기준으로 요소를 필터링할 수 있습니다. 가능한 값은 `true` 또는 `false`
입니다. `busy: false`를 쿼리하면 `busy: undefined` 상태의 요소도 일치시킵니다. 자세한
내용은 [위키](https://github.com/callstack/react-native-testing-library/wiki/Accessibility:-State)를 참조하세요.

- 이 옵션은 `toBeBusy()` Jest 매처를 사용하여 표현할 수도 있습니다.

`expanded`: 확장된 상태(`aria-expanded` 속성 또는 `accessibilityState.expanded` 속성에서 가져옴)를 기준으로 요소를 필터링할 수 있습니다. 가능한 값은 `true`
또는 `false`입니다.

- 이 옵션은 `toBeExpanded()` / `toBeCollapsed()` Jest 매처를 사용하여 표현할 수도 있습니다.

`value`: `aria-valuemin`, `aria-valuemax`, `aria-valuenow`, `aria-valuetext` 또는 `accessibilityValue` 속성을 기반으로 접근성 값을
사용하여
요소를 필터링합니다. 접근성 값은 개념적으로 숫자 `min`, `max`, `now` 항목과 문자열 `text` 항목으로 구성됩니다.

- 이 옵션은 `toHaveAccessibilityValue()` Jest 매처를 사용하여 표현할 수도 있습니다.

## *ByLabelText

> getByLabelText, getAllByLabelText, queryByLabelText, queryAllByLabelText, findByLabelText, findAllByLabelText

```Typescript
getByLabelText(
  text: TextMatch,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
    includeHiddenElements?: boolean;
  },
): ReactTestInstance;
```

일치하는 레이블을 가진 `ReactTestInstance를` 반환합니다:

- `aria-label`/`accessibilityLabel` 속성과 일치하거나
- `aria-labelledby`/`accessibilityLabelledBy` 속성에 의해 참조된 뷰의 텍스트 내용과 일치합니다.

### 예시

```Typescript
import { render, screen } from '@testing-library/react-native';

render(<MyComponent />);
const element = screen.getByLabelText('my-label');
```

## *ByPlaceholderText

> getByPlaceholderText, getAllByPlaceholderText, queryByPlaceholderText, queryAllByPlaceholderText,
> findByPlaceholderText, findAllByPlaceholderText

```Typescript
getByPlaceholderText(
  text: TextMatch,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
    includeHiddenElements?: boolean;
  }
): ReactTestInstance;
```

일치하는 플레이스홀더를 가진 `TextInput`의 `ReactTestInstance`를 반환합니다. 플레이스홀더는 문자열 또는 정규식일 수 있습니다.

### 예시

```Typescript
import { render, screen } from '@testing-library/react-native';

render(<MyComponent />);
const element = screen.getByPlaceholderText('username');
```

## *ByDisplayValue

> getByDisplayValue, getAllByDisplayValue, queryByDisplayValue, queryAllByDisplayValue, findByDisplayValue,
> findAllByDisplayValue

```Typescript
getByDisplayValue(
  value: TextMatch,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
    includeHiddenElements?: boolean;
  },
): ReactTestInstance;
```

일치하는 표시 값을 가진 `TextInput`의 `ReactTestInstance`를 반환합니다. 표시 값은 문자열 또는 정규식일 수 있습니다.

### 예시

```Typescript
import { render, screen } from '@testing-library/react-native';

render(<MyComponent />);
const element = screen.getByDisplayValue('username');
```

## *ByText

> getByText, getAllByText, queryByText, queryAllByText, findByText, findAllByText

```Typescript
getByText(
  text: TextMatch,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
    includeHiddenElements?: boolean;
  }
): ReactTestInstance;
```

일치하는 텍스트를 가진 `ReactTestInstance`를 반환합니다. 텍스트는 문자열 또는 정규식일 수 있습니다.

이 메서드는 `<Text>` 형제 요소를 결합하여 일치하는 항목을 찾습니다. 이는 React Native가 이러한 구성 요소를 처리하는 방식과 유사합니다. 이를 통해 시각적으로 함께 렌더링되지만 의미상으로는 별개의
React 구성 요소인 문자열을 쿼리할 수 있습니다.

### 예시

```Typescript
import { render, screen } from '@testing-library/react-native';

render(<MyComponent />);
const element = screen.getByText('banana');
```

## *ByHintText

> getByA11yHint, getAllByA11yHint, queryByA11yHint, queryAllByA11yHint, findByA11yHint, findAllByA11yHint
> getByAccessibilityHint, getAllByAccessibilityHint, queryByAccessibilityHint, queryAllByAccessibilityHint,
> findByAccessibilityHint, findAllByAccessibilityHint
> getByHintText, getAllByHintText, queryByHintText, queryAllByHintText, findByHintText, findAllByHintText

```Typescript
getByHintText(
  hint: TextMatch,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
    includeHiddenElements?: boolean;
  },
): ReactTestInstance;
```

일치하는 `accessibilityHint` 속성을 가진 `ReactTestInstance`를 반환합니다.

### 예시

```Typescript
import { render, screen } from '@testing-library/react-native';

render(<MyComponent />);
const element = screen.getByHintText('Plays a song');
```

## *ByTestId

> getByTestId, getAllByTestId, queryByTestId, queryAllByTestId, findByTestId, findAllByTestId

```Typescript
getByTestId(
  testId: TextMatch,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
    includeHiddenElements?: boolean;
  },
): ReactTestInstance;
```

일치하는 `testID` 속성을 가진 `ReactTestInstance`를 반환합니다. `testID`는 문자열 또는 정규식일 수 있습니다.

### 예시

```Typescript
import { render, screen } from '@testing-library/react-native';

render(<MyComponent />);
const element = screen.getByTestId('unique-id');
```

> 참고: 지침 원칙에 따라, 다른 쿼리가 사용 사례에 적합하지 않을 때만 이 쿼리를 사용하는 것이 좋습니다. testID 속성 사용은 소프트웨어가 실제로 사용되는 방식과 유사하지 않으므로 가능하면 피해야 합니다.
> 하지만 실제 기기에서 엔드 투 엔드 테스트를 수행할 때는 매우 유용하며, 이 방법을 사용하는 것이 권장됩니다. 자세한 내용은 블로그
> 글 ["Making your UI tests resilient to change"](https://kentcdodds.com/blog/making-your-ui-tests-resilient-to-change)에서
> 확인할 수 있습니다.

## TextMatch 타입

```Typescript
type TextMatch = string | RegExp;
```

대부분의 쿼리 API는 `TextMatch`를 인수로 사용하며, 이는 인수가 문자열 또는 정규식일 수 있음을 의미합니다.

### 예시

다음 렌더링이 주어졌을 경우

```Typescript
render(<Text>Hello World</Text>);
```

다음 일치 항목을 찾을 수 있습니다.

```Typescript
// 문자열 일치:
screen.getByText('Hello World'); // 전체 문자열 일치
screen.getByText('llo Worl', { exact: false }); // 부분 문자열 일치
screen.getByText('hello world', { exact: false }); // 대소문자 구분 없음

// 정규식 일치:
screen.getByText(/World/); // 부분 문자열 일치
screen.getByText(/world/i); // 부분 문자열 일치, 대소문자 구분 없음
screen.getByText(/^hello world$/i); // 전체 문자열 일치, 대소문자 구분 없음
screen.getByText(/Hello W?oRlD/i); // 고급 정규식
```

다음 일치 항목은 찾지 못합니다.

```Typescript
// 부분 문자열이 일치하지 않음
screen.getByText('llo Worl');
// 전체 문자열이 일치하지 않음
screen.getByText('Goodbye World');

// 대소문자 구분 정규식이 다른 대소문자와 일치하지 않음
screen.getByText(/hello world/);
```

### 옵션

**Precision**

```Typescript
type TextMatchOptions = {
  exact?: boolean;
  normalizer?: (text: string) => string;
};
```

`TextMatch`를 사용하는 쿼리는 문자열 매칭의 정밀도에 영향을 미치는 옵션을 포함할 수 있는 객체를 두 번째 인수로 허용합니다.

- exact: 기본값은 `true`입니다. 전체 문자열과 대소문자를 구분하여 일치시킵니다. `false`일 경우 부분 문자열과 대소문자를 구분하지 않고 일치시킵니다.
    - 정규식 인수에는 `exact`가 영향을 미치지 않습니다.
    - 대부분의 경우 문자열 대신 정규식을 사용하는 것이 모호한 매칭에 대한 더 많은 제어를 제공하며 `{ exact: false }` 대신 선호됩니다.
- **normalizer**: 기본 정규화 동작을 재정의하는 선택적 함수입니다. 자세한
  내용은 [Normalization](https://callstack.github.io/react-native-testing-library/docs/api/queries#normalization)을 참조하세요.

### 정규화 예시

공백을 자르지 않고 텍스트와 일치시키려면 다음과 같습니다.

```Typescript
screen.getByText(node, 'text', {
  normalizer: getDefaultNormalizer({ trim: false }),
});
```

일부(모든 내장 정규화가 아닌) 유니코드 문자를 제거하도록 정규화를 재정의하려면 다음과 같습니다.

```Typescript
screen.getByText(node, 'text', {
  normalizer: (str) => getDefaultNormalizer({ trim: false })(str).replace(/[\u200E-\u200F]*/g, ''),
});
```