# API 개요
React Native Testing Library는 다음과 같은 API로 구성되어 있습니다:

- [**`render` 함수 사용법**](render-함수-사용법-(render-함수).md) - UI 구성 요소를 테스트 목적으로 렌더링합니다.
- [**`Screen` 객체 활용법**](Screen-객체-활용법-(screen-object).md) - 렌더링된 UI에 접근할 수 있습니다:
  - [**쿼리 사용 가이드**](쿼리-사용-가이드(Queries).md) - 역할, 텍스트, 테스트 ID 등을 기준으로 관련 구성 요소를 찾습니다.
  - **생명주기 메서드**: `rerender`, `unmount`
  - **도구들**: `debug`, `toJSON`, `root`
- [**Jest 매처 활용법**](Jest-매처-활용법-(Jest-Matchers).md) - UI에 대한 가정을 검증합니다.
- [**사용자 이벤트 시뮬레이션**](사용자-이벤트-시뮬레이션(User-Event-Interactions).md) - 사용자 상호작용(예: 클릭, 입력)을 현실적으로 시뮬레이션합니다.
- [**이벤트 트리거 방법**](이벤트-트리거-방법-(Fire-Event-API).md) - 간소화된 방식으로 구성 요소 이벤트를 시뮬레이션합니다.
- **기타 API**:
  - [**`renderHook` 훅 사용법**](renderHook-사용법-(renderHook-function).md) - 테스트를 위해 훅을 렌더링합니다.
  - [**비동기 유틸리티 활용**](비동기-유틸리티-활용(Async-Utilities).md): `findBy*` 쿼리, `wait`, `waitForElementToBeRemoved`
  - [**테스트 환경 설정**](테스트-환경-설정-(Configuration).md): `configure`, `resetToDefaults`
  - [**접근성**](접근성-(Accessibility).md): `isHiddenFromAccessibility`
  - [**추가 유틸리티 도구**](추가-유틸리티-도구-(Other-Helpers).md): `within`, `act`, `cleanup`