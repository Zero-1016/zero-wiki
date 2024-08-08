# 컴포넌트와 props

## 컴포넌트에 대해 알아보기

**리액트는 컴포넌트 기반 (Component -Based)**의 구조라는 중요한 특징이 있다는 것을 간략하게 배웠습니다. 리액트는 모든 페이지가 컴포넌트로 구성되어 있고, 하나의 컴포넌트는 또 다른 여러 개의 컴포넌트의 조합으로 구성될 수 있습니다.

<img src="리액트 컴포넌트.png" alt="리액트 컴포넌트"/>

다음과 같이 A로 표시된 부분과 B로 표시된 부분이 리액트 컴포넌트라고 할 수 있습니다. A와 B 둘 다 똑같이 여러 번 반복적으로 사용해서 하나의 페이지를  구성하고 있습니다. 이처럼 우리가 리액트 **컴포넌트 기반**이라고 부르는것은 작은 A, B 컴포넌트 들이 모여 위 이미지 처럼 하나의 컴포넌트를 구성하고 이 컴포넌트들이 모여서 전체 페이지를 구성하기 때문입니다.

**컴포넌트의 장점**

하나의 컴포넌트를 반복적으로 사용함으로써 전체 코드의 양이 줄어 자연스레 개발 시간과 유지보수 비용도 줄일 수 있습니다. 마치 자바스크립트의 함수와 비슷하죠

함수가 입력을 받아서 출력을 내뱉는 것처럼 리액트 컴포넌트도 똑같습니다. 하지만 약간 비슷하지 같지 않습니다.

<img src="함수형 컴포넌트 생성 과정.png" alt="함수형 컴포넌트 생성 과정"/>

리액트의 입력은 바로 props라는 것이고 출력은 앞에서 배운 리액트 엘리먼트가 됩니다. 즉 리액트 컴포넌트가 해주는 역할은 **속성들을 입력 받아서 그에 맞는 리액트 엘리먼트를 생성하여 리턴해주는 것**입니다.

<img src="붕어빵.png" alt="붕어빵"/>

마치 붕어빵 기계에 붕어 모양 틀에 반죽을 부어 붕어빵을 만들듯이 리액트 컴포넌트로부터 엘리먼트가 만들어지는 과정역시 비슷합니다. 컴포넌트는 붕어빵 틀을 붕어빵들은 리액트 엘리먼트를 의미한다고 볼 수 있습니다.

이는 소프트웨어 공학에서 나오는 객체 지향의 클래스와 인스턴스의 개념과 비슷합니다.

## Props에 대해 알아보기

**Props의 개념**

prop은 property라는 영어 단어를 줄여서 쓴 것입니다. 이는 ‘속성’, ‘특성’이라는 뜻을 갖고있는데 리액트에서는 **속성**이라는 뜻으로 사용합니다. 그렇다면 무엇의 속성일까요? 바로 **리액트 컴포넌트의 속성**입니다.

<img src="붕어빵 props.png" alt="붕어빵 props"/>

하나의 붕어빵 기계를 사용한다해도 다 똑같지 않습니다. 안에 무엇이 들어가냐에 따라서 결과가 다르기 때문이죠. 이처럼 안에 들어가는 재료를 prop 이것들이 여러개가 들어가면 props라고 하는 것입니다. 이처럼 props는 같은 리액트 컴포넌트에서 눈에 보이는 글자나 색깔 등의 속성을 바꾸고 싶을 때 사용하는 컴포넌트의 속 재료라고 생각하면 됩니다.

이처럼 결과물 즉 컴포넌트의 모습과 속성을 결정하는 것이 바로 props입니다. props는 **컴포넌트에 전달할 다양한 정보를 담고 있는 자바스크립트 객체**입니다. 컴포넌트에 어떤 데이터를 전달하고 전달된 데이터에 따라 다른 모습의 엘리먼트를 화면에 렌더링하고 싶을 때, 해당 데이터를 props에 넣어 전달하는 것이죠

**Props의** **특징**

Props의 중요한 특징은 읽기 전용(Read-Only)라는 것입니다. 읽기만 가능하다 즉 값을 변경할 수 없습니다. Props는 엘리먼트를 생성하기 위해서 사용이 됩니다. 하지만 도중에 재료가 바뀌어 버리면 제대로 된 엘리먼트가 생성될 수 없겠죠?

**그렇다면 다른 props의 값으로 엘리먼트를 생성하려면 어떻게 해야할까요?**

새로운 값(Props)를 컴포넌트에 전달하여 새로 엘리먼트를 생성하면 됩니다. 이 과정에서 엘리먼트가 다시 렌더링 되는 것이겠죠.

**pure 함수**

```JavaScript
// pure 함수
// input을 변경하지 않으면 같은 input에 대해서 항상 같은 output을 리턴
function sum (a, b){
	return a + b;
}
```

위 sum 함수에서 파라미터 값이 변경되지 않을 경우에는 항상 같은 값을 리턴할 것입니다. 이러한 함수를 Pure하다라고 표현합니다. 이 뜻은 함수가 순수하다는 뜻인데 이 말은 **입력값을 변경하지 않으며, 같은 입력값에 대해서는 항상 같은 출력값을 낸다**는 의미입니다.

**impure 함수**

```JavaScript
// impure 함수
// input을 변경함
function withdraw(account, amount){
	account,total -= amount;
}
```

위 withdraw 함수는 파라미터를 받아 total이라는 값에서 amount를 빼는 함수입니다. 이 함수는 입력으로 받은 파라미터 account의 값을 변경했습니다. 이런 경우 Imputer하다 라고 합니다. 순수하지 않나는 것이죠

리액트 공식사이트에서는 컴포넌트의 특징에 대해서 아래와 같이 기술했습니다.

**All React components must act like pure functions with respect to their props**

모든 리액트 컴포넌트는 그들의 props에 관해서는 Pure 함수 같은 역할을 해야한다.

즉 **모든 리액트 컴포넌트는 props를 직접 바꿀 수 없고, 같은 props에 대해서는 항상 같은 결과를 보여줄 것!** 이라는 뜻이 됩니다. 앞서 말했던 것처럼 리액트 컴포넌트는 자바스크립트의 함수와 같은 개념이라고 했었습니다. 함수가 Pure하다는 것은 입력값인 파라미터를 바꿀 수 없다는 의미를 포함하고 있기에 **리액트 컴포넌트에서는 props를 바꿀 수 없다.** 라는 의미가 됩니다. 그리고 Pure 함수의 경우 항상 같은 결과를 보여줘야 하기 때문에 **같은 props에 대해서는 항상 같은 결과를 보여줘야 한다.** 라는 의미가 됩니다.

정리하자면 **리액트 컴포넌트의 props는 바꿀 수 없고, 같은 props가 들어오면 항상 같은 엘리먼트를 리턴해야 합니다.**

**Props 사용법**

```JavaScript
function App(props){
    return(
        <Profile
            name = '소플'
            introduction= '안녕하세요 소플입니다.'
            viewCount ={1500}
        />
    );
}
```

위 코드에서는 App 컴포넌트가 나오고, 그 안에서는 Profile 컴포넌트를 사용하고 있습니다. 여기에서 Profile 컴포넌트에는 name. introduction, viewCount라는 세 가지 속성을 넣어 주었습니다. 이렇게 하면 이속성의 값이 Profile 컴포넌트에 props로 전달되며 자바스크립트 객체 형태가 됩니다.

```JavaScript
{
    name : "소플",
    introduction : "안녕하세요, 소플입니다.",
    viewCount : 1500
}
```

한가지 눈여겨 봐야할 부분은 각 속성에 값을 넣을때 중괄호를 사용한 것과 사용하지 않은것의 차이입니다. 앞에서 JSX 에대해 배울 때 **중괄호를 사용하면 무조건 자바스크립트 코드가 들어간다.** 라고 배웠습니다 마찬가지로 props에 값을 넣을 때에도 중괄호를 사용해서 감싸주어야 합니다. 따라서 중괄호를 활용하면 아래와 같이 넣을 수 있습니다.

```JavaScript
function App(props){
    return(
        <Layout
            width = {2560}
            height = {1440}
            header = {
                <Header title = "소플의 블로그입니다." />
            }
            footer = {
                <Footer />
            }
        />
    );
}
```

## 컴포넌트 만들기

**컴포넌트의 종류**

컴포넌트는 클래스 컴포넌트와 함수 컴포넌트로 나뉩니다. 리액트 초기버전에서는 클래스 컴포넌트를 주로 사용하였지만, 불편하다는 의견이 많이 나왔고 이후 함수 컴포넌트를 개선해서 사용하게 되었습니다. 개선하는 과정에서 개발된 것이 바로 훅(Hook)입니다.

**함수 컴포넌트**

앞서 모든 리액트 컴포넌트는 Pure 함수의 역할을 해야 한다고 했습니다. 이 말은 결국 **리액트의 컴포넌트를 일종의 함수** 라고 생각한다는 뜻입니다.

```JavaScript
function Welcome(props){
	return <h1>안녕, {props.name}</h1>;
}
```

이 코드에는 Welcome 이라는 이름을 가진 함수가 하나 나옵니다. 이 함수의 경우 하나의 props 객체를 받아서 인사말이 담긴 리액트 엘리먼트를 리턴하기에 리액트 컴포넌트라고 할 수 있습니다. 그리고 이렇게 생긴 것을 **함수 컴포넌트**라고 합니다.

**클래스 컴포넌트**

클래스 컴포넌트는 자바스크립트 ES6의 클래스를 이용해서 만들어진 형태의 컴포넌트입니다. 함수 컴포넌트에 비해 몇 가지 추가적인 기능이 있습니다.

```JavaScript
class Welcome extends React.Component{
	render() {
		return <h1>안녕, {this.props.name}</h1>
	}
}
```

위 함수 컴포넌트 Welcome과 동일한 형태의 클래스 컴포넌트 입니다. 가장 큰 차이점은 리액트의 모든 클래스 컴포넌트는 React.Component를 상속 받아서 만든다는 것입니다.

**컴포넌트 이름 짓기**

컴포넌트의 이름을 지을 때 한  가지 유의해야 할 중요한 것이 있스빈다. 바로 컴포넌트의 이름은 **항상 대문자로 시작해야 된다**는 것입니다. 왜나하면 리액트는 소문자로 싲가하는 컴포넌트를 DOM태그로 인식하기 때문에 항상 대문자로 작성해야 합니다.

**컴포넌트 렌더링**

앞서 말했던것 처럼 컴포넌트는 붕어빵 틀의 역할을 합니다 그렇기 때문에 실제로 컴포넌트가 화면에 렌더링되는 것은 아닙니다. 틀을 통해 나온 엘리먼트들이 화면에 보이겠죠? 그렇다면 렌더링을 하기 위해서는 엘리먼트들을 먼저 만들어줄 필요가 있습니다.

```JavaScript
function Welcom(props){
	return <h1>안녕, {props.name}</h1>;
}

const element = <Welcome name ="인제"/>;
ReactDOM.render(
	element,
	document.getElementById('root')
);
```

위 코드에서는 먼저 Welcome이라는 함수 컴포넌트를 선언하고 있습니다. 그리고 `<Welcome name =”인제” />` 라는 값을 가진 엘리먼트를 파라미터로 해서 ReactDOM.render( ) 를 호출 합니다. 이렇게 하면 Welcome 컴포넌트에{ name: “인제” } 라는 props를 넣어서 호출하고 결과로 리액트 엘리먼트가 생성됩니다. 생성된 엘리먼트는 React DOM을 통해 실제 DOM에 업데이트가 되고 우리가 브라우저를 통해서 볼 수 있습니다.

**컴포넌트 합성**

컴포넌트 합성은 **여러 개의 컴포넌트를 합쳐서 하나의 컴포넌트를 만드는 것 입니다.** 리액트 에서는 컴포넌트 안에 또 다른 컴포넌트를 사용할 수 있기 때문에, 복잡한 화면을 여러 개의 컴포넌트로 나눠서 구현할 수 있습니다.

```JavaScript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App(props) {
  return (
    <div>
      <Welcome name = "Mike" />
      <Welcome name = "Steve" />
      <Welcome name = "Jane" />
    </div>
  );
}

ReactDOM.render(
    <App/>,
    document.getElementById('root')
);
```

위의 코드를 보면 props의 값을 다르게 해서 Welcome 컴포넌트를 여러번 사용하는 것을 볼 수 있습니다. 이렇게 여러 개의 컴포넌트를 합쳐서 또 다른 컴포넌트를 만드는 것을 **컴포넌트 합성**이라고 합니다.

<img src="합성 컴포넌트.png" alt="합성 컴포넌트"/>

App 컴포넌트 안에 세 개의 Welcome 컴포넌트가 있고, 각각의 Welcome 컴포넌트는 각기 다른 props를 가지고 있습니다. 이렇게 App 컴포넌트를 root로 해서 하위 컴포넌트들이 존재하는 형태가 리액트로만 구성된 앱의 기본적인 구조입니다.

**컴포넌트 추출**

컴포넌트 합성과 반대로 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 나눌 수도 있습니다. 이러한 과정을 **컴포넌트 추출**이라고 부릅니다. **큰 컴포넌트에서 일부를 추출해 새로운 컴포넌트를 만든다**는 뜻이죠, 이 추출을 잘 활용하게 되면 컴포넌트의 재사용성이 올라가게 됩니다. 왜냐하면 컴포넌트의 기능과 목적이 명확해지고, props도 단순해지기 때문에 다른 곳에서 사용할 수 있는 확률이 높아지기 때문입니다.

```JavaScript
function Comment(props) {
  return (
    <div className="comment">
      <div className="user-info">
        <img className="avatar" 
            src={props.author.avatarUrl} 
            alt={props.author.name} 
        />
        <div className="user-info-name">
            {props.author.name}
        </div>
      </div>
      
      <div className="comment-text">
        {props.text}
      </div>

      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

여기에 Comment라는 컴포넌트가 하나 있습니다. 여기서 컴포넌트를 하나씩 추출해보겠습니다.

```JavaScript
function Avartar(props){
    return(
        <img className="avatar" 
        src={props.user.avatarUrl} 
        alt={props.user.name} 
		    />  
    );
}
```

추출된 Avatar 컴포넌트는 이런 모습이 될 것입니다. 그리고 props에 기존에 사용하던 author 대신 조금더 보편적인 의미를 갖고 있는 user를 사용하였습니다. 보편적인 어를 사용하는 것은 재사용성 측면을 고려하는 것이라고 보면 됩니다.

```JavaScript
function Comment(props) {
  return (
    <div className="comment">
      <div className="user-info">
        <Avartar user={props.autor}/>
        <div className="user-info-name">
            {props.author.name}
        </div>
      </div>
      
      <div className="comment-text">
        {props.text}
      </div>

      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

추출된 Avatar 컴포넌트가 적용된 모습입니다.

```JavaScript
function UserInfo(props){
    return(
        <div className="user-info">
        <Avartar user={props.user}/>
        <div className="user-info-name">
            {props.user.name}
        </div>
      </div>
    );
}
```

이번엔 사용자의 정보를 담고 있던 부분인 UserInfo라는 컴포넌트를 추출한 것입니다. 아까 추출했던 Avatar 컴포넌트도 여기에 함께 추출된 것을 볼 수 있습니다.

```JavaScript
function Comment(props) {
  return (
    <div className="comment">
      <UserInfo user={props.author}/>
      <div className="comment-text">
        {props.text}
      </div>

      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

UserInfo 컴포넌트를 반영하면 이렇게 됩니다. 코드가 처음에 비해 훨씬 더 단순해졌습니다.

<img src="추출 컴포넌트.png" alt="추출 컴포넌트"/>

추출된 컴포넌트 구조

Comment 컴포넌트가 UserInfo 컴포넌트를 포함하고 있고, 그안에 Avatar 컴포넌트를 포함하고 있는 구종비니다. 지금까지 추출한 것 이외에도 추가적으로 Comment, 글과 작성일 등 추출이 가능합니다. 컴포넌트를 어느 정도 수준까지 추출하는 것이 좋은지에 대해 정해진 기준은 없습니다. 하지만 **기능 단위로 구분하는 것이 좋고, 나중에 곧바로 재사용이 가능한 형태로 추출하는 것이 좋습니다.**