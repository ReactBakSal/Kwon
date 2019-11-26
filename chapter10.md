# Chapter 10 - 일정 관리 웹 애플리케이션 만들기

# Index
- [프로젝트 준비하기](#PROLOG)
- [UI 구성하기](#UI)
- [기능 구현하기](#구현)

# PROLOG
## 라이브러리
- node-sass :  Sass를 사용할 수 있는 라이브러리
- classnames : 조건부 스타일링을 좀 더 편하게 하기 위한 라이브러리
- react-icons : 리액트에서 다양한 아이콘을 사용할 수 있는 라이브러리

## Prettier 설정
- Prettier : 코드 스타일을 지정할 수 있다. (2장 참고)

### .prettierrc
```json
{
    "singleQuote" : true, // 큰 따옴표 대신 작은 따옴표를 사용
    "semi" : true, // 명령문 끝에 세미콜론을 출력

    "useTabs" : false, // 공백 대신 탭으로 줄을 들여써라
    "tabWidth" : 2, // 들여 쓰기 수준 당 공백 수를 지정
    "trailingComma" : "all", // 여러 줄을 사용할 때 가능한 한 후행 쉼표를 출력
    "printWidth" : 80 // 프린터를 감쌀 줄 길이를 지정
}
```
- [목록으로](#INDEX)

# UI
## 컴포넌트 소개
1. TodoTemplate
    - 화면을 가운데에 정렬시켜 주며, 앱 타이틀(일정 관리)을 보여준다.
    - children으로 내부 JSX를 props로 받아 와서 렌더링해준다.
2. TodoInsert
    - 새로운 항목을 입력하고 추가할 수 있는 컴포넌트
    - state를 통해 인풋의 상태를 관리한다.
3. TodoListItem
    - 각 할 일 항목에 대한 정보를 보여주는 컴포넌트
    - todo 객체를 props로 받아 와서 상태에 따라 다른 스타일의 UI를 보여준다.
4. TodoList 
    - todos 배열을 props로 받아 온 후, 이 를 배열 내장 함수 map을 사용해서 여러 개의 TodoListItem 컴포넌트로 변환하여 보여준다.

## TodoTemplate
- TodoTemplate.js가 VS Code에서 다른 탭으로 열려 있지 않다면 jsonconfig.json을 만들고 Ctrl + Space를 하자.

## TodoInsert
### 모르는 부분
```css
&::placeholder {
    color: #dee2e6;
}
cursor: pointer;
transition: 0.1s, background-color ease-in;
&:hover {
    background: #adb5bd;
}
```
## TodoListItem
### 모르는 부분
```css
 &.checked {
    svg {
        color: #22b8cf;
    }
    .text {
        color: #adb5bd;
        text-decoration: line-through;
    }
}
```
- [목록으로](#INDEX)

# 구현
## App에서 todos 상태 사용하기
- 나중에 추가할 일정 항목에 대한 상태들은 모두 App 컴포넌트에서 관리한다.
- App.js에서 useState를 사용하여 todos라는 상태를 정의하고 todos를 TodoList의 props로 전달
```js
const [todos, setTodos] = useState([
    {
        id: 1,
        text: '리액트의 기초 알아보기',
        checked: true,
    },
    {
        id: 2,
        text: '컴포넌트 스타일링해보기',
        checked: true,
    },
    {
        id: 3,
        text: '일정 관리 앱 만들어보기',
        checked: false,
    },
]);

return (
    <TodoTemplate>
      <TodoInser />
      <TodoList todos={todos} />
    </TodoTemplate>
);
```
- todos 배열 안에 들어 있는 객체에는 각 항목의 고유 id, 내용, 완료 여뷰를 알려주는 값이 포함된다.
- 이 배열은 TodoList에 props로 전달된다.
- TodoList에서 이 값을 받아 온 후 TodoItem으로 변환하여 렌더링하도록 설정해야 한다.
- 여러 종류의 값을 전달하는 경우에는 객체로 통째로 전달하는 편이 나중에 성능 최적화를 할 때 편리하다.

## 항목 추가 기능 구현하기
- TodoInsert 컴포넌트에서 인풋 상태를 관리
- App 컴포넌트에서 todos 배열에 새로운 객체를 추가하는 함수를 만들어야 한다.

### TodoInsert value 상태 관리하기
- useState 사용
- 인풋에 넣어 줄 onChange 함수 추가
- useCallback Hook을 이용하여 한 번 함수를 만들고 재사용한다.

### todos 배열에 새 객체 추가하기
- 새로운 객체를 만들 때마다 id 값에 1씩 더해줘야 한다. : useRef
- id값은 렌더링하는 정보가 아니기 때문에 useState가 아닌 useRef를 사용한다.

### TodoInsert에서 onSubmit 이벤트 설정
- button을 클릭하면 발생할 이벤트 설정
- App에서 TodoInsert에 넣어준 onInsert 함수에 현재 useState를 통해 관리하고 있는 value 값을 파라미터로 넣어서 호출한다.
- onSubmit 이벤트는 Enter를 눌렀을 때에도 발생한다.

## 지우기 기능 구현하기
- 리액트 컴포넌트에서 배열의 불변성을 지키면서 배열 원소를 제거해야 할 때, flilter를 사용한다.

### filter
- 기존의 배열은 그대로 둔 상태에서 특정 조건을 만족하는 원소들만 따로 추출하여 새로운 배열을 만든다.
```js
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const biggerThanFive = array.filter(number => number > 5);
// 결과: [6, 7, 8, 9, 10]
```

### todos 배열에서 id로 항목 지우기
- App 컴포넌트에 id를 파라미터로 받아와서 같은 id를 가진 항목을 todos 배열에서 지우는 onRemove 함수 작성

### TodoListItem에서 삭제 함수 호출하기
- onRemove 함수를 TodoList에서 TOdoListItem에 그대로 전달하자.

## 수정 기능 구현하기
- onToggle 함수를 App에 구현한 뒤 TodoListItem에 전달하면 된다.

### map을 사용하여 특정 원소를 업데이트하는 이유
- map 함수는 배열 을 전체적으로 새로운 형태로 변환하여 새로운 배열을 생성할 때 사용한다.
- onToggle 함수를 보면 todo.id === id? ... : ...이라는 삼항 연산자가 사용된다.
- todo.id와 현재 파라미터로 사용된 id 값이 같을 때에는 새로운 규칙을 생성하지만
- id 값이 다를 때에는 변화를 주지 않고 처음 받아 왔던 상태 그대로 반환한다.
- 그렇기 때문에 map을 사용하여 만든 배열에서 변화가 필요한 원소만 업데이트되고 나머지는 그대로 남아있게 된다.
- [목록으로](#INDEX)
