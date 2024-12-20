# Node.js와 스웨거 코드젠으로 백엔드 구축

## 스웨거 코드젠 소개

코드젠은 'code generation' 줄임말이며, OpenAPI 정의서 파일을 입력받아 여러 가지 언어로 작성된 코드를 결과물로 생성할 수 있습니다.

주요 기능은 클라이언크 코드 생성과 서버 코드 생성을 할 수 있습니다.

### 클라이언트 코드 생성

코드젠을 통해 생성된 클라이언트 코드는 일종의 SDK 라고 볼 수 있습니다. API 관점에서 SDK는 API를 감싸는 라이브러리라고 할 수 있습니다. 개발자는 직접 HTTP 요청을 생성해서 API를 호출하는 대신에
SDK를 사용해서 API와 연동되는 애플리케이션을 만들 수 있습니다.

### 서버 코드 생성

서버 코드 생성은 스텁 코드 생성이라고 볼 수 있다.

> 프로그래밍에서 **스텁**이란 쉽게 말해 아직 완전하게 작성되지 않은 미완 코드를 의미한다.

이렇게 만들어진 코드는 실제 데이터를 반환하는 것이 아닌 모의 데이터를 반환합니다. 따라서 개발자용 빈칸 채우기 퍼즐과 같다고 볼 수 있습니다. 이런식으로 코드젠은 서버 애플리케이션의 초안을 만들어 줍니다.

[온라인 버전](https://generator.swagger.io/)을 통해 손쉽게 사용할 수 있습니다.

~하지만 해당 문서에서는 실행할 수 없으므로 다른 [링크](https://editor.swagger.io/)를 통해 시도하기 바란다.~

![스웨거 코드젠으로 백엔드 구축-1.png](스웨거 코드젠으로 백엔드 구축-1.png)

다음과 같이 손쉽게 서버 코드를 발급받을 수 있다.

만들어진 파일에는 백엔드 프로젝트 코드를 담고있는 코드젠 버전이 명시돼 있는 파일들이 있다.

각 service 디렉터리는 Service 파일을 포함하고 있다. 이 파이ㅏㄹ에는 controllers의 Default 파일안에 있던 함수가 호출하는 코드가 들어있다.

각 API 연산에는 하나의 컨트롤러 함수와 하나의 서비스 함수가 대응되며 두 함수는 동일한 이름을 갖고 있다.

서비스 함수는 API 연산이 반환하는 mock 응답을 갖고있다.

## 백엔드 OpenAPI 수정

앞서 살펴본 것처럼 코드젠은 입력받은 OpenAPI 정의서에 우리가 직접 정의하지 않는 몇 가지 항목을 추가한다. 그런 항목을 직접 정의하면 코드젠이 그 값을 읽고 그에 맞는 코드를 생성해냄로 더 나은 백엔드 구조를
만들어 낼 수 있다.

### operation ID 추가

하나의 연산을 식별하는 유일한 식별자이므로 하나의 OpenAPI 정의서 파일 안에서 동일한 operation ID 값은 한 번만 사용할 수 있다.

| 메소드    | 경로                          | operation ID          |
|--------|-----------------------------|-----------------------|
| POST   | /users                      | registerUser          |
| GET    | /users/{id}                 | viewUserWithId        |
| DELETE | /jobs/{id}/job-applications | viewApplicationForjob |

```yaml
openapi: 3.0.3
paths:
  /users:
  post:
  summary: Register User
  operationId: registerUser
  responses:
    '201':
      description: Created
      headers:
        Location:
          schema:
            ty[e: string
  requestBody:
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/User'
```

### API 연산에 태그 지정

**아키텍처를 설계할 때는 하나의 파일에 너무 많은 내용이 들어가지 않도록 주의하고 적절한 단위로 분리하는것이 중요하다.**

- 스웨거 UI는 태그를 기준으로 연산을 여러 개로 그룹 지어 표시한다. 이렇게 하면 API 문서를 읽는 사람이 어떤 연산들이 함께 묶여 있고 API의 목적이 무엇인지 더 쉽게 파악할 수 있다.
- 코드젠은 태그를 사용해 컨트롤러와 서비스 코드를 생성한다. 각 API 연산에 대한 컨토롤러 메소드는 하나의 컨트롤러 파일 안에만 정의되므로 라우터는 요청을 어느 컨토롤러 함수에 전달할지 알 수 있다.

하나의 연산에 여러 개의 태그를 지정하면 해당 연산이 여러 번 표시되고 아무 문제가 되지 않지만 소스 코드에서는 동일한 연산이 여러 곳에 중복으로 구현돼서는 안 된다. 따라서 코드젠은 하나의 연산에 열 ㅓ태그가
지정돼 있더라도 첫 번째 태그 하나에 대해서만 코드를 생성하며, 모호함을 피하기 위해 x-swagger-router-controller를 사용한다.


### 태그 선정

API 정의서에 사용할 적절한 태그를 선정하려면 도메인 모델과 연산 URL을 살펴보는 것이 좋다. 대부분 도메인 모델에 사용되는 개념이나 URL 경로의 첫 번째 부분을 태그로 사용할 수 있다.

### 태그 할당

다음과 같이 연산에 태그를 붙일 수 있다.

```yaml
openapi: 3.0.3
paths:
  /users:
  post:
    tags:
      - Users
  summary: Register User
  operationId: registerUser
  responses:
    '201':
      description: Created
      headers:
        Location:
          schema:
            ty[e: string
  requestBody:
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/User'
```

자동 생성된 코드는 OpenAPI 정의서에 있는 스키마를 바탕으로 입력값 검증 기능도 수행하며, 스키마에 맞지 않는 입력값이 들어오면 걸러낸다. 따라서 API를 설계할 때 스키마를 잘 설계해야 한다.
