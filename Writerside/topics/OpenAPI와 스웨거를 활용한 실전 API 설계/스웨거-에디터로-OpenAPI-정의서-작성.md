# 스웨거 에디터로 OpenAPI 정의서 작성

스웨거 에디터는 OpenAPI 정의서 작성을 도와주는 도구입니다.

`https://editor.swagger.io/` 사이트에서 호스팅 되는 웹 애플리케이션이며 다운로드하여 자체 웹 서버에 직접 호스팅하여 사용할 수도 있습니다.

우리가 입력한 결과물이 즉각적으로 표시되므로 텍스트 에디터에 작성한 내용이 올바른지 바로 피드백을 받을 수 있습니다.

## 1. 스웨거 에디터 소개

온라인 서비스를 이용할 수도 있고, 자체 웹 서버, 도커 등에 호스팅 할 수도 있다. 해당 책에서는 최신 버전으로 서비스되는 온라인 버전을 사용한다.

### 1.1 에디터 패널

에디터 패널을 통해 OpenAPI 정의서 YAML 파일을 작성할 수 있는 텍스트 에디터를 제공한다. 작성 내용이 OpenAPI 정의서가 된다.

### 1.2 UI 문서 패널

에디터 패널에 작성된 내용이 UI로 만들어저 표시된다.

### 1.3 도구 메뉴

URL로부터 정의서 가져오기, 스텁 및 SDK 코드 자동 생성 OpenAPI 코드 자동 생성 유틸리티 등이 포함되어 있다.

## 2. 스웨거 에디터에서 OpenAPI 정의서 작성

### 2.1 유효한 미니 OpenAPI 정의서

OpenAPI 정의서가 유효하려면 최소한 다음 세 가지 조건이 충족돼야 한다.

- OpenAPI 정의서 식별자와 사용한 OpenAPI 명세 버전
- title, version 필드를 포함하는 info 객체
- 비어있는 paths 객체

```yaml
openapi: 3.0.3
info: # 기술할 API 메타데이터를 info 객체에 저장
  title: a-ni API # 인간 친화적인 API 정의서 이름
  version: v0.0.1 # API 버전을 의미하며 아무 문자열이나 값으로 지정할 수 있다.
paths: {}
```
### 2.2 스웨거 에디터에서 Open API 작성

![image.png](스웨거 에디터를 위한 최소 조건.png)

### 2.3 검증

잘못된 입력이 있을 경우 에디터 패널에서 바로바로 오류를 보여준다.

## 3. GET /path 추가

```yaml
openapi: 3.0.3
info: # 기술할 API 메타데이터를 info 객체에 저장
  title: a-ni API # 인간 친화적인 API 정의서 이름
  version: v0.0.1 # API 버전을 의미하며 아무 문자열이나 값으로 지정할 수 있다.
servers:
  - url: http://localhost:9090
paths: 
  /introduce:
    get:
      description: 패스의 소개 페이지에 해당하는 정보를 호출합니다.
      parameters:
        - name: pathId
          description: 패스의 아이디로 소개 페이지에 해당하는 내용을 호출합니다.
          in: query
          schema:
            type: number
      responses:
        200:
          description: 패스의 소개 내용을 불러옵니다.
```

![image.png](스웨거 에디터 일반 호출 예시 파일.png)

## 4. API 호출

### 4.1 /path 호출

try-it-out 기능을 통해 api 실행이 가능하다.

```yaml
servers:
  - url: http://localhost:9090
```

servers를 기술해줌으로서 서버 위치를 지정할 수 있다.

![image.png](텍스트 에디터로 서버로 직접 호출하기.png)