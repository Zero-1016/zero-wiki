# OpenAPI를 사용한 API 설계

이전 단계에서 기술해둔 도메인 모델을 토대로 OpenAPI에 기술해서 프론트엔드와 백엔드를 연결한다.

## 도메인 모델을 OpenAPI로 전환

도메인 모델에서는 다양한 개념에 대해 속성, 관계, 기능을 할당한다.

일반적으로 OpenAPI 정의서의 스키마는 도메인 모델의 속성관 관계를 표현한 것이며, 개략적인 도메인 모델 관점에서는 '모델'이나 '개념'을 계속 사용할 것이고, 기술적인 구현을 위한 데이터 구조 관점에서는 '
스키마'를 사용해 구분할 것이다.

OpenAPI로 도메인 모델링을 하는 다른 문서에서는 '모델'과 '스키마'를 혼용하는 것을 볼 수도 있을 것이다.

### 재사용성 보장

이전에 API 명세서에서 사용한 스키마는 내부에 스키마를 작성하여 이러한 스키마는 '인라인 스키마'라고 부릅니다.

```yaml
type: object
properties:
  message:
    type: string
    example: An awesome time for the whole family.
  rating:
    type: integer
    minimum: 1
    maximum: 5
    example: 5
```

만약 api에서 보낼때와 받을 때, 해당 스키마에 있는 데이터를 받는다고 한다면, 우리는 중복된 스키마를 작성해야 한다. 또한, CRUD, 4군데에서 사용을 한다면, 객체명을 변경할 때 마다 4군데를 수정해야하는
번거로움이 발생한다. 이러한 오류를 방지하고자 일관성있게 작성하고자 스키마를 통해 정의하여 재사용할 수 있다.

## 스키마 생성

스키마는 다음과 같이 components 내에 작성할 수 있다.

```yaml
openapi: 3.0.3
info:
  title: PetSitter API
  version: "0.1"
path: { }
components:
  schemas: { }
```

### 공통 스키마 참조

OpenAPI 정의서의 components 항목에 공통 스키마를 생성하면, $ref 키워드를 통해 해당 스키마를 참조할 수 있습니다.

$ref 키워드의 값은 JSON 포인터를 통해 OpenAPI 정의서의 스키마를 참조합니다.

```yaml
$ref: '#/components/schemas/User'
```

## API 연산과 CRUD

일반적으로 API에서는 다음 두 가지의 종류의 API 경로를 구분할 수 있다.

- 리소스 엔드포인트
- 컬렉션 엔드포인트

여기서 말하는 리소스란 도메인 모델에 있는 개념의 개별 인스턴스를 말합니다. 패스 모델과 사용자 모델이 있을때 이러한 하나하나를 모두 리소스라고 부를 수 있다.

개별 리소스에 대한 경로 이름을 지정하는 가장 좋은 방법은 모델의 복수형 이름 뒤에 슬래시를 붙인 다음 인스턴스의 고유 식별자(아이디)를 추가하는 것이다. 예를 들어 ID가 45인 사용자의 경우 /users/45로
접근할 수 있다.

개별 리소스를 가리키는 URL을 **리소스 엔드포인트**라고 부릅니다.

하지만 개별 리소스만을 조회하는 것은 아닙니다. 사용자 목록과 같이 리소스 목록을 조회하는 경우도 있습니다. 이러한 목록을 가리키는 URL을 **컬렉션 엔드포인트**라고 부릅니다.

다음과 같이 사용 가능합니다.

| 연산          | HTTP 메소드     | URL 경로                                         |
|-------------|--------------|------------------------------------------------|
| 생성 (create) | POST         | 컬렉션 엔드포인트(/{스키마이름}s} 사용                        |
| 읽기 (read)   | GET          | 컬렉션 엔드포인트와 리소스 엔드포인트(/{스키마이름}s/${id}) 모두 사용 가능 |
| 수정 (update) | PUT 또는 PATCH | 리소스 엔드포인트 사용                                   |
| 삭제 (delete) | DELETE       | 리소스 엔드포인트 사용                                   |

### API 요청과 응답 정의

API를 통해서 상호 작용을 하려면 필수 정보인 URL, HTTP 메소드를 정해야 하고, 클라이언트가 서버로 보내는 요청 파라미터와 요청 본문도 선택사항으로 정의해야 합니다. 서버는 HTTP 상태 코드와 응답 본문을
포함하는 응답을 클라이언트에게 반환합니다.

#### 요청

읽기와 삭제 연산에는 요청 본문(body)가 없습니다. 만약 연산을 수행하기 위해 보내야할 정보가 있다면 쿼리 파라미터를 통해 보냅니다.

생성과 수정 연산에는 요청 본문이 필요하며, 스키마에 맞는 리소스 정보를 JSON으로 표현해서 요청 본문에 담아 보내야 합니다.

#### 응답

리소스 엔드포인트를 통해 읽기 연산이 수행되면 해당 자원의 스키마에 맞는 JSON 객체가 응답 본문으로 반환됩니다. 성공하면 상태 코드 200 OK가 반환되고 존재하지 않으면 404 Not Found가 반환됩니다.

컬렉션 엔드포인트를 통해 읽기 연산이 수행되면 객체로 이루어진 배열들과 선택적으로 추가적인 메타데이터 (페이지의 갯수, 총 아이템의 갯수)등에 대한 컬렉션 객체가 응답 본문으로 반환됩니다. 때문에 리소스 객체의
배열을 items라고 정하여 응답하기도 합니다.

```yaml
responses:
  '200':
    description: A list of items
    content:
      application/json:
        schema:
          type: object
          properties:
            items:
              type: array
              items:
              $ref: '#/components/schemas/Item'
```

> 비어있는 컬렉션을 반환할 때 왜 404로 반환하면 안되는가?에 대한 질문은 컬렉션이 비어 있어도 자체는 존재하므로 200으로 반환합니다. 과일 바구니처럼 과일이 없다고 하더라도 바구니는 계속 사용
> 가능합니다.

컬렉션 엔드포인트에 요청을 보내서 수행할 경우 다음과 같이 응답을 기대할 수 있습니다.

|연산|상태 코드| 응답 본문|
|-|-|-|
|생성|201|비어 있고 Location 헤더 반환|
|읽기|200|리소스 또는 컬렉션을 포함하는 객체|
|수정|200|리소스|
|삭제|204|비어있음|

생성의 경우 새로 생성된 리소스를 가리키는 리소스 엔드포인트를 응답의 Location 헤더에 담아서 반환하기도 합니다.