# 사용자 이벤트 시뮬레이션 (User Event Interactions)

## RNTL 최소 버전 요구 사항

User Event 상호작용을 사용하려면 RNTL v12.2.0 이상이 필요합니다.

## Fire Event API와의 비교

Fire Event는 기존의 이벤트 시뮬레이션 API로, 호스트 또는 컴포지트 요소에 선언된 모든 이벤트 핸들러를 호출할 수 있습니다. 전달된 이벤트 이름에 대한 `onEventName` 핸들러가 요소에 없거나
요소가 비활성화된 경우, Fire Event는 요소 트리를 따라 올라가며 호스트 및 컴포지트 요소에서 이벤트 핸들러를 찾습니다. 기본적으로 이벤트 데이터를 전달하지 않지만, 마지막 인수로 사용자 지정 데이터를 제공할
수 있습니다.

반면, User Event는 `press` 또는 `type`과 같은 사용자 상호작용에 대한 현실적인 이벤트 시뮬레이션을 제공합니다. 각 상호작용은 React Native 런타임 동작에 해당하는 일련의 이벤트를
트리거합니다. 이 이벤트들은 호스트 요소에서만 호출되며, 각 이벤트에 해당하는 이벤트 데이터를 자동으로 받습니다.

User Event가 특정 상호작용을 지원하는 경우, Fire Event를 사용하는 것보다 User Event를 사용하는 것이 훨씬 더 현실적이고 신뢰할 수 있는 테스트를 작성할 수 있으므로 항상 권장됩니다. 그러나
User Event가 지원하지 않는 이벤트나 컴포지트 요소에서 이벤트 핸들러를 호출해야 할 때는 Fire Event를 사용해야 합니다.

## `setup()`

```typescript
userEvent.setup(options?: {
  delay: number;
  advanceTimers: (delay: number) => Promise<void> | void;
});
```

### 예시

```Javascript
const user = userEvent.setup();
```

User Event 객체 인스턴스를 생성하며, 이를 사용하여 이벤트를 트리거할 수 있습니다.

### 옵션

- `delay`: 후속 이벤트 간의 기본 지연 시간(예: 키 입력 간)을 제어합니다.
- `advanceTimers`: 가짜 타이머를 사용할 때 사용해야 하는 시간 진행 유틸리티 함수입니다. 기본 설정은 실제 타이머와 Jest 가짜 타이머를 모두 처리합니다.

## `press()`

```Typescript
press(
  element: ReactTestInstance,
): Promise<void>;
```

### 예시

```Javascript
const user = userEvent.setup();
await user.press(element);
```

이 헬퍼는 `Pressable`, `TouchableOpacity`, `Text`, `TextInput` 등 누를 수 있는 요소에 대한 `press` 상호작용을 시뮬레이션합니다. `fireEvent.press()`와
달리, 이 함수는
React Native 런타임에서 발생하는 전체 `press` 상호작용을 더 현실적으로 재현하여 `pressIn` 및 `pressOut`과 같은 추가 이벤트도 트리거합니다.

## `longPress()`

```Javascript
longPress(
  element: ReactTestInstance,
  options: { duration: number } = { duration: 500 }
): Promise<void>;
```

### 예시

```Javascript
const user = userEvent.setup();
await user.longPress(element);
```

`longPress` 사용자 상호작용을 시뮬레이션합니다. React Native에서는 `press` 지속 시간이 `longPress` 임계값(기본값 500ms)을 초과할 때 `longPress` 이벤트가 발생합니다.
이 작업은
일반 `press` 동작과 유사하게 `pressIn` 및 `pressOut` 이벤트를 트리거합니다. `press` 지속 시간은 옵션을 통해 사용자 정의할 수 있으며, 이는 `delayLongPress` prop을
사용할 때
유용합니다. 실제 타이머를 사용하는 경우 500ms가 걸리므로 테스트 실행 시간이 오래 걸리지 않도록 가짜 타이머와 함께 이 API를 사용하는 것이 좋습니다.

### 옵션

`duration`: `press`의 지속 시간(밀리초). 기본값은 500ms입니다.

## `type()`

```Typescript
type(
  element: ReactTestInstance,
  text: string,
  options?: {
    skipPress?: boolean;
    submitEditing?: boolean;
  },
): Promise<void>;
```

### 예시

```Javascript
const user = userEvent.setup();
await user.type(textInput, 'Hello world!');
```

이 헬퍼는 사용자가 `TextInput` 요소에 포커스를 맞추고, 텍스트를 하나씩 입력하고, 요소를 떠나는 동작을 시뮬레이션합니다.

이 함수는 호스트 `TextInput` 요소만 지원합니다. 다른 요소 타입을 전달하면 오류가 발생합니다.

> 참고:
> 이 함수는 `TextInput`에 이미 존재하는 텍스트(예: `value` 또는 `defaultValue` 속성으로 지정된 텍스트)에 텍스트를 추가합니다. 기존 텍스트를 교체하려면 먼저 `clear()` 헬퍼를
> 사용하세요.

### 옵션

`skipPress`: `true`일 경우, `pressIn` 및 `pressOut` 이벤트가 트리거되지 않습니다.
`submitEditing`: `true`일 경우, 텍스트 입력 후 `submitEditing` 이벤트가 트리거됩니다.

### 이벤트 순서

이벤트 순서는 `multiline` prop 및 전달된 옵션에 따라 다릅니다.

- 요소에 들어갈 때:
    - `pressIn` (선택 사항)
    - `focus`
    - `pressOut` (선택 사항)
    - `pressIn` 및 `pressOut` 이벤트는 기본적으로 전송되지만 `skipPress`: `true` 옵션을 전달하여 생략할 수 있습니다.
- 입력 중 (각 문자에 대해):
    - `keyPress`
    - `change`
    - `changeText`
    - `selectionChange`
    - `contentSizeChange` (멀티라인일 경우)
- 요소에서 나올 때:
    - `submitEditing` (선택 사항)
    - `endEditing`
    - `blur`

`submitEditing` 이벤트는 기본적으로 생략되며, `submitEditing: true` 옵션을 설정하여 전송할 수 있습니다.

## `clear()`

```Typescript
clear(
  element: ReactTestInstance,
): Promise<void>;
```

### 예시

```Javascript
const user = userEvent.setup();
await user.clear(textInput);
```

이 헬퍼는 사용자가 `TextInput` 요소의 내용을 지우는 동작을 시뮬레이션합니다.

이 함수는 호스트 `TextInput` 요소만 지원합니다. 다른 요소 타입을 전달하면 오류가 발생합니다.

### 이벤트 순서

`editable` prop이 `false`로 설정된 경우 이벤트는 전송되지 않습니다.

요소에 들어갈 때:

- `focus`

모든 내용 선택:

- `selectionChange`

백스페이스 누르기:

- `keyPress`
- `change`
- `changeText`
- `selectionChange`

요소에서 나올 때:

- `endEditing`
- `blur`

## `paste()`

```Typescript
paste(
  element: ReactTestInstance,
  text: string,
): Promise<void>;
```

### 예시

```Javascript
const user = userEvent.setup();
await user.paste(textInput, 'Text to paste');
```

이 헬퍼는 사용자가 지정된 텍스트를 `TextInput` 요소에 붙여넣는 동작을 시뮬레이션합니다.

이 함수는 호스트 `TextInput` 요소만 지원합니다. 다른 요소 타입을 전달하면 오류가 발생합니다.

### 이벤트 순서

`editable` prop이 `false`로 설정된 경우 이벤트는 전송되지 않습니다.

요소에 들어갈 때:

- `focus`

모든 내용 선택:

- `selectionChange`

백스페이스 누르기:

- `keyPress`
- `change`
- `changeText`
- `selectionChange`

요소에서 나올 때:

- `endEditing`
- `blur`

## `scrollTo()`

> 참고: scrollTo 상호작용은 RNTL v12.4.0에 도입되었습니다.

```Typescript
scrollTo(
  element: ReactTestInstance,
  options: {
    y: number,
    momentumY?: number,
    contentSize?: { width: number; height: number };
    layoutMeasurement?: { width: number; height: number };
  } | {
    x: number,
    momentumX?: number,
    contentSize?: { width: number; height: number };
    layoutMeasurement?: { width: number; height: number };
  },
): Promise<void>;
```

### 예시

```Javascript
const user = userEvent.setup();
await user.scrollTo(scrollView, { y: 100, momentumY: 200 });
```

이 헬퍼는 사용자가 호스트 `ScrollView` 요소를 스크롤하는 동작을 시뮬레이션합니다.

이 함수는 호스트 `ScrollView` 요소만 지원하며, 다른 요소 타입을 전달하면 오류가 발생합니다. `FlatList`는 호스트 `ScrollView` 요소로 렌더링되므로 허용됩니다.

스크롤 상호작용은 `ScrollView` 요소의 방향과 일치해야 합니다:

수직 스크롤뷰(기본값 또는 `horizontal={false}`)의 경우, `y` 옵션만 전달해야 합니다(선택적으로 `momentumY`도 전달할 수 있습니다).
수평 스크롤뷰(`horizontal={true}`)의 경우, x 옵션만 전달해야 합니다(선택적으로 `momentumX`도 전달할 수 있습니다).
각 스크롤 상호작용은 사용자가 손가락으로 스크롤뷰를 드래그하는 동작을 시뮬레이션하는 필수적인 드래그 스크롤 부분으로 구성됩니다(`y` 또는 `x` 옵션). 이후 사용자가 손가락을 들어 올린 후 스크롤뷰 콘텐츠의 관성
이동을
시뮬레이션하는 관성 스크롤 동작이 추가될 수 있습니다(`momentumY` 또는 `momentumX` 옵션).

### 옵션

- `y`: 목표 수직 드래그 스크롤 위치
- `x`: 목표 수평 드래그 스크롤 위치
- `momentumY`: 목표 수직 관성 스크롤 위치
- `momentumX`: 목표 수평 관성 스크롤 위치
- `contentSize`: `ScrollView` 이벤트에 전달되며 `FlatList` 업데이트를 활성화합니다.
- `layoutMeasurement`: `ScrollView` 이벤트에 전달되며 `FlatList` 업데이트를 활성화합니다.

User Event는 사용자 스크롤 상호작용을 시뮬레이션하기 위해 여러 중간 스크롤 단계를 생성합니다. 이러한 스크롤 단계의 정확한 수나 값을 신뢰해서는 안 되며, 이는 향후 버전에서 변경될 수 있습니다.

이 함수는 마지막 스크롤이 종료된 위치를 기억하므로 후속 스크롤 상호작용은 해당 위치에서 시작됩니다. 초기 스크롤 위치는 `{ y: 0, x: 0 }`으로 가정됩니다.

`FlatList`(및 `VirtualizedList` 기반의 다른 컨트롤)의 스크롤을 시뮬레이션하려면 `contentSize` 및 `layoutMeasurement` 옵션을 전달해야 하며, 이를 통해 현재 보이는
창을
업데이트하는 기본 로직이 활성화됩니다.

### 이벤트 순서

스크롤에 선택적인 관성 스크롤 구성 요소가 포함되어 있는지 여부에 따라 이벤트 순서가 달라집니다.

- 드래그 스크롤:
    - `contentSizeChange`
    - `scrollBeginDrag`
    - `scroll` (여러 이벤트)
    - `scrollEndDrag`
- 관성 스크롤 (선택 사항):
    - `momentumScrollBegin`
    - `scroll` (여러 이벤트)
    - `momentumScrollEnd`