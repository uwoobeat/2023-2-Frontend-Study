## About `index.html`

### 뷰포트

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

`<meta>` 태그는 문서의 메타데이터를 나타낸다. `name` 속성은 메타데이터의 이름을 나타내고, `content` 속성은 메타데이터의 값을 나타낸다.
`viewport`는 뷰포트의 크기를 나타내는 메타데이터이다. 뷰포트는 브라우저에서 실제로 내용이 표시되는 영역을 말한다.
`width=device-width`는 뷰포트의 너비를 디바이스의 너비와 같게 설정한다는 의미이다.
`initial-scale=1.0`은 페이지가 처음 로드될 때 초기 확대/축소 비율을 1.0으로 설정한다는 의미이다.

뷰포트는 웹페이지에서 보이는 영역을 말한다. 반응형 웹 디자인에서 주로 사용된다. 모바일 환경에서는 디스플레이의 크기가 작기 때문에 뷰포트의 크기를 조절해야 한다. 만약 지정하지 않을 경우, 뷰포트의 크기는 디바이스의 크기와 같아진다. 그러면 모바일 환경에서도 데스크톱 환경과 같은 크기의 화면을 보게 된다. 이러면 컨텐츠가 데스크톱과 모바일 환경의 비율만큼 작아지기 때문에 가독성이 저해된다.

단 뷰포트를 사용하면 미디어 쿼리를 사용하여 반응형으로 만들어야 한다. 미디어 쿼리를 사용하는 것은 번거로우므로 일단은 뷰포트를 사용하지 않고 만들어보자.


### div class vs id

### form vs div

### 인라인 이벤트 핸들러 vs `addEventListener`

### `<script>` 태그 위치

https://shape-coding.tistory.com/entry/JavaScript-Script-%ED%83%9C%EA%B7%B8%EC%9D%98-%EC%9C%84%EC%B9%98%EB%8A%94-%EC%96%B4%EB%94%94%EA%B0%80-%EC%A2%8B%EC%9D%84%EA%B9%8C

`</body>` 태그 바로 위에 위치시키는 것이 좋다. HTML 파싱이 끝나고 DOM 트리가 완성된 후에 스크립트를 다운로드 및 실행하기 때문에 사용자가 화면의 내용을 빨리 볼 수 있다.

### 엔터 키를 눌렀을 때 task가 추가되도록 하기

버튼을 누르는 것은 꽤나 번거로운 일이다. 그래서 할 일을 작성하고 엔터 키를 누르면 자동으로 추가되도록 하고 싶었다.
보통 엔터 키 관련 이벤트는 이런 식으로 처리한다.

```js
document.addEventListener('keydown', function(event) {
  if (event.keyCode === 13) {
    // 엔터 키를 눌렀을 때 실행할 코드
  }
});
```

하지만 keyCode는 deprecated 되었고, `event.key`를 사용하라고 한다.

```js
document.addEventListener('keydown', function(event) {
  if (event.key === 'Enter') {
    // 엔터 키를 눌렀을 때 실행할 코드
  }
});
```

실제로 구현한 코드를 보자.

```js
document.getElementById('newTask').addEventListener('keyup', function(event) {
    if (event.key == 'Enter') {
        event.preventDefault();
        addTodo();
    }
});
```

`addTaskBtn` 과 다르게 입력창에서 엔터를 누를 때 (엄밀하게는 키가 올라갈 때) 발생하므로 `newTask`에 이벤트를 걸어준다.
그리고 `event.preventDefault()`를 통해 기본 동작을 막아준다.

또 기존에는 input과 button 태그를 가지는 것이 div 태그였다.

```html
<div class="block">
    <input type="text" id="newTask" placeholder="Enter your to-do here!">
    <button id="addTaskBtn">+</button>
</div>
```

div보다는 form 태그를 사용하는 것이 의미론적으로 더 적절하다.
하지만 각 div 블록은 하나의 블록 단위 역할을 한다. 그래서 이걸 form으로 바꿀 순 없다.
내부에 각각 존재하는 input, button을 form 태그로 한번 더 감싸줬다.

```html
<form>
    <input type="text" id="newTask" placeholder="Enter your to-do here!">
    <button id="addTaskBtn">+</button>
</form>
```

하지만 이렇게 하면 투두를 입력하다가 엔터를 치면 화면 깜빡임이 발생한다.
로그를 보면 input 태그에서 엔터 키를 누르면 form 태그의 submit 이벤트가 발생한다.
이때 화면이 깜빡이는 것은 submit 이벤트가 발생하면서 페이지가 새로고침 되기 때문이다.
다양한 해결방법이 있지만 onSubmit 이벤트를 사용하면 쉽게 해결할 수 있다.

```html
<form onsubmit="return false;">
    <input type="text" id="newTask" placeholder="Enter your to-do here!">
    <button id="addTaskBtn">+</button>
</form>
```

## About `script.js`

다음과 같이 구현했다. 각 코드에 대한 간단한 설명이다.

### `DOMContentLoaded` 이벤트 리스너

```js
document.addEventListener("DOMContentLoaded", function() {
    loadFromLocalStorage();

    document.getElementById('addTaskBtn').addEventListener('click', addTodo);
});
```

#### `DOMContentLoaded` vs `window.onload`

바닐라 JS에서 페이지 시작 시 DOM에 이벤트 리스너를 추가할 때 보통 두 가지 방식을 사용한다. 하나는 `window.onload`를 사용하는 것이고, 다른 하나는 `DOMContentLoaded`를 사용하는 것이다. 두 방식의 차이점은 다음과 같다.

- `window.onload`는 페이지의 모든 리소스가 로드된 후에 실행된다. 즉, 이미지, 스크립트, 스타일시트 등의 리소스가 모두 로드된 후에 실행된다.
- `DOMContentLoaded`는 DOM 트리가 완성된 후에 실행된다. 즉, HTML 파싱이 끝나고 DOM 트리가 완성된 후에 실행된다.

보통은 `DOMContentLoaded`를 사용하는 것이 좋다. 왜냐하면 `window.onload`는 모든 리소스가 로드된 후에 실행되기 때문에 페이지가 느리게 로드되는 경우 사용자가 페이지를 볼 수 있는 시간이 늦어진다. 반면 `DOMContentLoaded`는 DOM 트리가 완성된 후에 실행되기 때문에 페이지가 빨리 로드되는 경우 사용자가 페이지를 볼 수 있는 시간이 빨라진다.

#### `loadFromLocalStorage()` 함수

```js
function loadFromLocalStorage() {
    const todos = JSON.parse(localStorage.getItem('todos')) || [];
    todos.forEach(todo => appendItem(todo));
}
```

페이지가 로드될 때 로컬 스토리지에 저장된 할 일 목록을 가져와서 화면에 표시한다. 로컬 스토리지에서 `todos` 라는 키로 문자열 값을 가져와서 JSON으로 파싱한다. 만약 값이 존재하지 않거나 파싱 에러가 발생하면 빈 배열을 반환한다.

그렇게 가져온 todos 배열을 forEach로 순회하면서 `appendItem()` 함수를 호출한다. 해당 함수는 주어진 투두 객체를 받아서 화면에 표시한다.

#### `click` 이벤트 리스너

```js
document.getElementById('addTaskBtn').addEventListener('click', addTodo);
```

`addTaskBtn` 버튼을 클릭하면 `addTodo()` 함수를 호출한다. 이 함수는 새로운 투두를 추가하는 함수이다. 

### `addTodo()` 함수

```js
function addTodo() {
    const input = document.getElementById('newTask');

    if (input.value.trim() === '') return;

    const todo = {
        id: Date.now(),
        content: input.value.trim(),
        done: false
    };

    appendItem(todo);
    saveToLocalStorage(todo);

    input.value = '';
}
```

- 먼저 할 일을 입력받는 `newTask` id를 가진 인풋 태그를 가져온다. 
- 그리고 인풋 태그의 값이 비어있는지 확인한다. 만약 비어있다면 함수를 리턴한다.
- 새로운 투두 객체인 `todo`를 생성한다. 다음 정보를 가진다.
    - id: 현재 시간을 밀리초로 나타낸 값 (id의 경우 auto increment를 사용하는 것보다 간단하게 현재 시간을 밀리초로 나타낸 값을 사용했다.)
    - content: 인풋 태그의 값 (`trim()`을 사용하여 앞뒤 공백을 제거한다.)
    - done: 할 일이 완료되었는지 나타내는 값
- `appendItem()` 함수를 호출하여 화면에 새로 만들어진 할 일을 추가한다.
- `saveToLocalStorage()` 함수를 호출하여 로컬 스토리지에 새로운 할 일을 저장한다.
- 마지막으로 인풋 태그의 입력 필드값을 비워준다. 이렇게 해야 추가 후에도 사용자가 다음 할 일을 입력할 수 있다.

### `appendItem(todo)` 함수

```js
function appendItem(todo) {
    const block = todo.done ? document.getElementById('doneBlock') : document.getElementById('todoBlock');
    const item = document.createElement('div');

    item.className = 'item';
    item.innerHTML = `
        ${todo.content}
        <span>
            <button onclick="toggleDone(${todo.id})">↔️</button>
            <button onclick="deleteItem(${todo.id})">❌</button>
        </span>
    `;

    block.appendChild(item);
}
```

- 투두 객체의 `done` 프로퍼티를 체크해서, 완료된 투두라면 `doneBlock`을, 아니라면 `todoBlock`을 선택하여 `block` 변수에 할당한다. (즉 어디에 추가할지 결정한다.)
- `div` 태그에 해당하는 `item` 변수를 생성한다. 이 `item` 변수는 인자로 받은 투두 객체를 화면에 표시할 때 사용된다.
- `item` 변수의 `className` 프로퍼티에 `item`을 할당한다. 즉 `div` 요소의 클래스 이름을 `item`으로 설정한다.
- `item` 변수의 `innerHTML` 프로퍼티에 투두 객체의 내용과 버튼을 추가한다.
    - 투두 객체의 내용은 `${todo.content}`로 표시한다.
    - 버튼은 `span` 태그로 감싸서 표시한다. 이렇게 하면 버튼이 한 줄에 표시된다.

#### `span` vs `div`

두 버튼을 `span` 태그로 감싸서 표시했다. `span` 태그는 인라인 요소이다. 즉, `div` 태그와 달리 한 줄에 표시된다. `div` 태그는 블록 요소이다. 즉, `span` 태그와 달리 한 줄을 다 차지한다.

### `toggleDone(id)` 함수

```js
function toggleDone(id) {
    const todos = JSON.parse(localStorage.getItem('todos')) || [];
    const index = todos.findIndex(todo => todo.id === id);

    if (index === -1) return;
    todos[index].done = !todos[index].done;

    localStorage.setItem('todos', JSON.stringify(todos));

    renderTodos();
}
```

- 로컬 스토리지에서 `todos` 키로 문자열 값을 가져와서 JSON으로 파싱한다. 만약 값이 존재하지 않거나 파싱 에러가 발생하면 빈 배열을 반환한다.
- `findIndex()` 메서드를 사용하여 주어진 조건에 맞는 요소의 인덱스를 반환한다. 만약 조건에 맞는 요소가 없다면 -1을 반환한다.
- 만약 인덱스가 -1이라면 함수를 리턴한다.
- 인덱스가 -1이 아니라면 해당 인덱스의 `done` 프로퍼티를 반전시킨다. (`true` <-> `false`)
- 로컬 스토리지에 `todos` 키로 문자열 값을 저장한다. 이때 `JSON.stringify()` 메서드를 사용하여 문자열로 변환한다.
- `renderTodos()` 함수를 호출하여 화면을 다시 렌더링한다.

### `deleteItem(id)` 함수

```js
function deleteItem(id) {
    let todos = JSON.parse(localStorage.getItem('todos')) || [];

    todos = todos.filter(todo => todo.id !== id);
    localStorage.setItem('todos', JSON.stringify(todos));

    renderTodos();
}
```

- 로컬 스토리지에서 `todos` 키로 문자열 값을 가져와서 JSON으로 파싱한다. 만약 값이 존재하지 않거나 파싱 에러가 발생하면 빈 배열을 반환한다.
- `filter()` 메서드를 사용하여 주어진 조건에 맞는 요소만 추출한다. 즉, `id`가 일치하지 않는 요소만 추출한다.
- 로컬 스토리지에 `todos` 키로 문자열 값을 저장한다. 이때 `JSON.stringify()` 메서드를 사용하여 문자열로 변환한다.
- `renderTodos()` 함수를 호출하여 화면을 다시 렌더링한다.

### `renderTodos()` 함수

```js
function saveToLocalStorage(todo) {
    const todos = JSON.parse(localStorage.getItem('todos')) || [];
    todos.push(todo);
    localStorage.setItem('todos', JSON.stringify(todos));
}
```
- 로컬 스토리지에서 `todos` 키로 문자열 값을 가져와서 JSON으로 파싱한다. 만약 값이 존재하지 않거나 파싱 에러가 발생하면 빈 배열을 반환한다.
- `push()` 메서드를 사용하여 새로운 투두 객체를 배열에 추가한다.
- 로컬 스토리지에 `todos` 키로 문자열 값을 저장한다. 이때 `JSON.stringify()` 메서드를 사용하여 문자열로 변환한다.

### `loadFromLocalStorage()` 함수

```js
function loadFromLocalStorage() {
    const todos = JSON.parse(localStorage.getItem('todos')) || [];
    todos.forEach(todo => appendItem(todo));
}
```

- 로컬 스토리지에서 `todos` 키로 문자열 값을 가져와서 JSON으로 파싱한다. 만약 값이 존재하지 않거나 파싱 에러가 발생하면 빈 배열을 반환한다.
- `forEach()` 메서드를 사용하여 배열의 모든 요소에 대해 `appendItem()` 함수를 호출한다.

### `renderTodos()` 함수

```js
function renderTodos() {
    document.getElementById('todoBlock').innerHTML = "<h3>Todo</h3>";
    document.getElementById('doneBlock').innerHTML = "<h3>Done</h3>";
    loadFromLocalStorage();
}
```

- `todoBlock`과 `doneBlock`의 `innerHTML`을 초기화한다.
- `loadFromLocalStorage()` 함수를 호출하여 화면을 다시 렌더링한다.


## 리팩토링 해보기

### `appendItem(todo)` 함수

#### 기존 방식의 문제점

크게 두 가지 문제점이 있다.

1. `innerHTML` 방식을 사용하고 있다.
    - 해당 방식은 DOM을 직접적으로 수정한다. 즉 DOM을 수정할 때마다 DOM 트리를 다시 재구성한다. 즉 다시 렌더링하는 것이다. 규모가 크다면 성능 이슈가 발생할 수 있다.
2. 인라인 이벤트 핸들러를 사용하고 있다. 
    - HTML과 JS는 관심사가 다르므로 분리해야 한다.

2번보다는 1번이 가장 큰 문제점이다. 따라서 `item.innerHTML` 대신 `item.appendChild()`를 사용하고, 인라인 이벤트 핸들러 대신 `addEventListener()`를 사용하도록 리팩토링하자.

```js
function appendItem(todo) {
    const block = todo.done ? document.getElementById('doneBlock') : document.getElementById('todoBlock');
    const item = document.createElement('div');

    item.className = 'item';
    
    // 투두 내용 추가
    const content = document.createElement('div');
    content.className = 'content';
    content.innerText = todo.content;
    item.appendChild(content);

    // span 태그 추가
    const span = document.createElement('span');

    // 투두 토글 버튼 추가
    const toggleBtn = document.createElement('button');
    toggleBtn.className = 'toggleBtn';
    toggleBtn.innerText = todo.done ? '⏪' : '✅';
    toggleBtn.addEventListener('click', () => toggleDone(todo.id));
    span.appendChild(toggleBtn);

    // 투두 삭제 버튼 추가
    const deleteBtn = document.createElement('button');
    deleteBtn.className = 'deleteBtn';
    deleteBtn.innerText = '❌';
    deleteBtn.addEventListener('click', () => deleteItem(todo.id));
    span.appendChild(deleteBtn);

    item.appendChild(span);
    block.appendChild(item);
}
```

몇 가지 변경점이 있다. 기존에는 텍스트 노드였다면 투두의 내용을 div 안에 감싸도록 수정했다. 그리고 done 값에 따라 롤백 이모지와 체크 이모지를 다르게 표시하도록 수정했다. 그리고 버튼을 생성하고 이벤트 리스너를 추가했다. 마지막으로 `item`에 `content`와 `span`을 추가했다.

## 그 외 개선해보면 좋을 점

매번 함수에서 로컬 스토리지에서 `todos` 키로 문자열 값을 가져와서 JSON으로 파싱하는 로직이 중복되어 있다. 뭔가 이러한 상태 값의 경우 중앙에서 관리하고... 변경될 때마다 업데이트하면 어떨까?

```js
const state = {
    todos: JSON.parse(localStorage.getItem('todos')) || []
};
```

중앙에 이런 식으로 상태를 배치하고...

```js
function addTodoToState(todo) {
    state.todos.push(todo);
    updateLocalStorage();
    renderTodo(todo);
}
```

이런 식으로 상태를 업데이트하는 것이다.

렌더링 로직도 전체가 아닌 부분만 업데이트 되도록 최적화해보면 어떨까?

```js
function renderTodo(todo) {
    const block = todo.done ? document.getElementById('doneBlock') : document.getElementById('todoBlock');
    const item = createTodoElement(todo);
    block.appendChild(item);
}

function renderTodos() {
    document.getElementById('todoBlock').innerHTML = '';
    document.getElementById('doneBlock').innerHTML = '';
    state.todos.forEach(todo => renderTodo(todo));
}

document.addEventListener("DOMContentLoaded", function() {
    renderTodos();
    ...
}
```

모든 투두를 렌더링하는 함수와 각각의 투두를 렌더링하는 함수를 분리했다.

실제로는 이렇게 사용될 것이다.

```js
function addTodoToState(todo) {
    state.todos.push(todo);
    updateLocalStorage();
    renderTodo(todo);
}
```

느낌적인 부분으로만 잡아봤고 시간이 없어서 구현은 하지 못했다. 나중에 기회가 된다면 리팩토링 해보는 것으로 하자.