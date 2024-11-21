# API 응답 기술하기

## 1. HTTP 응답

OpenAPI를 사용해 HTTP 응답을 기술할 때는 최소한의 상태 코드와 설명이 있어야 한다.

예외로 선택 항목인 응답 본문도 응답에 포함된다면 응답 본문에 대한 미디어 타입도 지정을 해두어야 합니다. (예: application/json)

OpenAPI는 데이터 모양새를 기술하기 위해 JSON 스키마 명세를 사용합니다.

```yaml
responses:
  200:
    description: 성공 응답 # 응답 설명
    content:
      application/json: # JSON용 미디어 타입
      schema: # 스키마(OpenAPI 방식의 JSON 스키마)
        type: object
        properties:
          #...
```

## 2. 문제 정의

응답을 받아야 할 데이터는 다음과 같습니다.

| 필드           | 타입      | 설명          | 제한                  |
|--------------|---------|-------------|---------------------|
| isSubscribed | boolean | 구독을 했는지의 여부 |                     |
| pathId       | number  | 패스의 고유 아이디  | primary             | 
| startDate    | date    | 패스 시작 날짜    |                     |
| endDate      | date    | 패스 끝나는 날짜   | startDate보다 뒤에 있어야함 |
| level        | string  | 패스의 난이도     |                     |
| maxStudent   | number  | 패스를 최대 수용인원 | 최저 6명               |

```yaml
openapi: 3.0.3
info:
  description: Path 소개 정보를 제공하는 API
  version: 1.0.0
paths:
  /pass/{pathId}:
    get:
      summary: Get pass details
      description: 특정 패스의 정보를 가져옵니다.
      parameters:
        - name: pathId
          in: path
          required: true
          description: 패스의 고유 ID
          schema:
            type: integer
      responses:
        '200':
          description: 패스 정보 가져오기 성공
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Pass'
        '404':
          description: 패스를 찾을 수 없음
components:
  schemas:
    Pass:
      type: object
      properties:
        isSubscribed:
          type: boolean
          description: 구독 여부
        pathId:
          type: integer
          description: 패스의 고유 아이디
        startDate:
          type: string
          format: date
          description: 패스 시작 날짜
        endDate:
          type: string
          format: date
          description: 패스 끝나는 날짜 (startDate보다 뒤에 있어야 함)
        level:
          type: string
          description: 패스의 난이도
        maxStudent:
          type: integer
          description: 패스의 최대 수용 인원 (최소 6명)
          minimum: 6
```

## 3. 놀라운 데이터 스키마의 세계

애플리케이션의 사용자는 데이터가 어떤 구조로 저장될지, 어떤 구조에 저장될 수 있는지를 알아야 데이터를 사용할 수 있다.

이때 데이터 스키마를 데이터 모양세라고 생각하는 것도 괜찮다.

> `number` 대신 `integer`를 사용한 이유는 `integer`는 정수만 허용하기 때문입니다. 반면 `number`는 정수와 소수점을 모두 허용합니다.

## 5. 상태 코드

상태 코드는 HTTP 응답에 표시되는 세 자리 숫자다. 100에서 599까지의 값을 가지며, 응답을 둘러싼 다양한 상황을 개괄적으로 나타냅니다.

| 범위  | 카테고리          | 참고                              |
|-----|---------------|---------------------------------|
| 1xx | Informational | 서버가 요청을 수신했음을 알리는 임시 응답         |
| 2xx | Success       | 200 OK나 201 Created처럼 요청 성공을 의미 |
| 3xx | Redirects     | 요청 자원의 위치/URI가 변경됐음을 의미         |
| 4xx | Client error  | 클라이언트가 제공한 요청 정보에 오류가 있음을 의미    |
| 5xx | Server error  | 서버 처리과정에서 오류가 있음을 의미            |

## 6. 미디어 타입(MIME)

HTTP는 멀티미디어 프로토콜이라서 여러 가지 형식의 요청과 응답을 처리할 수 있습니다. 오가는 데이터가 어떤 형식인지 알려주기 위해 일반적으로 Content-Type 헤더에 미디어 타입을 명시합니다.

미디어 타입 또는 MIME은 요청이나 응답에 포함된 데이터 형식을 표시하는 방법을 나타냅니다.

미디어 타입은 타입과 서브타입, 선택적 파라미터로 구성됩니다. 타입은 text, audio, image와 같은 상위 카테고리이고 서브타입은 text/plain, image/png와 같이 구체적인 정보를 포함합니다.

| 미디어 타입           | 설명                |
|------------------|-------------------|
| text/html        | 웹 서버가 반환해주는 HTML  |
| text/csv         | 쉼표로 구분된 값         |
| image/png        | PNG 형식으로 인코딩된 이미지 |
| application/json | JSON 데이터          |
| application/xml  | XML 데이터           |

## 7. 응답 기술하기

### 7.1 초 미니 응답

응답에는 하나의 상태 코드가 대응됩니다.

최소한의 응답을 정의한 코드는 다음과 같다.

```yaml
paths:
  /path/{pathId}:
    get:
      responses:
        '200':
          description: 패스 소개 정보를 가져옵니다.
```

### 7.2 응답 본문

응답 뼈대가 오나성됐으므로 JSON 스키마를 추가할 수 있습니다. 하지만 그 전에 content 필드에 적어도 하나의 미디어 타입(application/json)을 명시해야 합니다.

```yaml
paths:
  /path/{pathId}:
    get:
      responses:
        '200':
          description: 패스 소개 정보를 가져옵니다.
          content: # 응답 본문은 content 아래에 작성한다.
            application/json: # 응답 본문의 미디업 타입
              schema: # 데이터의 모양을 나타내는 스키마가 포함된다.
                type: array
                items:
                  type: object
                  properties:
                    parts:
                      type: object
                    message:
                      type: string
                      nullable: true
```

> OpenAPI에서 배열은 `type: array`로 명시하며, 배열의 아이템은 `items` 키워드를 사용해 정의합니다.

> OpenAPI에서는 JSON 스키마와 같이 타입을 배열로 줄 수 없다. 따라서 `nullable`과 같은 키워드를 통해 추가할 수 있다.

