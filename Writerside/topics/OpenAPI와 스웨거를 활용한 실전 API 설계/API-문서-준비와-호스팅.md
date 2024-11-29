# API 문서 준비와 호스팅

API의 라이선스와 담당자 연락처 등 API 사용자에게 필요한 중요한 정보, 메타데이터를 API 정의서의 info 부분에 추가할 것이다.

메타데이터는 말장난 같지만 데이터에 대한 데이터이다. OpenAPI를 다루는 문맥에서 메타데이터란 API 정의서에 대한 데이터라고 할 수 있다.

## 문제 정의

다음과 같이 정의할 수 있다.

```yaml
info:
  version: v1
  title: FarmStall API
  description: |- # 여러줄이 들어가므로 멀티라인 문자열을 사용한다.
    An API for writing reviews about your favorite or worst
    farm stalls. # 멀티라인이므로 들여쓰기를 맞춰야한다.
  contact:
    name: Josh Ponelat
    email: jasd@gmail.com
    url: https://farmstall.designqapis.com
  license:
    url: https://www.apache.org/licenses/LICENSE-2.0.html
    name: Apache 2.0
# ...info 섹션 이하 생략
  externalDocs:
    url: https://farmstall.designapis.com
    description: Hosted docs
```

## 태그로 연산 그룹 짓기

API 정의서에서 연산을 조직화할 수 있도록 OpenAPI에서는 태그 기능을 지원합니다.

```yaml

tags:
  - name: Reviews # 태그 설명 추가
    description: Reviews of your favorite / worst farm stalls
  - name: Users
    description: Users and authorization

paths:
  /reviews:
    get:
      tags: # 연산에 태그 추가
        - Reviews
      description:
      # ...
```

## 배포 진행하기

배포를 진행하기 위해서 두 개의 파일이 필요하다. html 파일과 openapi가 정의되어 있는 yml 파일이다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" type="text/css" href="//unpkg.com/swagger-ui-dist@3/swagger-ui.css">
  <title>FarmStall API v1</title> <!--스웨거 UI가 주입될 DOM 요소-->
<body>
  <div id="farmstall-docs" /> <!--스웨거 UI JS 번들-->
  <script src="//unpkg.com/swagger-ui-dist@3/swagger-ui-bundle.js"></script>
  <script>
    window.onload = function() { // 브라우저가 로딩을 마쳤을 때 실행
      const ui = SwaggerUIBundle({ // 인스턴스 초기화
        url: "openapi.yml", // 상대 경로 매핑
        dom_id: "#farmstall-docs", // HTML이 주입되는 DOM 요소의 id
        deepLinking: true, // 각 연산에도 링크 url을 만들어주는 추가 기능
      })
    }
  </script>
</body>
</html>
```

파일 경로는 다음과 같다.

```text
📂 root
    ⚛️ index.html
    📜 openapi.yml
```

해당 파일을 업로드함으로서 배포를 진행할 수 있다.

netlify를 통해 손쉽게 배포를 진행할 수 있다.

https://melodic-puppy-58c76c.netlify.app/