# renderHook 훅 사용법 (renderHook Function)

```typescript
function renderHook<Result, Props>(
  callback: (props?: Props) => Result,
  options?: RenderHookOptions<Props>
): RenderHookResult<Result, Props>;
```

`renderHook` 함수는 제공된 콜백을 호출하는 테스트 컴포넌트를 렌더링하며, 콜백에서 호출되는 모든 훅을 포함합니다. 이 함수는 `RenderHookResult` 객체를 반환하며, 이를 통해 테스트와
상호작용할 수
있습니다.

### 예시

```Typescript
import { renderHook } from '@testing-library/react-native';
import { useCount } from '../useCount';

it('should increment count', () => {
  const { result } = renderHook(() => useCount());

  expect(result.current.count).toBe(0);
  act(() => {
    // 훅의 상태를 업데이트하는 함수 호출을 `act`로 감싸야 다음 어설션 전에 대기 중인 useEffect가 실행되도록 합니다.
    result.current.increment();
  });
  expect(result.current.count).toBe(1);
});
```

```Typescript
// useCount.js
export const useCount = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((previousCount) => previousCount + 1);

  return { count, increment };
};
```

**`renderHook` 함수의 인수**:

이 함수는 테스트 컴포넌트가 렌더링될 때마다 호출됩니다. 이 함수는 하나 이상의 테스트할 훅을 호출해야 합니다.

콜백에 전달되는 props는 `renderHook`의 `options`에서 제공된 `initialProps`이며, 새 props가 이후의 `rerender` 호출로 제공되지 않는 한 계속해서 사용됩니다.

## `options`

`RenderHookOptions<Props>` 객체를 통해 콜백 함수의 실행을 수정할 수 있으며, 다음과 같은 속성을 포함합니다:

### `initialProps`: 콜백 함수에 props로 전달할 초기 값들입니다. Props 타입은 `renderHook` 호출에서 전달되거나 추론된 타입에 따라 결정됩니다.

### `wrapper`: 테스트 컴포넌트를 렌더링할 때 테스트 컴포넌트를 감쌀 React 컴포넌트입니다. 보통 `useContext`를 사용하여 훅이 접근할 수 있도록 `React.createContext`에서 컨텍스트 제공자를

추가하는 데 사용됩니다.

## `RenderHookResult`

```Typescript
interface RenderHookResult<Result, Props> {
  result: { current: Result };
  rerender: (props: Props) => void;
  unmount: () => void;
}
```

`renderHook` 함수는 다음과 같은 속성을 가진 객체를 반환합니다:

### `result`

현재 값은 renderHook에 전달된 콜백에서 반환된 최신 값을 반영합니다. Result 타입은 renderHook 호출에서 전달되거나 추론된 타입에 따라 결정됩니다.

### `rerender`

테스트 컴포넌트를 다시 렌더링하는 함수로, 모든 훅을 다시 계산하게 만듭니다. newProps가 전달되면 이후의 rerender에서 콜백 함수의 initialProps를 대체합니다. Props 타입은
renderHook 호출에서 전달되거나 추론된 타입에 따라 결정됩니다.

### `unmount`

테스트 컴포넌트를 언마운트하는 함수입니다. 일반적으로 useEffect 훅의 클린업 효과를 트리거하는 데 사용됩니다.

### 예시

### `initialProps` 사용

```Typescript
const useCount = (initialCount: number) => {
  const [count, setCount] = useState(initialCount);
  const increment = () => setCount((previousCount) => previousCount + 1);

  useEffect(() => {
    setCount(initialCount);
  }, [initialCount]);

  return { count, increment };
};

it('should increment count', () => {
  const { result, rerender } = renderHook((initialCount: number) => useCount(initialCount), {
    initialProps: 1,
  });

  expect(result.current.count).toBe(1);

  act(() => {
    result.current.increment();
  });

  expect(result.current.count).toBe(2);
  rerender(5);
  expect(result.current.count).toBe(5);
});
```

### `wrapper` 사용

```Typescript
it('should use context value', () => {
  function Wrapper({ children }: { children: ReactNode }) {
    return <Context.Provider value="provided">{children}</Context.Provider>;
  }

  const { result } = renderHook(() => useHook(), { wrapper: Wrapper });
  // ...
});
```