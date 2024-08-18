# React Native 앱의 네이티브 기능을 테스트할 수 있나요?

짧은 답변: 아니요.

React Native Testing Library는 전체 React Native 런타임을 제공하지 않습니다. 전체 런타임을 제공하려면 물리적 기기나 iOS 시뮬레이터/안드로이드 에뮬레이터에서 OS와 플랫폼 API를 사용할 수 있어야 하기 때문입니다.

대신, React Native 렌더러를 사용하는 대신 React Test Renderer를 통해 런타임의 JavaScript 부분만을 시뮬레이션하며, 실제 런타임의 특정 동작을 모방하는 쿼리 및 이벤트 API(User Event, Fire Event)를 제공합니다.

우리의 테스트 환경에 대해 더 자세히 알아볼 수 있습니다 [여기](https://github.com/callstack/react-native-testing-library/blob/main/docs/Environment.md)에서 확인하세요.

이 접근 방식은 특정 장점과 단점을 가지고 있습니다. 긍정적인 측면은 다음과 같습니다:

- 일반적인 React Native 앱의 대부분의 로직을 테스트할 수 있습니다.
- Jest 또는 다른 테스트 러너가 지원하는 모든 OS에서 테스트를 실행할 수 있습니다. 예: CI 환경
- 전체 런타임 시뮬레이션보다 훨씬 적은 리소스를 사용합니다.
- Jest 가짜 타이머를 사용할 수 있습니다.

부정적인 측면은 다음과 같습니다:

- 네이티브 기능을 테스트할 수 없습니다.
- 특정 JavaScript 기능을 완벽하게 시뮬레이션하지 못할 수 있지만, 이를 개선하기 위해 작업 중입니다.

User Event 상호작용은 기본 Fire Event API보다 더 현실적인 이벤트 처리를 제공하므로, 일부 시뮬레이션 문제를 해결할 수 있습니다.

## Screen 쿼리를 사용하거나 마이그레이션해야 할까요?

기존 테스트 코드를 screen 기반 쿼리로 마이그레이션할 필요는 없습니다. 여전히 render가 반환하는 쿼리와 기타 함수들을 사용할 수 있습니다. screen 객체는 최신 렌더 결과를 캡처합니다.

새로운 코드에서는 screen을 사용하는 것이 권장됩니다. 그 이유는 Kent C. Dodds가 작성한 [이 기사](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)에서 잘 설명되어 있습니다.

## User Event 상호작용으로 마이그레이션해야 할까요?

기존 테스트를 User Event 상호작용으로 마이그레이션하는 것이 권장됩니다. 이는 기본 Fire Event API보다 더 현실적인 이벤트 처리를 제공하므로, 코드의 품질에 대한 신뢰도를 높일 수 있습니다.
