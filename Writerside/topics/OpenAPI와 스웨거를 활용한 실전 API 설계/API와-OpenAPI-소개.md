# API와 OpenAPI 소개

## 1.API 생태계란?

API는 각 서비스가 제공할 수 있는 것이 무엇이고, 어떻게 상호작용할 수 있는지 정의한다. 'API 설계자'의 역할은 사용자로부터 피드백을 받아 변경사항에 대해 미리 협의할 수 있도록 API를 설계하는 것이다.

## 2.API 기술하기

생태계를 개별 서비스라고 생각한다면 개별 서비스가 어떻게 전체 시스템을 형성하는지 알 수 없게 된다.

> 전체를 이해할 수 있는 큰 그림을 그리려면 개별 서비스가 어떻게 연결돼 있는지를 알아야 한다.

### 1. 브리짓의 업무

브리짓이 관리하고 있는 웹 서비스는 여러 서비스가 서로 의존하며 협력하고 있다. 또한 브리짓의 통제 밖에 있는 외부 서비스도 사용한다.

API를 변경하면 종종 문제가 발생하며 해당 API를 사용하는 서비스가 제대로 동작하지 않게 되기도 한다. 때문에 브리짓은 고민한다.

"API가 변경되면 그 변경에 영향받는 개발자가 대응할 수 있도록 미리 알려서 생태계가 깨지지 않고 돌아가게 만들 수 없을까?"

브리짓은 생태계를 깨트리는 변경사항이 생길 경우 보고서를 생성하는 프로그램을 작성했다, 해당 보고서는 단순하여 연동 실패를 유발할 가능성이 있는지 쉽게 볼 수 있었다.

### 2. 브리짓 해법의 잠재력

해당 보고서를 통해 문서 생산, 빌드 전 변경 부분, 테스트, 불필요한 준비 코드 절감 등 다양한 곳에서 활용할 수 있었다.

## 3. OpenAPI란?

다시 다룰 RESTful API를 포함하는 HTTP 기반 API를 설명하는 방식을 정해놓은 규격이다. YAML이나 JSON 파일로 작성되며 API의 입력과 출력을 나타냅니다.

## 4. OpenAPI 정의서는 어디에 사용하는 것이 좋을까?

API 정의서가 만들어지면 자동화된 워크 플로를 만들 수 있다. API 정의서를 기계가 읽을 수 있기 때문이다.

이를 이용하여 API 테스트 자동화, 설계 조기 피드백, 일관성 보장, 버전별 API 변경사항 비교등 가능하다.

## 5. 스웨거란?

스웨거 프로젝트는 스웨거 UI와 대략적인 YAML 파일 작성 가이드로 시작되었다. 이후 가이드를 토대로 더 많은 도구가 만들어졌고 명세와 표준이 되었다.

## 6. REST란?

네트워크 설계 방법에 대한 아이디어 모음이다.

REST에 담겨 있는 아이디어는 단순함, API와 API를 공급하는 서비스를 분리하는 데 목표를 두고 있다. REST는 요청-응답 모델을 사용하며, 무언가를 처리하기 위해 필요한 모든 정보가 포함돼 있으므로 상태를 관리하지 않는 무상태 방식이다.

REST의 핵심 아이디어 중 하나는 바로 자원이다. 사용자 계정, 과금 알림, 날씨 등, 각 자원은 URI에 의해 식별될 수 있다. 포스트 정보는 `/post/4`이라는 URL을 가질 수 있으며 API 안에서 유일하게 식별할 수 있는 자원을 의미한다.

HTTP는 프로토콜이고 REST는 API를 기술하는 한 방식이다. HTTP에는 REST의 아이디어가 스며들어 있으며 REST 설계 패턴을 따르지 않는 HTTP API를 보통 RESTfull 하지 않다고 말한다.

## 7. OpenAPI는 언제 사용하는가?

OpenAPI를 항상 사용할 필요는 없다. OpenAPI는 RESTful API를 포함하는 HTTP API를 기술하는데 사용되므로 HTTP API를 설계하고 관리하고 사용할 때 OpenAPI를 사용하면 충분히 효과를 볼 수 있다.

### 사용자의 입장

API를 사용한다면 본능적으로 찾아보는 것은 본인만의 SDK다. OpenAPI로 문서가 기술되어 있다면, 여러 언어로 SDK를 자동으로 만들어 낼 수 있다.

### 제공자의 입장

OpenAPI 정의서를 사용하여 준비 코드와 스텁을 자동으로 생성할 수 있다면 필요에 맞게 템플릿을 커스터마이징해서 생산성과 일관성을 높일 수 있다.

입력 값이 OpenAPI 정의서의 스키마에 부합하는지 확인하는 검증 계층으로 사용할 수도 있다. 이런 방식은 작은 서비스를 빠르게 개발 및 출시하는 마이크로서비스 지향 아키텍처에서 점점 더 많이 활용되고 있다.

### 설계자의 입장

애자일 방식도 좋지만, API 설계는 오랫동안 사용되어야 한다는 점을 염두에 두고 진행돼야 한다. 왜냐하면 API 변경은 API 설계자의 통제 범위 밖에 존재하는 API 사용자에게도 변경을 유발하기 때문이다.

이러한 OpenAPI를 이용한 방법은 사용자와 제공자 사이의 의사소통을 도와주는 매체 역할을 하기도 한다. 이를 통해 이른 시기에 피드백을 받을 수 있고, 반복적으로 설계를 개선할 수도 있다.

## 정리

- OpenAPI는 RESTful API를 포함하는 HTTP 기반 API 명세이다.
- 스웨거는 스마트베어의 도구 세트를 지칭하지만 OpenAPI 명세 자체를 의미하기도 한다.
- YAML 형식의 API 정의서 작성을 통해 다양한 API 관련 프로세스에 여러 도구를 사용해 자동화할 수 있다.
- OpenAPI는 API 사용자, 제공자, 설계자 모두에게 유용하다. 각자의 입장에서 정의서를 이해하는 도구를 사용해 자동화의 장점을 누릴 수 있다.