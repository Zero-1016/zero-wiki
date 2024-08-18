# Jest 매처 활용법 (Jest-Matchers)

## RNTL 최소 버전 요구 사항

내장된 Jest 매처를 사용하려면 RNTL v12.4.0 이상이 필요합니다.

이 가이드는 내장된 Jest 매처에 대해 설명하며, 이 매처들을 사용하는 것을 권장합니다. 이러한 매처들은 가독성이 높은 테스트, 접근성 지원, 그리고 더 나은 개발자 경험을 제공합니다.

## 설정

내장된 매처를 사용하려면 `jest-setup.ts` 파일에 다음 줄을 추가하세요 (`setupFilesAfterEnv`를 사용하여 구성):

```typescript
// jest-setup.ts
import '@testing-library/react-native/extend-expect';
```

또는, 위의 스크립트를 Jest 구성에 추가할 수도 있습니다 (일반적으로 `jest.config.js` 파일 또는 `package.json` 파일의 "jest" 키 아래에 위치합니다)

```Javascript
// jest.config.js
{
  "preset": "react-native",
  "setupFilesAfterEnv": ["@testing-library/react-native/extend-expect"]
}
```

## 레거시 Jest Native 매처에서의 마이그레이션

이미 레거시 Jest Native 매처를 사용 중인 경우, 내장된 매처로 이동하기
위한 [마이그레이션 가이드](https://callstack.github.io/react-native-testing-library/docs/migration/jest-matchers)를 참조하세요.

## 요소 존재 확인

### `toBeOnTheScreen()`

```Javascript
expect(element).toBeOnTheScreen();
```

이 매처는 요소가 요소 트리에 연결되어 있는지 여부를 확인할 수 있게 해줍니다. 요소에 대한 참조를 유지하고 있고, 테스트 중에 해당 요소가 언마운트되면 이 어설션을 통과하지 못합니다.

## 요소 콘텐츠

### `toHaveTextContent()`

```typescript
expect(element).toHaveTextContent(
  text: string | RegExp,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
  },
);
```

이 매처는 주어진 요소에 특정 텍스트 콘텐츠가 있는지 여부를 확인할 수 있게 해줍니다. 문자열 또는 정규식 매처, 그리고 `exact` 및 `normalizer`와 같은 텍스트 매칭 옵션을 허용합니다.

### `toContainElement()`

```typescript
expect(container).toContainElement(
  element: ReactTestInstance | null,
);
```

이 매처는 주어진 컨테이너 요소가 다른 호스트 요소를 포함하는지 여부를 확인할 수 있게 해줍니다.

### `toBeEmptyElement()`

```Javascript
expect(element).toBeEmptyElement();
```

## 요소 상태 확인

### `toHaveDisplayValue()`

```typescript
expect(element).toHaveDisplayValue(
  value: string | RegExp,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
  },
);
```

### `toHaveAccessibilityValue()`

```Typescript
expect(element).toHaveAccessibilityValue(
  value: {
    min?: number;
    max?: number;
    now?: number;
    text?: string | RegExp;
  },
);
```

이 매처는 주어진 요소에 지정된 접근성 값이 있는지 여부를 확인할 수 있게 해줍니다. 이
매처는 `aria-valuemin`, `aria-valuemax`, `aria-valuenow`, `aria-valuetext`,
`accessibilityValue` 속성에 기반하여 접근성 값을 확인합니다. 정의된 값 항목만 어설션에 사용되며, 요소는 추가적인 접근성 값 항목을 가질 수 있으며 여전히 일치할 수 있습니다.

텍스트 항목으로 쿼리할 때는 문자열 또는 정규식을 사용할 수 있습니다.

### `toBeEnabled()`/`toBeDisabled()`

```Typescript
expect(element).toBeEnabled();
expect(element).toBeDisabled();
```

이 매처들은 주어진 요소가 사용자 관점에서 활성화되었는지 또는 비활성화되었는지 확인할 수 있게 해줍니다. 이 매처는 `aria-disabled` 또는 `accessibilityState.disabled` 속성에 의해
설정된
접근성 비활성화 상태에 의존합니다. 요소 또는 그 조상 중 하나가 비활성화된 경우 해당 요소는 비활성화된 것으로 간주됩니다.

> 참고:
> 이 매처들은 서로의 부정이며, 이중 부정을 피하기 위해 둘 다 제공됩니다.

### `toBeSelected()`

```Typescript
expect(element).toBeSelected();
```

이 매처는 주어진 요소가 사용자 관점에서 선택되었는지 여부를 확인할 수 있게 해줍니다. 이 매처는 `aria-selected` 또는 `accessibilityState.selected` 속성에 의해 설정된 접근성
선택 상태에 의존합니다.

### `toBeChecked()/toBePartiallyChecked()`

```Typescript
expect(element).toBeChecked();
expect(element).toBePartiallyChecked();
```

이 매처들은 주어진 요소가 사용자 관점에서 체크되었는지 또는 부분적으로 체크되었는지 확인할 수 있게 해줍니다. 이 매처는 `aria-checked` 또는 `accessibilityState.checked` 속성에
의해 설정된 접근성 체크 상태에 의존합니다.

> 참고:
`toBeChecked()` 매처는 체크박스 또는 라디오 역할을 가진 요소에만 적용됩니다.
`toBePartiallyChecked()` 매처는 체크박스 역할을 가진 요소에만 적용됩니다.

이 매처들은 주어진 요소가 사용자 관점에서 확장되었는지 또는 축소되었는지 확인할 수 있게 해줍니다. 이 매처는 `aria-expanded` 또는 `accessibilityState.expanded` 속성에 의해
설정된 접근성 확장 상태에 의존합니다.

> 참고:
> 이 매처들은 확장 가능한 요소들(명시적인 `aria-expanded` 또는 `accessibilityState.expanded` 속성을 가진 요소들)에 대한 부정입니다. 그러나
> 명시적인 `aria-expanded`
> 또는 `accessibilityState.expanded` 속성이 없는 비확장 가능한 요소들에는 적용되지 않습니다.

### `toBeBusy()`

```Typescript
expect(element).toBeBusy();
```

이 매처는 주어진 요소가 사용자 관점에서 바쁜 상태인지 확인할 수 있게 해줍니다. 이 매처는 `aria-busy` 또는 `accessibilityState.busy` 속성에 의해 설정된 접근성 바쁨 상태에
의존합니다.

## 요소 스타일 확인

### `toBeVisible()`

```Typescript
expect(element).toBeVisible();
```

이 매처는 주어진 요소가 사용자 관점에서 보이는지 여부를 확인할 수 있게 해줍니다.

요소 자체 또는 그 조상 중 하나가 `display: none` 또는 `opacity: 0` 스타일을 가지거나, 접근성에서 숨겨져 있는 경우 해당 요소는 보이지 않는 것으로 간주됩니다.

### `toHaveStyle()`

```Typescript
expect(element).toHaveStyle(
  style: StyleProp<Style>,
);
```

이 매처는 주어진 요소가 특정 스타일을 가지고 있는지 여부를 확인할 수 있게 해줍니다.

## 기타 매처

### `toHaveAccessibleName()`

```Typescript
expect(element).toHaveAccessibleName(
  name?: string | RegExp,
  options?: {
    exact?: boolean;
    normalizer?: (text: string) => string;
  },
);
```

이 매처는 주어진 요소가 지정된 접근성 이름을 가지고 있는지 여부를 확인할 수 있게 해줍니다. 문자열 또는 정규식 매처, 그리고 `exact` 및 `normalizer`와 같은 텍스트 매칭 옵션을 허용합니다.

접근성 이름은 `aria-labelledby`, `accessibilityLabelledBy`, `aria-label`, `accessibilityLabel` 속성에 기반하여 계산됩니다. 이러한 속성이 없는 경우
요소의 텍스트 내용이 사용됩니다.

`name` 매개변수가 정의되지 않은 경우 요소가 접근성 이름을 가지고 있는지만 확인합니다.

### `toHaveProp()`

```Typescript
expect(element).toHaveProp(
  name: string,
  value?: unknown,
);
```

이 매처는 주어진 요소가 특정 prop을 가지고 있는지 여부를 확인할 수 있게 해줍니다. `value` 매개변수가 정의되지 않은 경우 해당 prop의 존재 여부만 확인하고, `value`가 정의된 경우 실제 값이
전달된
값과 일치하는지 확인합니다.

> 참고:
> 이 매처는 다른 매처들이 적합하지 않을 때 사용할 수 있는 예외적인 도구로 취급해야 합니다.