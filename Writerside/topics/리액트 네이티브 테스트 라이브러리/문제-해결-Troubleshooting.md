# 문제 해결 (Troubleshooting)

이 가이드는 React Native Test Library를 프로젝트에 통합할 때 발생할 수 있는 일반적인 문제들을 설명합니다.

## React Native, React 및 React Test Renderer 버전 일치 확인

핵심 종속성의 버전이 일치하는지 확인하세요:

- React Native
- React
- React Test Renderer

React Native는 React와 다른 버전 관리 방식을 사용하므로, React Native와 React 버전 간의 일치를
확인하려면 [React Native Upgrade Helper](https://react-native-community.github.io/upgrade-helper/)를 사용할 수 있습니다. Expo를 사용하는
경우, Expo에서 권장하는 종속성 버전을 사용해야 하며, `expo upgrade` 명령어를 통해 설정할 수 있습니다.

React Test Renderer는 React와 밀접하게 관련되어 있으며, React 모노레포의 일부이기 때문에 일반적으로 React와 동일한 주요 및 부 버전을 가집니다.

관련
이슈: [#1061](https://github.com/callstack/react-native-testing-library/issues/1061), [#938](https://github.com/callstack/react-native-testing-library/issues/938), [#920](https://github.com/callstack/react-native-testing-library/issues/920)

이 문제와 관련된 오류는 다음과 같습니다:

- `TypeError: Cannot read property 'current' of undefined` (render() 호출 시 발생)
- `TypeError: Cannot read property 'isBatchingLegacy' of undefined` (render() 호출 시 발생)

## 예제 저장소

우리는 TypeScript 등을 포함한 최신 React Native Testing Library 설정을 보여주는 예제 저장소를 유지 관리하고 있습니다.

설정에서 문제가 발생하면 이 저장소를 참조하여 권장 설정을 확인할 수 있습니다.

## 정의되지 않은 컴포넌트 오류

경고: `React.jsx: type is invalid -- expected a string (for built-in components) or a class/function (for composite components) but got: undefined.`

이 오류는 복잡한 모듈을 잘못 목(mock)했을 때 자주 발생합니다. 예를 들어:

```javascript
jest.mock('@react-navigation/native', () => {
  return {
    useNavigation: jest.fn(),
  };
});
```

위의 목(mock)은 의도한 대로 `useNavigation` 훅을 목(mock)하지만, 동시에 `@react-navigation/native` 패키지의 다른 모든 내보내기(export)가 undefined로 처리됩니다.
동일한 패키지에서 `NavigationContainer` 컴포넌트를 사용하려고 하면 정의되지 않아서 위와 같은 오류가 발생할 수 있습니다.

주어진 패키지의 일부만 목(mock)하려면 `jest.requireActual` 도우미를 사용하여 다른 모든 내보내기를 다시 내보내야 합니다.

```Typescript
jest.mock('@react-navigation/native', () => {
  return {
    ...jest.requireActual('@react-navigation/native'),
    useNavigation: jest.fn(),
  };
});
```

이 방법으로 목(mock)은 `@react-navigation/native`의 모든 멤버를 다시 내보내고 `useNavigation` 훅만 덮어쓰게 됩니다.

또한, `jest.spyOn`을 사용하여 패키지 내보내기를 선택적으로 목(mock)할 수도 있습니다.

## React Native 목킹(Mock)

`react-native` 패키지를 목킹할 경우 전체 패키지를 한 번에 목킹하지 않는 것이 좋습니다. 이는 `jest.requireActual` 호출과 관련된 문제가 발생할 수 있기 때문입니다. 이 경우 패키지 내의 특정
라이브러리 경로를 목킹하는 것이 좋습니다. 예를 들어 다음과 같습니다.

```Typescript
// jest-setup.ts
jest.mock('react-native/Libraries/EventEmitter/NativeEventEmitter');
```

## Act 경고

테스트를 작성할 때 `act()` 함수와 관련된 경고를 접할 수 있습니다. 이러한 경고에는 두 가지 종류가 있습니다:

1. 동기 act() 경고 - `Warning: An update to Component inside a test was not wrapped in act(...)`
2. 비동기 act() 경고 - `Warning: You called act(async () => ...) without await`

`act()` 함수에 대해 더 자세한 내용은 [act 함수 이해 가이드](https://callstack.github.io/react-native-testing-library/docs/understanding-act)에서 확인할 수 있습니다.

일반적으로 동기 `act()` 경고는 발생하지 않지만, 발생할 경우 이는 테스트에 문제가 있음을 나타내며 조사되어야 합니다.

비동기 `act()` 함수의 경우, 특히 컴포넌트에 비동기 로직이 포함된 경우, 이 경고가 더 자주 발생할 수 있습니다. 현재까지 이 경고는 테스트의 정확성에 영향을 미치지 않는 것으로 보입니다.