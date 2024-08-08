# state와 생명주기 훅

## 상태

state는 상태 라는 뜻을 가지고 있습니다. 리액트에서의 state는 **리액트 컴포넌트의 상태**를 의미합니다. 다만 상태라는 단어가 정상인지 비정상인지를 나타내는 것이라기 보다 리액트 컴포넌트의 데이터라는 의미에 더 가깝습니다. 쉽게 말하자면 **리액트 컴포넌트의 변경 가능한 데이터**를 state라고 부릅니다. 이 state는 리액트 컴포넌트를 개발하는 각 개발자가 직접 정의해서 사용하게 됩니다.

state를 정의할 때 중요한 점은 꼭 **렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 한다**는 것입니다. 왜냐하면 state가 변경될 경우 컴포넌트가 재렌더링되기 떄문에 데이터 흐름에 관련 없는 값을 포함하면 해당 컴포넌트가 다시 렌더링되어 성능을 저하 시킬 수 있습니다.

그래서 관련 있는 값만 state에 포함하도록 해야 하며, 그렇지 않은 값은 컴포넌트 인스턴스의 필드로 정의하면 됩니다.

## State의 특징

state는 따로 복잡한 형태가 있는게 아니라 하나의 **자바스크립트 객체**입니다.

```Javascript
class LikeButton extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            liked: false
        };
    }
    ... 
}
```

이 코드는 LikeButton 이라는 리액트 클래스 컴포넌트를 나타낸 것입니다. 모든 클래스 컴포넌트에는 constructor 라는 이름의 생성자 함수가 존재하는데 바로 클래스가 생성될 때 실행되는 함수입니다. 이 생성자 코드를 보면 this.state라는 부분이 나오는데 이 부분이 바로 현재 컴포넌트의 state를 정의하는 부분입니다. 클래스 컴포넌트의 경우 state를 생성자에서 정의하고 함수 컴포넌트의 경우 state를 useState( )라는 훅을 사용해서 정의합니다.

이렇게 정의한 state는 정의된 이후 일반적인 자바스크립트 변수를 다루듯이 직접 수정할 수는 없습니다.

```Javascript
// state를 직접 수정 (잘못된 사용법)
this.state = {
    name : 'Inje'
};

// setState 함수를 통한 수정 (정상적인 사용법)
this.setState({
    name: 'Inje'
});
```

위의 코드의 첫 번째 방법은 state를 직접 수정하는 방법이고, 두 번째는 setState( ) 함수를 통해 수정하는 방법입니다. state를 직접 수정할 수는 있지만 리액트는 인지하지 못할 수 있기 때문에 **state는 직접적인 변경이 불가능하다**고 생각하는 것이 좋습니다.

앞서 말했듯이 state는 컴포넌트의 렌더링과 관련 있기 때문에 마음대로 수정하게 되면 개발자가 의도한 대로 작동하지 않을 가능성이 있습니다. **그래서 state를 변경하고자 할 때에는 꼭 setState( ) 라는 함수를 사용해야 합니다.**

## 생명주기에 대해 알아보기

리액트 컴포넌트의 생명주기에 대해 알아보겠습니다. LifeCycle 즉 생명주기. 사람들은 출생을 하고 노화가 오게 되어 사망하게 됩니다. 리액트 컴포넌트도 생명주기를 갖고 있습니다.

<img src="리액트의 생명주기.png" alt="리액트의 생명주기"/>

위 그림을 보면 크게 출생, 인생, 사망으로 나누어져 있습니다. 각 과정의 하단에 초록색으로 표시도니 부분은 생명주기에 따라 호출되는 클래스 컴포넌트의 함수입니다. 이 함수들을 **Lifecycle method** 라고 부르며 번역하면 **생명주기 함수**가 됩니다. 이제 하나씩 살펴보겠습니다.

먼저 컴포넌트가 **생성**되는 시점, 사람으로 말하면 **출생**입니다. 이 과정을 **마운트**라고 부릅니다.

이때 컴포넌트의 constructor (생성자)가 실행됩니다. 앞에서 본 것처럼 생성자에서는 컴포넌트의 state를 정의하게 됩니다. 또한 컴포너느가 렌더링 되며 이후에 componentDidMount( ) 함수가 호출이 됩니다.

태어난 이후 각자의 인생을 사는것처럼 신체적, 정신적으로 변화를 겪습니다. 리액트 컴포넌트도 마찬가지입니다. 이러한 **과정**, 즉 **변화**를 겪으면서 여러 번 렌더링 됩니다. 이를 리액트 컴포넌트로 말하면 **업데이트**과정이라고 할 수 있습니다.

업데이트 과정에서는 컴포넌트의 props가 변경되거나 setState( ) 함수 호출에 의해 state가 변경되거나, forceUpdate( ) 라는 강제 업데이트 함수 호출로 인해 컴포넌트가 다시 렌더링 됩니다. 그리고 렌더링 이후 ComponentDidUpdate( ) 함수가 호출이 됩니다.

마지막으로 **사망**과정이 있습니다. 리액트 컴포넌트에서도 해당 과정을 겪습니다. 이 과정을 **언마운트**라고 부릅니다. 그렇다면 컴포넌트는 언제 언마운트가 될까요? 바로 **상위 컴포넌트에서 현재 컴포넌트를 더 이상 화면에 표시하지 않게 될 때 언마운트된다.**라고 볼 수 있습니다. 이 때 언마운트 직전에 component**Will**Unmount ( ) 함수가 호출됩니다.

세 가지 생명주기 함수 이외에도 다른 생명주기 함수가 존재하지만 지금은 클래스 컴포넌트를 거의 사용하지 않기 때문에 다루지 않았습니다. 컴포넌트 생명주기에서 기억해야 할 부분은 **컴포넌트가 계속 존재하는 것이 아니라 시간의 흐름에 따라 생성되고 업데이트되다가 사라진다는 것**입니다. 이것은 비단 클래스 컴포넌트 뿐만 아니라, 함수 컴포넌트에도 해당되는 부분입니다.

# 훅

## 훅이란 무엇인가?

원래 기존 함수 컴포넌트는 별도로 state를 정의해서 사용하거나 컴포넌트의 생명주기에 맞춰 어떤 코드가 실행되도록 할 수 없었습니다. 따라서 함수 컴포넌트에 이런 기능을 지원하기 위해서 나온 것이 바로 훅입니다.

훅을 사용하면 함수 컴포넌트도 클래스 컴포넌트의 기능을 모두 동일하게 구현할 수 있게 되는 것이죠.

**Hook이란?**

영단어는 갈고리라는 뜻을 갖고 있습니다, 프로그래밍에서는 **원래 존재하는 어떤 기능에 마치 갈고리를 거는 것 처럼 끼어 들어가 같이 수행되는 것**을 의미합니다. 리액트 훅도 마찬가지로 **리액트의 state와 생명주기 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행되도록 만든 것 입니다,** 그리고 이때 실행되는 함수를 훅이라고 부르기로 정했죠.

이런 훅의 이름은 모두 use로 시작합니다. 수행하는 기능에 따라 이름을 짓게 되었는데 각 기능을 사용하겠다는 의미로 use를 앞에 붙였습니다. 개발자가 직접 커스텀 훅을 만들어서 사용할 수 도있는데 규칙에 따라 앞에 use를 붙여서 훅이라는 것을 나타내주어야 합니다.

## useState

useState( )는 이름에서 알 수 있듯이 state를 위한 훅입니다. 함수 컴포넌트에서는 기본적으로 state를 사용하지 않기 때문에 useState( ) 훅을 사용해야합니다.

```Javascript
import React, { useState } from "react";

function Counter(props) {
  var count = 0;

  return (
    <div>
      <p>
        {count}번 클릭했습니다.
        <button onClick={() => count++}>클릭</button>
      </p>
    </div>
  );
}
```

Counter 컴포넌트는 버튼을 클릭하면 카운트를 하나씩 증가시키고 현재 카운트를 보여주는 단순한 컴포넌트입니다. 그러나 위처럼 카운트를 함수의 변수로 선언해서 사용하게 되면 버튼을 클릭시 카운트 값을 증가시킬 수는 있지만, 재렌더링이 일어나지 않아 새로운 카운트 값을 표기할 수 없게 됩니다. 이런경우 state를 사용해서 값이 바귈때마다 재랜더링이 되도록 해야합니다. 하지만 함수 컴포넌트에는 해당 기능이 따로 없기 때문에 usetState( )를 사용하여 state( )를 선언하고 업데이트 해야 합니다.

useState( )는 아래와 같이 사용합니다.

```Javascript
const [변수명, set함수명] = useState(초깃값);
```

useState( ) 를 호출할 때에는 파라미터로 선언할 state의 초기값이 들어갑니다. 클래스 컴포넌트 생성자에서 state를 선언할 때 초깃값을 넣어 주는 것과 동일한 것이라고 보면 됩니다. useState 함수를 호출하면 리턴 값으로 배열이 나옵니다. 리턴된 배열에는 2 가지 항목이 들어가 있는데 첫 번째 항목은 state로 선언된 변수이고 두 번째 항목은 해당 state의 set 함수입니다. useState ( )를 사용하는 코드를 보도록 합시다.

```Javascript
import React, { useState } from "react";

function Counter(props) {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>총 {count}번 클릭했습니다.</p>
      <button onClick={() => setCount(count + 1)}>클릭</button>
    </div>
  );
}
```

위의 코드는 useState( )를 사용하여 카운트 값을 state로 관리하도록 만든 것입니다. 이 코드에서는 state의 변수명과 set 함수가 각각 count,  setCount로 되어있는 것을 볼 수 있습니다. 버튼이 눌렸을 때 setCount( ) 함수를 호출해서 카운트를 1 증가시킵니다. 그리고 count의 값이 변경되면 컴포넌트가 재렌더링되면서 새로운 카운트 값이 표시됩니다.

이 과정은 클래스 컴포넌트에서 setState( ) 함수를 호출해서 state가 업데이트되고 이후 컴포넌트가 재렌더링되는 과정과 동일하다고 보면 됩니다. 다만 클래스 컴포넌트에서는 setStatus( ) 함수 하나를 사용해서 모든 state 값을 업데이트할 수 있었지만 useState( ) 를 사용하는 방법에서는 변수 각각에 대해 set 함수가 따로 존재한다는 것을 의미합니다.

## useEffect

useEffect( )는 사이드 이펙트를 수행하기 위한 훅입니다. 그러면 사이드 이펙트가 뭘까요? 사이드 이펙트는 사전적으로 **부작용**이라는 의미를 내포하고 있습니다. 개발자가 의도하지 않은 코드가 실행되면서 버그가 나타나면 사이드 이펙트가 발생했다고 말합니다. 하지만 리액트에서의 사이드 이펙트는 부정적인 의미가 아닙니다.

리액트에서의 사이드 이펙트란 효과 혹은 영향을 뜻하는 이펙트의 의미에 더 가깝습니다. 예를 들면 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 등의 작업을 의미합니다. 이런 작업을 이펙트라고 부르는 이유는 다른 컴포넌트에 영향을 미칠 수 있으며 렌더링 중에는 작업이 완료될 수 없기 때문입니다.

**useEffect( )**는 리액트 함수 컴포넌트 사이에서 사이드 이펙트를 실행할 수 있도록 해주는 훅입니다. **useEffect( )**는 클래스 컴포넌트에서 제공하는 생명주기 함수인 componentDidMount( ), componentDidUpdate( ), 그리고 componentWillUnmount( )와 동일한 기능을 하나로 통합해서 제공합니다. 그래서 useEffect( ) 훅 하나만으로 생명주기 함수와 동일한 기능을 수행할 수 있습니다.

```Javascript
useEffect(이펙트 함수, 의존성 배열);
```

첫 번째 파라미터로는 이펙트 함수가, 두 번째 파라미터로는 의존성 배열이 들어갑니다 의존성 배열은 말 그대로 이 이펙트가 의존하고 있는 배열인데 **배열 안에 있는 변수 중에 하나라도 값이 변경되었을 때** 이펙트 함수가 실행됩니다.

만약 이펙트가 마운트와 언마운트 (처음과 끝)에만 실행되게 하고 싶으면, 의존성 배열 옆에 빈 배열([ ])을 넣으면 됩니다. 그러면 이펙트가 props나 state에 있는 어떤 값에도 의존하지 않는 것이 되므로 여러번 실행되지 않습니다. 의존성 배열은 생략할 수도 있는데 생략하게되면 컴포넌트가 업데이트될 때마다 호출됩니다.

```Javascript
import React, { useEffect, useState } from "react";

function Counter(props) {
  const [count, setCount] = useState(0);

  // componentDidMount, componentDidUpdate와 비슷하게 작동합니다.
  useEffect(() => {
    // 브라우저 API를 사용해서 document의 title을 업데이트 합니다.
    document.title = `총 ${count}번 클릭했습니다.`;
  });

  return (
    <div>
      <p>총 {count}번 클릭했습니다.</p>
      <button onClick={() => setCount(count + 1)}>클릭</button>
    </div>
  );
}
```

위 코드는 useState( )를 배울 때 살펴본 코드와 거의 동일하며 추가로 useEffect( ) 훅을 사용하고 있습니다. useEffect( )를 사용해서 클래스 컴포넌트에서 제공하는 componentDidMount ( ), componentDidUpdate( ) 안에 있는 이펙트 함수에서는 브라우저에서 제공하는 API를 사용해서 document의 title을 업데이트 합니다.

위 코드 처럼 의존성 배열 없이 useEffect( )를 사용하면 리액트는 **DOM이 변경된 이후에 해당 이펙트 함수를 실행하라**는 의미로 받아들입니다. 그래서 기본적으로 컴포넌트가 처음 렌더링 될 때를 포함해서 매번 렌더링될 때마다 이펙트가 실행된다고 보면 됩니다.

또한, 이펙트는 함수 컴포넌트 안에서 선언되기 때문에 해당 컴포넌트의 props와 state에 접근할 수도 있습니다. 위 코드에서는 count라는 state에 접근하여 해당 값이 포함된 문자열을 생성해서 사용하는 것을 볼 수 있습니다.

```Javascript
import React, { useEffect, useState } from 'react';

export default function test() {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status){
    setIsOnline(status.isOnline);
  }

  useEffect(()=>{
    ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
    return () => {
      ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange)
    };
  });

  if (isOnline === null){
    return '대기중 ...';
  }
  return isOnline ? '온라인' : '오프라인';

}
```

위 코드는 useEffect( ) 에서 먼저 ServerAPI를 사용하여 사용자의 상태를 구독하고 있습니다. 이후 함수를 하나 리턴하는데 해당 함수 안에는 구독을 해지하는 API를 호출하도록 되어있습니다. **useEffect( )에서 리턴하는 함수는 컴포넌트가 마운트 해제될 때 호출됩니다.** 결과적으로 useEffect( )의 리턴 함수의 역할을 componentWillUnmount ( ) 함수가 하는 역할과 동일합니다.

useEffect( ) 훅은 하나의 컴포넌트에 여러 개를 사용할 수 있습니다.

```Javascript
import React, { useEffect, useState } from "react";

export default function test() {
  const [isOnline, setIsOnline] = useState(0);

  useEffect(() => {
    document.title = `총 ${count}번 클릭했습니다.`;
  })

  useEffect(() => {
    ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
    return () => {
      ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  ...
}
```

```Javascript
useEffect(()=>{
  // 컴포넌트가 마운트 된 이후,
  // 의존성 배열에 있는 변수들 중 하나라도 값이 변경되었을 때 실행됨
  // 의존성 배열에 빈 배열([])을 넣으면 마운트와 언마운트시에 단 한 번씩만 실행됨
  // 의존성 배열 생략시 컴포넌트 업데이트 시마다 실행됨
  ...

  return ()=>{
    // 컴포넌트가 마운트 해지되기 전에 실행됨
    ...
  }
}, [의존성 변수1, 의존성 변수2, ...]
)
```

## useMemo

useMemo( )훅은 Memoized value를 리턴하는 훅입니다. 파라미터로 Memoized value을 생성하는 create 함수와 의존성 배열을 받습니다. **의존성 배열에 들어있는 변수가 변했을 경우에만 새로 create 함수를 호출하여 결괏값을 반환하며, 그렇지 않은 경우에는 기존 함수의 결괏값을 그대로 반환합니다**. useMemo( ) 훅을 사용하면 컴포넌트가 다시 렌더링될 때마다 연산량이 높은 작업을 피할 수 있습니다.

요약하자면 배열안에 있는 변수가 변했을 경우에만 함수를 호출하여 결괏값을 반환하고 아닐경우 기존의 함수값을 재사용한다. (연산을 하지 않는다.)

```Javascript
const memoizedValue = useMemo(
    () => {
      // 연산량이 높은 작업을 수행하여 결과를 반환
      return computeExpensiveValue(의존성 변수1, 의존성 변수2);
    },
    [의존성 변수1, 의존성 변수2]
  );
```

useMemo( )로 전달된 함수는 **렌더링이 일어나는 동안 실행된다는 점**입니다. 그렇기 때문에 일어나는 동안 실행돼서는 안될 작업을 **useMemo( )**의 함수에 넣으면 안됩니다. 예를 들면 useEffect( ) 훅에서 실행돼야 할 사이드 이펙트 같은 것이 있습니다. 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 작업 등은 렌더링이 일어나는 동안 실행돼서는 안되기 때문에 useMemo( ) 훅의 함수에 넣으면 안 되고 useEffect( ) 훅을 사용해야 합니다.

useMemo( ) 훅도 의존성 배열을 넣지 않을 경우 렌더링이 일어날 때마다 매번 함수가 실행됩니다. 즉 의존성 배열을 넣지 않는 것은 아무런 의미가 없습니다. 만약 빈 배열을 넣게되면 컴포넌트 마운트 시에만 함수가 실행이 됩니다.

```Javascript
const MemoizedValue = useMemo(
	() => computeExpensiveValue(a, b)
);
```

**eslint-plugin-react-hooks 패키지**

userMemo( ) 에서 의존성 배열에 넣은 변수들은 create 함수의 파라미터로 전달되지 않습니다. 하지만 원래의 의미가 의존성 배열의 변수중 하나라도 변하면 create 함수를 다시 호출하기 때문에 create 함수에서 참조하는 모든 변수를 의존성 배열에 넣어주는 것이 맞습니다. ??

## useCallback

useCallback( )훅은 이전에 나온 useMemo( )훅과 유사한 역할을 합니다. 한 가지 차이점은 **값이 아닌 함수를 반환한다는 점입니다.** useCallback( ) 훅은 useMemo( ) 훅과 마찬가지로 함수와 의존성 배열을 파라미터로 받습니다. useCallback( ) 훅에서 파라미터로 받는 이 함수를 콜백이라고 부릅니다. 그리고 의존성 배열에 있는 변수 중 하나라도 변경되면 콜백 함수를 반환합니다.

```
const memoizedValue = useCallback(
    () => {
      doSomething(의존성 변수1, 의존성 변수2);
    },
    [의존성 변수1, 의존성 변수2]
  );
```

의존성 배열에 변화에 따라 Memoized 값을 반환한다는 점에서 useMemo( )훅과 완전히 동일합니다. 그래서 useCallback(function, dependencies)은 useMemo(( ) ⇒ function, dependencies)와 동일하다고 볼 수 있습니다.

만약 useCallback( ) 훅을 사용하지 않고 컴포넌트 내에 함수를 정의한다면 매번 렌더링이 일어날 때마다 함수가 새로 정의됩니다. 따라서 useCallback( ) 훅을 사용하여 특정 변수의 값이 변한 경우에만 함수를 다시 정의하도록 해서 불필요한 반복작업을 없애주는 것입니다.

## 메모제이션

useMemo( )와 useCallback( ) 훅에서는 **메모제이션**이라는 개념이 나옵니다. 컴퓨터 분야에서 메모이제이션은 최적화를 위해서 사용하는 개념입니다. 비용이 높은(연산량이 많은) 함수의 호출결과를 기억을 해두었다가 같은 입력값으로 함수를 호출하면 새로 함수를 호출하지 않고 이전에 저장해놨던 호출 결과를 바로 반환 하는 것입니다. 이렇게 할 경우 불필요한 연산을 하지않고 바로 반환을하여 호출 결과를 받기까지의 시간도 짧아지겠죠.

## useRef

useRef( ) 훅은 래퍼런스를 사용하기 위한 훅입니다. 리액트에서 래퍼런스란 **특정 컴포넌트에 접근할 수 있는 객체**를 의미합니다.  useRef( ) 훅은 바로 래퍼런스 객체를 반환합니다.

래퍼런스 객체에는 .current라는 속성이 있는데 이것은 현재 레퍼런스(참조)하고 있는 엘리먼트를 의미한다고 보면 됩니다.

```Javascript
const refContainer = useRef(초기값);
```

위와 같이 useRef( ) 훅을 사용하면 파라미터로 들어온 초깃값으로 초기화된 레퍼런스 객체를 반환합니다. 만약 초깃값이 null이라면 .current의 값이 null인 래퍼런스 객체가 반환되겠죠? 이렇게 반환된 래퍼런스 객체는 컴포넌트의 라이프타임 전체에 걸쳐서 유지됩니다. 마운트가 해제되기 전까지요.

쉽게 말해 useRef( )훅은 **변경 가능한 .current라는 속성을 가진 하나의 상자**라고 생각하면 됩니다.

```Javascript
function TextInputWithFocusButton(props) {
    const inputElem = useRef(null);

    const onButtonClick = () => {
      // `current`는 마운트된 input element를 가리킵니다.
      inputElem.current.focus();
    };

    return (
      <>
        <input ref={inputElem} type="text" />
        <button onClick={onButtonClick}>Focus the input</button>
      </>
    );
  }
```

래퍼런스와 관련하여 DOM에 접근하기 위해 사용하는 ref속성에 익숙할 수도 있습니다. 비슷하게 리액트에서는 <div ref={myRef} />라는 코드를 작성하면 node가 변경될 때 마다 myRef의 .current 속성에 현재 해당되는 DOM node를 저장합니다.

ref 속성과 기능은 비슷하지만 useRef( ) 훅은 클래스의 인스턴스 필드를 사용하는 것과 유사하게 다양한 변수를 저장할 수 있다는 장점이 있습니다. 이렇게 가능한 이유는 useRef( ) 훅이 일반적인 자바스크립트 객체를 리턴하기 때문입니다.

그럼 내가 직접 .current 속성이 포함된 자바스크립트 객체를 만들어 써도 되는 것 아닌가? 라고 생각할 수도 있습니다. 물론 그렇게 해도 목적 달성은 가능할 수 있지만 useRef( ) 훅을 사용하는 것과 직접 { current: … }모양의 객체를 만들어 사용하는 것의 차이점으로 useRef( ) 훅은 **매번 렌더링될 때마다 항상 같은 ref 객체를 반환한다는 것**입니다.

한 가지 기억해야 할 점은 useREF( ) 훅은 내부의 데이터가 변경되었을 때 별도로 알리지 않는다는 점입니다. current 속성을 변경하는 것은 재렌더링을 일으키지 않습니다. 따라서 ref에 DOM node 가 연결(attach)되거나 분리(detach)되었을 경우 어떤 코드를 실행하고 싶다면 callback ref를 사용해야 합니다.

**DOM node의 변화를 어떻게 알 수 있을까요? (callback ref 사용하기)**

가장 기초적인 방법으로는 callback ref를 사용하는 것이 있습니다. 리액트는 ref가 다른 node에 연결될 때마다 콜백을 호출하게 됩니다. 아래 예제 코드를 봅시다.

```Javascript
import React, { useCallback, useState } from "react";

export default function test() {
  function MeasureExample(props) {
    const [height, setHeight] = useState(0);

    const measuredRef = useCallback((node) => {
      if (node !== null) {
        setHeight(node.getBoundingClientRect().height);
      }
    }, []);

    return (<>
      <h1 ref={measuredRef}>안녕, 리액트</h1>
      <h2>위 헤더의 높이는 {Math.round(height)}px입니다.</h2>
    </>);
  }
}
/**
 * 위 코드에는 레퍼런스를 위해서 useRef() 훅을 사용하지 않고 useCallback()훅을 이용한
 * callback ref 방식을 사용했습니다. useRef()훅을 사용하게 되면 래퍼런스 객체가 .current
 * 속성이 변경되었는지를 따로 알려주지 않기 때문이죠. 하지만 callback ref 방식을 사용하게 되면
 * 자식 컴포넌트가 변경 되었을 때 알림을 받을 수 있고, 이를 통해 다른 정보들을 업데이트 할 수
 * 있습니다. 이 예제 코드에서는 <h1> 태그의 높이 값을 매번 업데이트하고있습니다. 그리고 
 * useCallback () 훅의 의존성 배열로 비어있는 배열을 넣었는데, 이렇게 하면 <h1> 태그가
 * 마운트, 언마운트될 때만 콜백 함수가 호출되며 재렌더링이 일어날 때에는 호출되지 않습니다.
 */
```

## 훅의 규칙

훅은 단순한 자바스크립트 함수이지만 두 가지 지켜야 할 규칙이 있습니다.

첫 번째 규칙은 **훅은 무조건 최상위 레벨 에서만 호출해야 합니다.** 여기에서 말하는 최상위 레벨은 리액트 함수 내에서의 최상위 레벨로 **반복문이나 조건문 안 같은 중첩된 함수들 안에서는 훅을 호출하면 안됩니다.** 이 규칙에 따라 훅은 컴포넌트가 **렌더링 될 때마다 매번 같은 순서로 호출됭야 합니다.** 그렇게 해야 useState( ), useEffect( )훅의 호출에서 state를 올바르게 관리할 수 있게 됩니다.

```
import React, { useEffect, useState } from 'react';

export default function ReactHook() {
    const [name, setName] = useState('Inje')

    if(name!= ''){
        useEffect(()=>{
            ...
        });
    }
    return (
        <div>
            
        </div>
    );
}
```



위 코드를 본다면 name ≠ ‘ ‘ 이라는 조건이 있습니다. 해당 값이 참인 경우에만 useEffect( )가 호출하도록 되어있습니다. 하지만 만약 중간에 name의 값이 빈 문자열이 되면 useEffect( )훅이 호출되지 않습니다.

결과적으로 렌더링할때 순서대로 호출되는 것이 아니라 조건문에 따라 호출되므로 잘못된 코드입니다.

두 번째 규칙은 **리액트 함수 컴포넌트에서만 훅을 호출해야 한다**는 것입니다. 그렇기 때문에 일반적인 자바스크립트 함수에서 훅을 호출하면 안됩니다. **훅은 리액트 함수 컴포넌트에서 호출하거나 직접 만든 커스텀 훅에서만 호출할 수있습니다.**

## 나만의 훅 만들기

리액트에선 기본적으로 제공되는 훅들 이외에 추가적으로 필요한 기능이 있다면 직접 훅을 만들어서 사용할 수 있습니다. 이것을  **커스텀 훅**이라고 부릅니다. 커스텀 훅을 만드는는 이유는 반복적으로 사용되는 로직을 훅으로 만들어 재사용하기 위함입니다.

**커스텀 훅을 만들어야 하는 상황**

아래 UserStatus라는 컴포넌트는 isOnline이라는 state에 따라서 사용자의 상태가 온라인인지 아닌지를 텍스트로 보여주는 컴포넌트 입니다.

```Javascript
import React, { useEffect, useState } from "react";

export default function ReactHook(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
    return () => {
      ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return "대기중...";
  }
  return isOnline ? "온라인" : "오프라인";
}
```

동일한 웹사이트에서 연락처 목록을제공하는데 이때 온라인인 사용자의 이름은 초록색으로 표시해주는 UsrListItem이 있습니다. 여기에는 위와 비슷한 로직을 넣어야 합니다.

```Javascript
import React, { useEffect, useState } from "react";

export default function ReactHook(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
    return () => {
      ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
    };
  });

  return <li style={{ color: isOnline ? "green" : "black" }}>{props.user.name}</li>;
}
```

보면 UserStatus의 useState( ) , useEffect( ) 훅을 사용하는 부분이 동일한 것을 볼 수 있습니다. 중복되는 코드인 것을 확인할 수 있습니다. 기존에는 render props 또는 HOC를 사용합니다. 하지만 이번에는 중복되는 코드를 추출하여 커스텀 훅으로 만드는 새로운 방식을 사용해 보겠습니다.

**커스텀 훅 추출하기**

기존의 자바스크립트 에서는 두 개의 함수에 하나의 로직을 공유하도록 하고 싶을 때 새로운 함수 하나를 만드는 방법을 사용합니다.

리액트 컴포넌트와 훅은 모두 함수이기 때문에 동일한 방법을 사용할 수 있습니다.

커스텀 훅은 특별한 것이 아니라 **이름이 use로 시작하고 내부에서 다른 훅을 호출하는 하나의 자바스크립트 함수**입니다.

아래 코드는 중복되는 로직을 useUserStatus( )라는 커스텀 훅으로 추출해낸 것입니다.

```Javascript
import React, { useEffect, useState } from "react";

export default function useUserStatus(userId) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ServerAPI.subscribeUserStatus(userId, handleStatusChange);
    return () => {
      ServerAPI.unsubscribeUserStatus(userId, handleStatusChange);
    };
  });

  return isOnline;
}
```

앞의 코드를 보면 특별할 것이 없이 두 개의 컴포넌트에서 중복되는 로직을 추출햇 가져왓습니다. 다만 다른 컴포넌트 내부에서와 마찬가지로 다른 훅을 호출하는 것은 무조건 커스텀 훅의 최상위 레벨어사만 해야합니다.

리액트 컴포넌트와 달리 커스텀 훅은 특별한 규칙이 없습니다. 파라미터로 무엇을 받을지, 어떤 것을 리턴해야 할지를 개발자가 직접 정할  수 있습니다. 다시 말하면 커스텀 훅은 그냥 단순한 함수와도 같습니다. 하지만 이름은 use를 사용함으로서 이것이 단순한 함수가 아닌 리액트 훅이라는 것을 나타내주는 것입니다.

위에서의 useUserStatus( ) 훅의 목적은 사용자의 온라인/오프라인 상태를 구독하는 것입니다.그렇기 때문에 아래 코드처럼 useUserStatus ( ) 훅의 파라미터로 userID를 받도록 만들었고 해당 사용자가 온라인인지 오프린인지 상태를 리턴하게 하였습니다.

```
export default function useUserStatus(userId) {
  const [isOnline, setIsOnline] = useState(null);
    ...
  return isOnline;
}
```

**커스텀 훅 사용하기**

처음 커스텀 훅을 만들기로 했을 때의 목표는 UserStatus와 UserListItem 컴포넌트로부터 중복된 로직을 제거하는 것이었습니다. 그리고 두개의 컴포넌트는 모두 사용자의 상태를 알기 원합니다.

이제 중복되는 로직을 useUserStatus( ) 훅으로 추출했기 때문에 이것을 사용하여 코드를 변경할 수 있습니다.

```Javascript
function useUserStatus(userId) {
  const [isOnline, setIsOnline] = useState(props.user.id);

  if (isOnline === null) {
    return "대기중...";
  }
  return isOnline ? "온라인" : "오프라인";
}

function UserListItem(props) {
  const isOnline = useUserStatus(props.user.id);

  return <li style={{ color: isOnline ? "green" : "black" }}>
					  {props.user.name}
				 </li>;
}
```

위의 코드는 커스텀 훅을 적용하기 전과 동일하게 작동합니다. 동작에[ 변경이 없고 중복되는 로직만을 추출하여 커스텀 훅으로 만든 것이기 때문이죠. **커스텀 훅은 리액트 기능이 아닌 훅의 디자인에서도 자연스럽게 따르는 규칙입니다.**

커스텀 훅의 이름은 꼭 use로 시작해야 할까요? 네, 그렇습니다 이것은 중요한 규칙이기 때문에 꼭 지켜야 합니다. **만약 이름이 use로 시작하지 않는다면 특정 함수의 내부에서 훅을 호출하는지를 알 수 없기 때문에 훅의 규칙 위반 여부를 자동으로 확인할 수 없습니다.**

그렇다면 같은 커스텀 훅을 사용하는 두 개의 컴포넌트는 state를 공유하는 것일까요?

아닙니다. 커스텀 훅은 단순히 state와 연관된 로직을 재사용이 가능하게 만든 것입니다. 따라서 여러 개의 컴포넌트에서 하나의 커스텀 훅을 사용할 때에 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있습니다.

그렇다면 커스텀 훅은 어떻게 state를 분리하는 것일까요? 각각의 커스텀 훅 호출에 대해서 분리된 state를 얻게 됩니다. 위의 예제 코드에서 useUserStatus( ) 훅을 직접 호출하는 것처럼 리액트 관점에서는 컴포넌트에서 useState ( )와 useEffect ( ) 훅을 호출하는 것과 동일 한 것입니다. 또한 하나의 컴포넌트에서 useState ( )와 useEffect ( ) 훅을 여러 번 호출 할 수 있는 것처럼 각 커스텀 훅의 호출 또한 완전히 독립적이라고 불 수 있습니다.

**훅들 사이에서 데이터를 공유하는 방법**

훅을 호춤함에 있어 각 호출은 완전히 독립히 독립적이라고 배웠습니다. 그렇다면 훅들 사이에서 데이터를 공유하고 싶다면 어떻게 해야할까요?

```Javascript
const userList = [
  { id: 1, name: "Inje" },
  { id: 2, name: "Mike" },
  { id: 3, name: "Steve" },
];

function ChatUserSelector(props) {
	const [userId, setUserId] = useState(1);
  const isUserOnline = useUserStatus(userId)
  return (
    <>
      <Circle color={isUserOnline ? "green" : "red"} />
      <select value={userId} onChange={(event) => setUserId(Number(event.target.value))}>
        {userList.map((user) => (
          <option key={user.id} value={user.id}>
            {user.name}
          </option>
        ))}
      </select>
    </>
  );
}
```

위 코드에서는 ChatUserSelector 라는 컴포넌트가 나옵니다. 사용자가 선택할 경우 해당 사용자가 온라인인지 아닌지 보여주게 됩니다.

```Javascript
import { useState } from "react";

const [userId, setUserId] = useState(1);
const isUserOnLine = useUserStatus(userId)
```