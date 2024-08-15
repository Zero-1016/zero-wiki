# HTTP 통신, 쿠키, 세션, JWT

각각의 클라이언트는 웹 서비스에서 개별적인 콘텐츠를 소비합니다. 그리고 매번 클라이언트에서 API 요청을 할 때마다 누구의 요청인지 정확하게 식별해야 합니다. 이러한 과정에서 **“인증”**이
이루어지지 않는다면 개인정보 유출이나 변조되는 등의 취약점이 발생할 수 있습니다.

기본적이지만 중요한 기능인 ‘로그인’에 대해서 알아보겠습니다.

로그인 하기전에 HTTP (통신 프로토콜)란 무엇일까에 대해 알아보고 가겠습니다.

<img src="HTTPS.png" alt="https"/>

## HTTP

HTTP는 클라이언트와 서버 사이에 이루어지는 요청/응답(request/response) 프로토콜입니다. 여기서 프로토콜이란 약속, 규범 이라는 뜻을 의미합니다. 즉 정해진 규칙이라는 얘기입니다.

**비연결성**이란 클라이언트에서 해당 요청을 보내면 서버에서는 해당 요청에 맞는 응답을 해주어야 합니다.

또한 **무상태(Stateless)** 라는 특징때문에 연결이 해제됨과 동시에 서버는 사용자의 상태 정보를 저장하지 않기 때문에 똑같은 사용자를만난다고 해도 이전에 통신을 한 적이 있는지, 어떤 데이터를 주고
받았는지 알지 못합니다.

이같은 특성때문에 HTTP는 ‘Stateless Protocol’ 무상태 프로토콜 이라고 불리며, 독립적인 요청과 응답을 처리함으로서, 상태를 저장해야 하는 서버의 부담을 감소시킬 수 있습니다.

그럼 로그인 로직과 같이 상태를 확인해야 할 때는 매번 데이터베이스를 왕복하면서 인증을 해야할까요?

매번 확인하는 절차를 생략하기 위해 쿠키 또는 세션을 이용합니다.

---

## 쿠키(Cookie)

쿠키(cookie)는 사용자가 웹 페이지에 접속할 때 웹 서버가 사용자의 컴퓨터에 저장하는 작은 양의 데이터를 의미합니다.

웹 페이지에서는 쿠키를 확인하여 로그인 상태나 장바구니에 상품이 담겨있는지를 확인합니다.

웹 서버는 쿠키를 사용자의 컴퓨터에 저장한 뒤 쿠키가 필요할 때 사용자의 컴퓨터에 요청을 하고 사용자의 컴퓨터는 저장된 쿠키를 웹 서버에 전송합니다.

<img src="쿠키.png" alt="쿠키"/>

**쿠키의 특징**

- 특정 웹 사이트를 재방문하거나 웹 사이트 내 다른 페이지 이동 시 다시 로그인 할 필요가 없습니다.
- 사용자의 컴퓨터에서 아이디나 비밀번호를 기억할 수 있습니다.
- 사용자의 웹 페이지 이용 패턴을 분석합니다.
- **클라이언트의 PC에 정보가 저장이 됩니다.**
- **보안에 취약합니다.**
- **클라이언트의 브라우저에서 사용 유무를 설정할 수 있습니다.**
- **도메인당 쿠키가 만들어집니다.**

---

## 세션(Session)

세션은 쿠키와 달리 보안을 이유로 사용자의 컴퓨터와 웹 서버에 모두 정보를 저장합니다.

<img src="세션.png" alt="세션"/>

웹 사이트를 방문하는 사용자의 컴퓨터에는 세션 ID 정보를 저장을 하고, 웹 서버에는 사용자의 컴퓨터 세션 ID에 대응되는 세션 정보를 저장합니다.

사용자의 컴퓨터 세션 ID가 유출되더라도 웹 서버에 주요 정보가 있기 때문에 쿠키보다는 보안성이 강합니다.

**세션의 특징**

- 서버에서 클라이언트 상태를 유지하고 있으므로, 사용자의 로그인 여부 확인이 용이하고 경우에 따라서 강제 로그아웃 등의 제재를 가할 수 있습니다.
- **서버에서 클라이언트의 상태를 모두 유지**하고 있어야 하므로, 클라이언트 수에 따른 메모리나 디스크 또느 DB에 부하가 심합니다.
- 브라우저 하나당 하나의 세션이 생성되고, 해당 브라우저에서 들어오는 요청들은 모두 동일한 세션 ID로 식별해서 처리합니다.
- 브라우저를 닫으면 자동으로 삭제되기 때문에 쿠키보다 보안이 좋습니다.

## 세션과 쿠키의 차이

**라이프 사이클**

쿠키와 세션 모두 만료시간이 있지만 쿠키는 브라우저 종료해도 계속해서 정보가 남아 있을 수 있다.

세션은 만료시간을 정할 수 있지만 브라우저가 종료되면 만료시간에 상관없이 삭제된다.

**속도**

쿠키에 정보가 있기 때문에 서버에 요청 시 속도가 빠르고, 세션은 정보가 서버에 있기 때문에 비교적 느린 처리속도를 가집니다.

**저장방식**

쿠키는 사용자의 컴퓨터 메모리에 저장하는 것을 말합니다. 예를 들어, 방문한 사이트를 저장하거나, 비회원의 장바구니 정보등을 저장하는데에 사용합니다.

**쿠키와 세션의 한계**

해커가 쿠키를 가로채서 보안 문제가 발생할 수 있고, 사용자가 몰릴 경우 서버의 부담이 커질 수 있습니다.

이러한 한계를 극복하고자 토큰의 개념이 나왔습니다.

---

## 토큰(Token)

로그인 서비스

1. 사용자가 로그인을 할 경우, 클라이언트에서 백엔드에 이메일과 비밀번호를 보내 맞는 토큰 값을 받습니다.
2. 사용자는 response로 받은 토큰 값을 PC (로컬)에 저장을 합니다.
3. 사용자는 이후 다른 요청을 보낼 때, HTTP 요청에 토큰을 동봉해서 보냅니다.
4. 서버는 사용자의 토큰 정보를 검증한 뒤 사용자 권한을 확인하고 **인가요청을 처리합니다.**

**토큰의 특징**

토큰은 세션과 달리, 쿠키처럼 사용자 PC(클라이언트)에 저장됩니다. 세션과 같이 서버에서 부담을 해야하는 비용이 없고 해당 토큰에 대한 검증 (암호화된 토큰을 복호화)하는 기능만 수행하면 되기 때문에
편리합니다.

→ 백엔드에서 요청한 유저가 누구인지에 대한 KEY 값을 암호화를 통해 저장을 합니다. 프론트에 전달을 하고 프론트는 해당 토큰을 요청마다 심어서 백엔드에게 요청을 합니다.

**토큰의 장점**

1. 백엔드가 유저 정보를 갖고 있을 필요없이, 해당 코드를 복호화하여 DB조회를 통해 조회를 하여 해당 유저가 누구인지 알아냅니다.
2. 인증 토큰이 있다는 것은 사이트를 통해 정상적으로 접근한 유저를 의미합니다.

## JWT

<img src="JWT.png" alt="jwt"/>

JWT통신방식

**JWT (JSON Web Token) 인증에 필요한 정보들을 암호화시킨 JSON 토큰**

주로 msa에 사용이 됩니다. 키만 알고있다면 어떤 서버에서든 jwt 토큰을 주고 받을 수 있습니다.

- msa : 백엔드와 프론트서버를 분리하는 것을 의미합니다. 예를들어 결제와 송금과 같이 여러 서비스가 있다면 송금이 점검이 필요하다면 둘다 서버를 다운하고 고치는게 아니라 송금만 서버를 다운시키고 결제 서비스는
  그대로 유지하는것처럼 서비스들을 분리하는 것을 의미합니다. (한 서버가 죽더라도 여러 서버가 살아있습니다.)

JWT는 웹에서 인증과 권한 허가를 관리하는 데 주로 사용되는 경량화된 토큰을 의미합니다. 주로 인증 목적으로 사용이 됩니다.

JWT 기반 인증은 JWT 토큰 (access token)을 HTTP 헤더에 실어 서버가 클라이언트를 식별하는 방식을 의미합니다.

**JWT의 구성요소**

1. Header: 토큰의 종류와 암호화 방법을 포함한다.
2. Payload: 토큰에 들어가는 정보(데이터)로, 여러 개의 claim으로 구성된다. 이 claim들은 사용자와 관련된 정보를 담고 있다.
3. Signature: 토큰을 인증하고 유효성을 확인하는 데 쓰이는 숫자 서명.

JWT는 JSON 데이터를 Base64 URL - safe Encode 방식을 통하여 인코딩해 직렬화 한 것으로 토큰 내부엔 위변조 방지를 위해 개인키를 통한 전자 서명도 포함이 되어져 있습니다.

물론 토큰도 중간에 가로채질 수 있습니다. 그래서 피해를 최소화하고자 2가지 종류의 토큰을 발급합니다.

1. **Access Token :** 출입증으로 사용할 토큰
2. **Refresh Token :** Access Token이 만료되었을 때 인증 상태를 연장해 줄 토큰

보통 보안을 위해 **Access Token**의 유효기간을 짧게 설정하고, **Refresh Token**의 유효기간을 길게 설정을 해둡니다. **Access Token**의 유효기간이 끝나면 **Refresh
Token**을 통하여 새로운 **Access Token**을 재발급 받습니다. **Refresh Token**은 만료시에만 노출이 되기 때문에 해킹의 위험이 적습니다.

<img src="AccessToken 로직.png" alt="AccessToken 로직"/>

## 토큰 전달하기

일반적으로 토큰은 **요청 헤더의 Authorization 필드**에 담아져 보내집니다.

서버는 **다양한 토큰**에 따라 다르게 처리를 하기위해서 전송받은 **type**으로 토큰을 다르게 처리합니다.

**토큰 전달받기**

```JavaScript
const login = async (email, password) => {
    try {
        const res = await axiosSign.post('/login', { email, password }, {
            withCredentials: true 
        })
        localStorage.setItem('accessToken', res.data.data.token)
    } catch (err) {
        console.log(err)
    }
}
```

로그인 로직에 성공하였을때 전달받은 data.token 파일을 localStorage에 accessToken 이라는 이름으로 저장을 하였습니다.

```JavaScript
export const axiosTodo = axios.create({
    baseURL: process.env.REACT_APP_BACKEND_URL + `/todo`,
    headers: {
        Authorization: `Bearer ${localStorage.getItem('accessToken')}`
		//                   타입                   토큰
    },
    withCredentials: true
})
```

이후 Axios를 이용한 통신을 할 때 마다 localStorage에서 accessToken을 가져와 headers 안에 Authorization으로 넣어주는것을 볼 수 있습니다.

**인증타입**

**Basic** : 사용자의 아이디와 암호를 Base64로 인코딩한 값을 토큰으로 사용합니다. 헤더에 인증 정보가 포함되어 전송됩니다.

**Bearer** : 주로 JWT나 OAuth에서 사용하는 토큰 방식입니다. 헤더에 "Bearer" 키워드와 함께 토큰이 포함되어 전송됩니다.

**Digest** : 서버에서 난수 데이터 문자열(nonce)을 클라이언트에 보내고, 클라이언트는 사용자 정보와 nonce를 이용하여 해시값을 계산하여 응답합니다. 이를 서버와 비교하여 인증을 수행합니다.

**HOBA** : 전자 서명 기반 인증으로, 클라이언트와 서버 간에 공개키 및 개인키를 사용하여 데이터를 암호화하고 복호화하며 인증을 수행합니다.

**Mutual** : 클라이언트와 서버가 상호 인증하는 방식으로, 각각 암호를 사용하여 인증 정보를 교환합니다. 현재 Mutual 인증은 IETF draft 상태입니다.

**AWS4-HMAC-SHA256** : AWS의 전자 서명 기반 인증 방식으로, AWS 서비스로의 요청에 대해 인증하려면 클라이언트와 서버 모두 SHA-256 HMAC 암호화 방식을 사용하여 요청에 서명해야
합니다.

이러한 인증 방식은 웹 애플리케이션과 서버 간 통신에 사용되며, 데이터의 보안 및 사용자 인증을 위해 설계되었습니다. 애플리케이션의 요구 사항, 보안 수준 및 인프라에 따라 적절한 인증 방식을 선택할 수 있습니다.

## 로직 구현하기

**로그인 로직 구현하기**

```JavaScript
const { axiosInstance } = require("./@core")
const PATH = '/user'

const AuthAPi = {
    login(email, password) {
        return axiosInstance.post(PATH + '/login', { email, password })
    },

    signUp(email, password) {
        return axiosInstance.post(PATH + '/signIn', { email, password })
    },

    logout() {
        return axiosInstance.post(PATH + '/logout')
    }

}

export default AuthAPi
```

**Axios 헤더에 토큰 전달**

```JavaScript
export const axiosInstance = axios.create({
    baseURL: process.env.REACT_APP_BACKEND_URL,
    headers: {
        Authorization: `Bearer ${TokenRepository.getToken()}`
    },
		timeout: 500s
    withCredentials: true
})
```

**Axios 전역 값 설정**

```JavaScript
// 프론트엔드가 백엔드에 요청을 보내기 전에 가로채는 것, 주로 access_token

// 백엔드에 요청을 보내기 전 로직을 작성합니다.
axiosInstance.interceptors.request.use(
  (config) => {
    const access_token = TokenRepository.getToken()
    // 요청을 보내기전 매번 토큰 불러온 것
    if (access_token) {
        config.headers.Authorization = `Bearer ${access_token}`
        // 헤더에 토큰을 실은 것
    }
    return config
    // 다시 요청 그대로 전송
  }
)

// 프론트엔드가 응답을 받기 전에 응답을 가로채는 것, 주로 refresh
axiosInstance.interceptors.response.use(
  (res) => {
      return res
      // 성공했을 때 가로챘을 때 로직이 있다면 구현
  },
  async (err) => {
    if (err.response.status === 401) {
        await AuthApi.logout()
        TokenRepository.removeToken()
    }
    const originalRequest = err.config;
    if (err.response.status === 403 && !originalRequest._retry) {
        originalRequest._retry = true;

        const res = await axiosInstance.post('/user/jwt');
        // 토큰 재발급 요청
        if (res.status === 200) {
            //성공
            const token = res.data.data
            // 응답 데이터 -> 토큰

            TokenRepository.setToken(token)
            // 토큰 웹 스토리지 재설정

            axiosInstance.defaults.headers.common['Authorization'] = `Bearer ${token}`
            // 헤더에 토큰 재설정

            return axiosInstance(originalRequest)
            // 기존 요청 재전송
        }
    }
  }
)
```

**로그인 로직**

```JavaScript
useEffect(() => {
        if (isLogin) {
            navigation('/todo/3')
        }
    }, [isLogin])

const onPressSignIn = async (e) => {
    e.preventDefault();
    const email = e.target.email.value;
    const password = e.target.password.value;

    try {
        const res = await AuthAPi.login(email, password)
        TokenRepository.setToken(res.data.data.token)
        setIsLogin(true)
        navigation('/todo/3')
    } catch (err) {
        console.log(err)
    }
};
```

**로그아웃 로직**

```JavaScript
const onClick = () => {
        TokenRepository.removeToken()
        setLoginState(false)
        navigation('/')
    }

return (
    <div>HEADER <button onClick={onClick}>로그아웃</button></div>
)
```

**데이터 통신 방법 정리.txt**

1. 데이터를 요청할려면
    1. 백엔드와 데이터 통신을 할려면, 누가 보냈는지, 주문 테이블이 어디인지, 뭐로 주문하는지 (ex : 토큰 or 세션)  알려주어야 합니다.
2. 데이터 주문 방법
    1. 인증 토큰의 경우 JWT를 사용하며 서버의 부담 없이 토큰을 주고받을 수 있습니다.
    2. session의 경우 서버에 부담이 크다는 대신, 인증 토큰보다 보안에 더 안정된 상황에서 데이터를 주고받을 수 있습니다.
3. 발급 방법
    1. 세션의 경우 로그인 시 백엔드에서 세션 ID를 보통 쿠키를 통해 프론트엔드에 전달합니다.
    2. 인증 토큰의 경우 데이터에 토큰과 재발급이 가능한 refresh 토큰을 전달합니다.
4. 각자 발급 받은 인증 데이터로 실행하는 방법
    1. 세션의 경우
        1. 세션의 경우 로그인 시 백엔드에서 세션 ID를 보통 쿠키를 통해 프론트엔드에 전달합니다. 인증 토큰의 경우 응답 데이터에 토큰과 이를 재발급할 수 있는 refresh 토큰을 전달합니다.
        2. 인증 토큰은 인증이 필요한 api 요청에 항상 header에 실어서 보내야하며
           토큰을 보내는 방법에는 2가지 방법이 존재합니다.
    2. 토큰의 경우
        - axios 인스턴스를 생성할 때 기본 값으로 토큰을 설정하며 Bearer는 해당 토큰이 JWT 토큰이라는 것을 알려주는 일종의 분류이므로 앞에 붙여 사용
        - axios의 인터셉터를 사용하여 백엔드에 요청을 보내기 전에 데이터를 가로채 헤더에 토큰을 심어서 전송
5. 토큰의 인증이 만료가 된경우
    - axios의 인터셉터를 사용하여 프론트엔드가 응답을 받기 전에 데이터를 가로채서 백엔드에 jwt 재발급 요청을 보내고 다시 토큰을 실어서 재요청을 합니다.
    - 모든 것은 사용자가 데이터를 받기 전에 이루어지므로, 사용자는 리프레시 토큰이 있다면 토큰이 만료된 것을 모르고 사이트를 이용할 수 있습니다.