# Asynchronous Javascript

자바스크립트는 대부분의 작업을 **외부 API**에 의존하여 처리하며, 결과는 **콜백** 형태로 전달받습니다.

예를 들어, 서버와의 통신, DOM 이벤트 처리(버튼 클릭 등) 등은 모두 비동기적으로 수행됩니다.

## 자바스크립트의 동작 원리

1. 싱글 스레드 언어

- 자바스크립트는 단일 스레드에서 실행됩니다.
- 동기적 작업은 콜 스택에서 처리되며, 비동기적 작업은 이벤트 루프를 통해 관리됩니다.

2. 이벤트 루프(Event Loop)

- 이벤트 루프는 코드 실행, 이벤트 처리, 다음 작업 스케줄링을 담당합니다.
- 이벤트 큐에 있는 작업을 콜 스택에 전달하여 실행 흐름을 제어합니다.

3. UI 업데이트 및 사용자 이벤트 처리

- 자바스크립트는 동일한 스레드에서 UI 업데이트와 사용자 이벤트를 처리합니다.

## 이벤트 루프와 자바스크립트의 구성 요소

자바스크립트의 동작은 메모리 힙과 콜 스택을 기반으로 합니다.

![image.png](자바스크립트_비동기처리.png)

- 메모리 힙 (Memory Heap)

동적으로 메모리를 할당하는 영역입니다.
불필요한 메모리를 반환하지 않을 경우 **메모리 누수**가 발생할 수 있습니다.
(최신 자바스크립트 엔진은 이를 방지하기 위해 **가비지 컬렉터**를 통해 자동으로 반환합니다.)

- 콜 스택 (Call Stack)

함수 호출 시 **스택 프레임** 단위로 쌓이며, 함수 실행이 완료되면 스택에서 제거됩니다.
동기적으로 실행되므로 한 번에 하나의 함수만 실행됩니다.

- 이벤트 큐 (Event Queue)

비동기 작업(예: 타이머, 네트워크 요청)이 완료된 후 콜백 함수를 대기시키는 영역입니다.
큐에 쌓인 작업은 **FIFO(First In, First Out)** 방식으로 처리됩니다.

- 이벤트 루프 (Event Loop)

콜 스택이 비어 있을 때, 이벤트 큐에서 대기 중인 작업을 가져와 실행합니다.

## 콜백 지옥 (Callback Hell)

비동기 작업을 중첩된 콜백 형태로 처리할 경우 코드가 복잡해지고 가독성이 떨어집니다. 이를 **콜백 지옥**이라고 부릅니다.

콜백 지옥을 해결하기 위해 자바스크립트는 **Promise**와 **async/await**를 제공합니다.

## Promise

Promise는 비동기 작업의 결과를 관리하기 위한 객체로, 세 가지 상태를 가집니다.

1. 대기(Pending): 작업이 아직 완료되지 않은 상태.
2. 이행(Resolved): 작업이 성공적으로 완료된 상태.
3. 거부(Rejected): 작업이 실패한 상태.

#### 주요 특징

- 성공 시: `.then()`
- 실패 시: `.catch()`
- 여러 Promise를 병렬 또는 순차적으로 처리하기 위해 `.all()`, `.race()` 등의 메서드를 제공합니다.

#### 예제

```JavaScript
const fetchData = new Promise((resolve, reject) => {
    const success = true;
    if (success) {
        resolve("Data fetched successfully!");
    } else {
        reject("Error fetching data.");
    }
});

fetchData
    .then((data) => console.log(data)) // 성공: "Data fetched successfully!"
    .catch((error) => console.error(error)); // 실패: "Error fetching data."

```

### Async/Await

`async/await`는 비동기 작업을 동기 코드처럼 작성할 수 있도록 도와줍니다.

`async` 함수는 항상 `Promise`를 반환합니다.

성공: `return`
실패: `throw`
`await` 키워드는 비동기 작업이 완료될 때까지 실행을 멈추고 결과를 반환받습니다.

#### 예제

```JavaScript
async function fetchData() {
    try {
        const data = await getDataFromServer();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

fetchData();
```

## 요약

- 비동기 작업: 자바스크립트는 싱글 스레드 환경에서 이벤트 루프를 통해 비동기 작업을 처리합니다.
- 콜백 지옥: 중첩된 콜백 구조는 복잡성을 증가시키므로 Promise나 async/await를 사용해 가독성을 높이는 것이 좋습니다.
- Promise: 상태 기반으로 비동기 작업을 관리.
- Async/Await: 비동기 코드를 동기 코드처럼 작성 가능.