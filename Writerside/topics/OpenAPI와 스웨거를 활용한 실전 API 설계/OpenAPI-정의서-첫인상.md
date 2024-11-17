# OpenAPI 정의서 첫인상

API 형식에 맞게 기술하는 것은 API 포함돼 있는 아이디어를 정의서라고 부르는 데이터로 변환하는 것을 의미합니다.

이런 형식적 기술하기는 비형식적 기술하기와 달리 기계가 판독하기도 쉽습니다. 이를 읽어서 요청 검증, 코드 스텁 생성, 문서화 등 여러 부분을 자동화할 수 있습니다.

## 1. 문제 정의

오픈소스 API가 수행하는 단순한 작업을 형식적으로 기술해 본다. 먼저 API 상세 내용을 알아보고 API 정의서 일부를 작성한다.

> API 정의의 품질은 작성하기에 따라 모호한 수준부터 지나치게 정확한 수준까지 매우 다양하게 나타난다. 후자 쪽이 낫지만 너무 많은 비용이 소모되어 실익이 떨어지므로 적절한 균형점을 찾는 것이 중요하다.

## 2. OpenAPI 명세 소개

OpenAPI 명세는 RESTful API 또는 HTTP API를 기술하는 방법을 형식적으로 규정해 놓은 규격이다.

```yaml
openapi: 3.0.3
# ...
paths:
  /reviews: # 경로 또는 URL
    get: # HTTP 메소드
    description: Get a bunch of a reviews # 사람이 읽을 수 있는 작업 설명
    parameters: # 파라미터 목록
      - name: maxRating # 파라미터 이름
        description: | # 사람이 읽을 수 있는 파라미터 설명
          Filter the reviews
          by the maximum rating
        in: query # 파라미터 타입
        schema:
          type: string # 파라미터 스키마 타입(값도 허용)
    responses: # 응답 목록
      200: # HTTP 상태 코드
        description: A bunch of reviews # 사람이 읽을 수 있는 응답 설명
```

> OpenAPI 정의서는 OpenAPI 명세를 준수하는 문서이다. OpenAPI 명세를 준수하지 않는 OpenAPI 정의서는 유효하지 않다.

## 3. YAML 들여다보기

YAML은 JSON보다 제약이 더 적으며 동일한 데이터를 여러 가지 방식으로 표현할 수 있다. 예를 들어 문자열에 따옴표를 붙여도 되고 안붙여도 되며, 끝에 붙이는 쉼표도 허용된다.

또한 플로우 타입을 지원한다. (JSON의 객체나 배열을 의미한다.) 때문에 JSON의 상위 호환이라고 할 수 있다.

### 1. JSON에서 YAML로

OpenAPI는 방대한 YAML 명세 중 필요한 최소한의 일부에 집중한다. 그 일부가 바로 YAML 1.2에 있는 YAML의 JSON 스키마다.

```yaml
SomeNumber: 1
SomeString: hello over there!
IsSomething: true
#주석
SomeObject: # yaml은 중첩 객체와 배열을 포함하기 위해 들여쓰기를 사용한다.
  SomeKey: Some string value # 몇 칸을 들여써야 할지는 중요하지 안다. 파서가 자동으로 판단한다.
  SomeNestedObject:
    Key: With a nested key/value pair
AList:
- a string
- another string
SomeOldSchoolJSONObject: { one: 1, two: 2 }
SomeOldSchoolJSONArray: [ "one", "two", three ]
MultiLineString: | # |를 사용하면 문단 사이의 공백 행과 문장 끝의 공백행을 입력한 것을 그대로 유지한다.
  hello over there,
  this.is a multiline string!
```

## 4. GET 연산 기술하기

```yaml
/reviews: # 서버에 대한 상대 결로
   get: # HTTP 메소드이름 소문자로 작성
      description: Get a bunch of a reviews # 연산에 대한 설명
   responses: # 응답 객체, 응답 상태 코드별 여러 응답을 가질 수 있다.
      200: # HTTP 상태 코드
        description: A bunch of reviews # 사람이 읽을 수 있는 응답 설명
```

위 내용이 연산의 핵심 세부 정보라고 할 수 있다.

## 5. GET 연산 확장

쿼리 파라미터를 추가하기 위해서는 다음과 같이 시도할 수 있다.


해당 파라미터 정보를 병합해준다.

```yaml
/reviews:
   get: 
      description: Get a bunch of a reviews
      parameter: # 파라미터 필드
      - name: maxRating # 파라미터 객체와 첫번째 필드
        description: Filter reviews by the maximum rating
        in: query
        schema:
          type: number # 파라미터 객체 정보 종료 다음 줄에서 들여쓰기가 달라진다.
   responses:
      200: 
        description: A bunch of reviews 
```

## 정리

1. 형식적으로 기술해야 소프트웨어가 쉽게 읽을 수 있다.
2. OpenAPI 정의서는 YAML 또는 JSON 형식으로 작성된다.
3. OpenAPI는 YAML 기능 중에서 JSON 스키마 형식에 맞는 기능만 사용한다. 지원하는 타입 데이터 타입만 사용할 수 있다.
4. 연산과 연산에 사용되는 파라미터 등 여러 정보를 기술할 수 있다.