# 비동기 유틸리티 활용 (Async Utilities)

## `findBy*` Queries

`findBy*` 쿼리는 비동기 작업의 결과로 추가될 요소를 찾는 데 사용됩니다. 이러한 요소들은 즉시 사용 가능하지 않지만, 비동기 작업 후에 DOM에 추가됩니다.

## `waitFor`

```typescript
function waitFor<T>(
  expectation: () => T,
  options?: { timeout: number; interval: number }
): Promise<T>;
```

`waitFor`는 `expectation` 콜백이 성공할 때까지 일정 시간 동안 대기합니다. 이 함수는 `timeout`과 `interval` 옵션에 따라 콜백을 여러 번 실행할 수 있으며, 시간이 초과되면 콜백이
던진 마지막
오류를 재던지게 됩니다.

### 잘못된 사용 방법

```Typescript
await waitFor(() => expect(mockFunction).toHaveBeenCalledWith());
```

- `waitFor` 함수는 기본적으로 50ms마다 콜백을 실행하며, 1000ms가 되면 타임아웃됩니다.
- 콜백이 오류를 던지지 않으면 즉시 실행을 멈추고, 콜백의 결과를 반환합니다.
- 오류를 던지지 않는 경우, 예를 들어 `false`를 반환하면 조건이 충족된 것으로 간주됩니다.

### 올바른 사용 방법

```Typescript
await waitFor(() => {
  expect(mockFunction).toHaveBeenCalled();
});
```

### 주의사항

- `waitFor`는 비동기 함수이므로 `await` 키워드를 사용하여 결과를 기다려야 합니다.
- 콜백 내에서 사이드 이펙트를 발생시키지 않도록 주의해야 하며, 하나의 `waitFor` 내에서 여러 개의 어설션을 사용하는 것은 피하는 것이 좋습니다.

## `waitForElementToBeRemoved`

```Typescript
function waitForElementToBeRemoved<T>(
  expectation: () => T,
  options?: { timeout: number; interval: number }
): Promise<T>;
```

`waitForElementToBeRemoved`는 비동기적으로 요소가 DOM에서 제거될 때까지 대기합니다. 이 함수는 일정 간격으로 `expectation`을 호출하여 요소가 제거되었는지 확인합니다.

### 예시

```Typescript
import { render, screen, waitForElementToBeRemoved } from '@testing-library/react-native';

test('waiting for an Banana to be removed', async () => {
  render(<Banana />);

  await waitForElementToBeRemoved(() => screen.getByText('Banana ready'));
});
```

- 이 함수는 요소가 초기에는 렌더 트리에 존재하고, 나중에 제거되는 것을 기대합니다. 요소가 이미 존재하지 않는 경우 오류를 던집니다.
- `getBy`, `getAllBy`, `queryBy`, `queryAllBy`와 같은 쿼리를 `expectation` 매개변수로 사용할 수 있습니다.

> 주의 사항
> - `waitForElementToBeRemoved`를 제대로 사용하려면 React >= 16.9.0 또는 React Native >= 0.61이 필요합니다.

> 참고:
> - `act()` 함수와 관련된 경고가 발생하면, Undestanding Act Function 문서를 참조하세요.