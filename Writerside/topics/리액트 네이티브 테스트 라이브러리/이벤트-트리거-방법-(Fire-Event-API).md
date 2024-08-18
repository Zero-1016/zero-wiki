# 이벤트 트리거 방법 (Fire Event API)

```typescript
function fireEvent(element: ReactTestInstance, eventName: string, ...data: unknown[]): void;
```

> 참고:
> `press`나 `type`과 같은 일반적인 이벤트의 경우, 더 현실적인 이벤트 시뮬레이션을 제공하는 User Event API를 사용하는 것이 좋습니다. User Event는 React Native 런타임
> 동작을 모방하여 올바른 이벤트 객체로 일련의 이벤트를 발생시킵니다.

Fire Event API는 User Event에서 지원되지 않는 경우나, 컴포지트 컴포넌트에서 이벤트 핸들러를 트리거해야 하는 경우에 사용하십시오.

Fire Event API는 호스트 및 컴포지트 컴포넌트에서 모든 종류의 이벤트 핸들러를 트리거할 수 있습니다. 전달된 요소에서 시작하여 컴포넌트 트리를 하위에서 상위로 탐색하며, 전달된 이벤트 이름과 일치하는
이벤트 핸들러(`onXxx`)를 찾아 호출하려고 시도합니다.

User Event와 달리, 이 API는 이벤트 객체를 자동으로 이벤트 핸들러에 전달하지 않습니다. 이벤트 객체를 생성하는 것은 사용자의 책임입니다.

### 예시

```Javascript
import { render, screen, fireEvent } from '@testing-library/react-native';

test('fire changeText event', () => {
  const onEventMock = jest.fn();
  render(
    // MyComponent는 'Enter details'라는 플레이스홀더를 가진 TextInput을 렌더링하고
    // `onChangeText`를 handleChangeText에 바인딩합니다.
    <MyComponent handleChangeText={onEventMock} />
  );

  fireEvent(screen.getByPlaceholderText('Enter details'), 'onChangeText', 'ab');
  expect(onEventMock).toHaveBeenCalledWith('ab');
});
```

> 참고:
> 7.0 버전부터 fireEvent는 비활성화된 요소에서 이벤트 발생을 방지하는 검사를 수행합니다.

### Fire Event를 사용한 네이티브 이벤트 예시

```Javascript
import { TextInput, View } from 'react-native';
import { fireEvent, render } from '@testing-library/react-native';

const onBlurMock = jest.fn();

render(
  <View>
    <TextInput placeholder="my placeholder" onBlur={onBlurMock} />
  </View>
);

// 'on' 접두사를 생략할 수 있습니다.
fireEvent(screen.getByPlaceholderText('my placeholder'), 'blur');
```

Fire Event는 `press`, `changeText`, `scroll`과 같은 일반적인 이벤트에 대한 편의 메서드를 제공합니다.

## `fireEvent.press`

```Typescript
fireEvent.press: (element: ReactTestInstance, ...data: Array<any>) => void;
```

> 참고:
> press 상호작용에 대해서는 User Event의 press() 헬퍼를 사용하는 것이 더 현실적인 시뮬레이션을 제공하므로 권장됩니다.

### 예시

```Javascript
import { View, Text, TouchableOpacity } from 'react-native';
import { render, screen, fireEvent } from '@testing-library/react-native';

const onPressMock = jest.fn();
const eventData = {
  nativeEvent: {
    pageX: 20,
    pageY: 30,
  },
};

render(
  <View>
    <TouchableOpacity onPress={onPressMock}>
      <Text>Press me</Text>
    </TouchableOpacity>
  </View>
);

fireEvent.press(screen.getByText('Press me'), eventData);
expect(onPressMock).toHaveBeenCalledWith(eventData);
```

## `fireEvent.changeText`

```Typescript
fireEvent.changeText: (element: ReactTestInstance, ...data: Array<any>) => void;
```

> 참고:
> 텍스트 변경 상호작용에 대해서는 User Event의 `type()` 헬퍼를 사용하는 것이 더 현실적인 시뮬레이션을 제공하므로 권장됩니다. `type()`은 문자별 타이핑, 요소 포커스 및 기타 편집 이벤트를
> 포함한
> 상호작용을 시뮬레이션합니다.

요소 또는 트리의 상위 요소에서 `changeText` 이벤트 핸들러를 호출합니다.

### 예시

```Typescript
import { View, TextInput } from 'react-native';
import { render, screen, fireEvent } from '@testing-library/react-native';

const onChangeTextMock = jest.fn();
const CHANGE_TEXT = 'content';

render(
  <View>
    <TextInput placeholder="Enter data" onChangeText={onChangeTextMock} />
  </View>
);

fireEvent.changeText(screen.getByPlaceholderText('Enter data'), CHANGE_TEXT);
```

## `fireEvent.scroll`

```Typescript
fireEvent.scroll: (element: ReactTestInstance, ...data: Array<any>) => void;
```

### ScrollView에서의 예시

```Typescript
import { ScrollView, Text } from 'react-native';
import { render, screen, fireEvent } from '@testing-library/react-native';

const onScrollMock = jest.fn();
const eventData = {
  nativeEvent: {
    contentOffset: {
      y: 200,
    },
  },
};

render(
  <ScrollView onScroll={onScrollMock}>
    <Text>XD</Text>
  </ScrollView>
);

fireEvent.scroll(screen.getByText('XD'), eventData);
```

> 참고:
> `ScrollView`, `FlatList`, `SectionList` 구성 요소의 경우 `fireEvent.scroll` 대신 User Event의 `scrollTo`를 사용하는 것이 좋습니다. User
> Event는 React
> Native 런타임 동작을 기반으로 한 더 현실적인 이벤트 시뮬레이션을 제공합니다.