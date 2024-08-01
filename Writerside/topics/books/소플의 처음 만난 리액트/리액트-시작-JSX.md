# 리액트 시작 JSX

## 리액트

리액트는 사용자 인터페이스를 만들기 위한 자바스크립트 라이브러리입니다.

### 라이브러리가 무엇인가요?

라이브러리는 직역하면 도서관이란 의미로, 프로그래밍에서는 자주 사용되는 기능을 모아 놓은 것을 의미합니다. 도서관처럼 특정 프로그래밍 언어에서 자주 사용되는 기능들을 잘 모아서 정리해 놓은 모음집이라고 보면 됩니다.

### 사용자 인터페이스를 만든다는 것은 무엇인가요?

사용자 인터페이스(User Interface, 줄여서 UI)는 사용자와 컴퓨터 프로그램이 상호작용하기 위해 중간에서 입력과 출력을 제어해주는 것을 의미합니다. UI를 만들기 위한 기능 모음집(라이브러리)을 UI
라이브러리라고 하며, 리액트는 대표적인 자바스크립트 UI 라이브러리입니다.

## 다양한 자바스크립트 UI 프레임워크

- 앵귤러JS: 5~6년 전까지 가장 많이 사용된 웹 개발 프레임워크입니다. 2018년 LTS 모드에 돌입하였고, 2022년 1월에 LTS 지원이 중단되었습니다. 공식적인 지원이 종료된 상태입니다.
- 뷰JS: 중국인 개발자 Evan You가 시작한 오픈소스 프로젝트로, 2014년 출시 이후 점점 영향력이 커져 리액트와 함께 항상 언급되는 대표 프레임워크입니다.
- 리액트JS: 메타에서 만든 오픈소스 자바스크립트 UI 라이브러리로, 2013년 출시 이후 가장 인기가 많은 자바스크립트 UI 라이브러리입니다.

### 프레임워크와 라이브러리의 차이점은 무엇인가요?

앵귤러JS와 뷰JS는 프레임워크라고 부르고, 리액트는 라이브러리라고 합니다. 프레임워크는 흐름의 제어 권한을 프레임워크가 갖고 있는 반면, 라이브러리는 흐름에 대한 제어를 하지 않고 개발자가 필요한 부분만 가져다
사용하는 형태입니다. 즉, 라이브러리는 제어 권한이 개발자에게 있으며, 프레임워크는 제어 권한이 프레임워크 자신에게 있다고 이해하면 됩니다.

## 리액트의 개념 정리

리액트는 사용자와 웹사이트의 상호작용을 돕는 인터페이스를 만들기 위한 자바스크립트 라이브러리입니다. 복잡한 사이트를 쉽고 빠르게 만들고 관리하기 위해 만들어졌습니다.

### SPA (Single Page Application)

SPA는 하나의 HTML 틀만 만들어 놓고, 사용자가 특정 페이지를 요청할 때 해당 페이지의 내용을 채워서 보내주는 웹사이트를 의미합니다. 리액트는 SPA를 쉽고 빠르게 만들 수 있도록 도와줍니다.

### 리액트의 장점

빠른 업데이트와 렌더링 속도

리액트는 Virtual DOM(가상 돔)을 사용하여 상태의 변경(State Change) 시 업데이트해야 할 최소한의 부분만 검색하고 렌더링하여 빠르게 내용을 보여줍니다.
컴포넌트 기반 구조

리액트에서는 모든 페이지가 컴포넌트로 구성되어 있으며, 하나의 컴포넌트는 여러 개의 컴포넌트의 조합으로 구성됩니다. 작은 레고 블록들이 모여 하나의 완성된 모형이 되는 것과 유사합니다.
재사용성

재사용성은 개발 기간을 단축시키고, 유지 보수를 용이하게 합니다. 재사용성이 높은 코드는 버그의 원인을 찾기 쉽고, 여러 모듈 간의 의존성이 낮아 잘 분리된 상태를 유지할 수 있습니다.
든든한 지원군

메타(구 페이스북)의 지원으로 프로젝트가 성장하고 꾸준히 유지될 수 있습니다. 많은 개발자들이 지속적으로 업데이트하며, 몇 년 동안 그 영향력이 지속될 것입니다.
활발한 지식 공유 & 커뮤니티

리액트는 GitHub과 StackOverflow에서 많은 관심을 받고 있습니다. 이는 기술의 생태계 규모를 판단하는 중요한 지표입니다.
모바일 앱 개발 가능

리액트를 배운 후 리액트 네이티브(React Native)를 사용하여 모바일 앱도 개발할 수 있습니다. 자바스크립트 코딩을 통해 안드로이드와 iOS 앱을 동시에 출시할 수 있습니다.

### 리액트의 단점

방대한 학습량

리액트는 다른 방식의 UI 라이브러리이기 때문에 배워야 할 것이 많습니다. 지속적인 버전 업데이트로 새로운 내용을 학습하고 숙지해야 합니다.
높은 상태 관리 복잡도

리액트의 중요한 개념인 상태 관리(state)는 프로젝트 규모가 커질수록 복잡도가 증가합니다. 이를 위해 Redux, MobX, Recoil 등의 외부 상태 관리 라이브러리를 사용하는 경우가 많습니다.

## JSX(JavaScriptXML)

### JavaScriptXML?

JSX는 자바스크립트의 확장 문법으로, JavaScript와 XML/HTML을 합친 것입니다.

```Javascript
const element = <h1>Hello, world</h1>;
```

이 코드는 HTML과 JavaScript 코드가 결합된 JSX 코드로, `<h1>` 태그로 둘러싸인 문자열을 element 변수에 저장합니다.

### 역할 알아보기

JSX는 내부적으로 XML/HTML 코드를 자바스크립트로 변환합니다. 이를 리액트의 createElement() 함수가 수행합니다.

```Javascript
class Hello extends React.Component {
    render() {
        return <div>Hello {this.props.toWhat}</div>;
    }
}

ReactDOM.render(
    <Hello toWhat="World" />,
    document.getElementById('root')
);
```

이 코드는 Hello라는 이름의 리액트 컴포넌트를 정의하고, render() 함수를 통해 화면에 렌더링합니다.

```Javascript
class Hello extends React.Component {
    render() {
        return React.createElement('div', null, `Hello ${this.props.toWhat}`);
    }
}

ReactDOM.render(
    React.createElement(Hello, { toWhat: 'World' }, null),
    document.getElementById('root')
);
```

위 코드는 순수 자바스크립트를 사용해 동일한 역할을 합니다. JSX를 사용하면 내부적으로 createElement() 함수로 변환됩니다.

```Javascript
const element = (
    <h1 className="greeting">
        Hello, world!
    </h1>
);

const element = React.createElement(
    'h1',
    { className: 'greeting' },
    'Hello, world!'
);
```

두 코드는 동일한 역할을 합니다. JSX를 사용하면 내부적으로 createElement() 함수를 사용하여 자바스크립트 객체를 생성합니다.

### JSX의 장점

1. 코드가 간결해진다.

```Javascript
// JSX 사용함
<div>Hello, {name}</div>

// JSX를 사용 안함
React.createElement('div', null, `Hello, ${name}`);
```

2. 가독성이 향상된다.

JSX를 사용하면 코드의 의미가 더욱 빠르게 와닿습니다. 이는 유지 보수 관점에서도 중요합니다.

3. 보안성이 올라간다.

JSX는 Injection Attack을 방어할 수 있습니다. ReactDOM은 렌더링하기 전에 임베딩된 값을 문자열로 반환하기 때문에 잠재적인 보안 위협을 줄일 수 있습니다.

### 사용법

**JSX에서 JavaScript 변수를 참조하는 방법**

```Javascript
const name = '소플';
const element = <h1>안녕, {name}</h1>;

ReactDOM.render(
    element,
    document.getElementById('root')
);
```

**JSX에서 JavaScript 함수를 활용하는 방법**

```Javascript
function formatName(user) {
    return user.firstName + ' ' + user.lastName;
}

const user = {
    firstName: 'Inje',
    lastName: 'Lee'
};

const element = (
    <h1>
        Hello, {formatName(user)}
    </h1>
);

ReactDOM.render(
    element,
    document.getElementById('root')
);
```

**JSX에서 특정 조건에 만족하면 실행하는 분기점을 나누는 방법**

```Javascript
function getGreeting(user) {
    if (user) {
        return <h1>Hello, {formatName(user)}</h1>;
    }
    return <h1>Hello, Stranger.</h1>;
}
```

**HTML 태그 중간이 아닌 태그 속성에 값을 넣는 방법**

```Javascript
// 큰따옴표 사이에 문자열을 넣음
const element = <div tabIndex="0"></div>;

// 중괄호 사이에 자바스크립트 코드를 넣음
const element = <img src={user.avatarUrl}></img>;
```

**JSX 문법에서 자식 요소를 정의하는 방법**

```Javascript
const element = (
    <div>
        <h1>안녕하세요</h1>
        <h2>열심히 리액트 공부해 봅시다!</h2>
    </div>
);
```

위의 코드처럼 상위 태그가 하위 태그를 둘러싸면 자식 요소들이 정의됩니다. 이는 `<h1>`, `<h2>` 태그가 `<div>` 태그의 자식 태그가 되는 것을 한눈에 볼 수 있어 가독성이 높고 간결하며 직관적입니다.