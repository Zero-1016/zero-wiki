# 테스트 환경 설정 (Configuration)

## `configure` Function

`configure` 함수는 테스트 설정을 사용자 정의하기 위해 사용됩니다. 이 함수는 `Config` 객체의 일부 또는 전체를 옵션으로 받아들입니다.

```Typescript
type Config = {
  asyncUtilTimeout: number;
  defaultHidden: boolean;
  defaultDebugOptions: Partial<DebugOptions>;
};

function configure(options: Partial<Config>) {}
```

## 옵션

### `asyncUtilTimeout`

비동기 헬퍼 함수(`waitFor`, `waitForElementToBeRemoved`) 및 `findBy*` 쿼리의 기본 타임아웃(밀리초 단위)을 설정합니다. 기본값은 1000ms입니다.

### `defaultIncludeHiddenElements`

모든 쿼리에
대해 [includeHiddenElements](https://callstack.github.io/react-native-testing-library/docs/api/queries#includehiddenelements-option)
쿼리 옵션의 기본값을 설정합니다. 기본값은 `false`로 설정되어 있으며, 이로 인해 모든 쿼리는 접근성에서 숨겨진 요소와 일치하지 않습니다. 이는 앱 사용자가
이러한 요소를 볼 수 없기 때문입니다.

이 옵션은 React Testing Library와의 호환성을 위해 `defaultHidden` 별칭으로도 사용할 수 있습니다.

### `defaultDebugOptions`

`debug()`를 호출할 때 사용할 기본 디버그 옵션을 설정합니다. 이러한 기본 옵션은 `debug()`를 호출할 때 직접 지정한 옵션에 의해 재정의됩니다.

### `resetToDefaults`

`resetToDefaults` 함수는 모든 설정을 기본값으로 재설정합니다. 이 함수는 `configure`를 통해 변경된 설정을 원래 상태로 되돌리는 데 사용됩니다.

## Environment Variables

### `RNTL_SKIP_AUTO_CLEANUP`

`cleanup()`이 각 테스트 후에 자동으로 실행되는 것을 비활성화합니다. `react-native-testing-library/dont-cleanup-after-each`을 임포트하거나
`react-native-testing-library/pure`를 사용하는 것과 동일한 방식으로 작동합니다.

```bash
$ RNTL_SKIP_AUTO_CLEANUP=true jest
```

### `RNTL_SKIP_AUTO_DETECT_FAKE_TIMERS`

가짜 타이머의 자동 감지를 비활성화합니다. 이 설정은 비정상적인 경우에 사용자가 Jest 이외의 가짜 타이머를 사용하고자 할 때 유용할 수 있습니다. 자세한 내용은 [이슈 #886](https://github.com/callstack/react-native-testing-library/issues/886)을 참조하세요.

```bash
$ RNTL_SKIP_AUTO_DETECT_FAKE_TIMERS=true jest
```