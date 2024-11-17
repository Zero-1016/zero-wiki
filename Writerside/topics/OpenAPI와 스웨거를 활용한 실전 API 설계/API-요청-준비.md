# API 요청 준비

## 1. 문제 정의

해당 장에서 해결할 문제는 오픈 소스 API 탐색 및 이해이다. 상호작용과 접속 방법, 요청 전송, 응답 분석 등 기본 지식을 쌓는데 목표를 둔다.

## 2. 포스트맨 준비

포스트맨은 편리한 UI를 제공하여 초보자와 숙련자 모두 쉽게 사용할 수 있는 범용 HTTP 도구이다.

## 7. 직접 http 연결하기

telnet과 다음 두가지 명령을 통해 HTTP 연결을 시도할 수 있다.

```bash
$ telnet farmstall.designapis.com 80
# https의 경우
$ openssl s_client -quiet -connect farmstall.designapis.com:443
```

명령을 실행하면 터미널이 멈추고 서버로 보낼 다음 응답을 기다립니다.

```Bash
GET /v1/reviews HTTP/1.1 <enter> # 메소드, URL, HTTP 프로토콜 버전을 포함하는 상태 행
Host: farmstall.designapis.com <enter> # 서버가 여러 사이트를 호스팅하는 경우가 많으므로 명시
<enter> # 비어 있는 행을 입력해서 헤더와 요청 본문을 구분한다.
```