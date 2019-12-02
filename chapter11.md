# Chapter 11 - 컴포넌트 성능 최적화
# Index
- [많은 데이터 렌더링하기](#11.1)
- [크롬 개발자 도구를 통한 성능 모니터링](#11.2)
- [느려지는 원인 분석](#11.3)
- [React.memo를 사용하여 컴포넌트 성능 최적화](#11.4)
- [onToggle, onRemove 함수가 바뀌지 않게 하기](#11.5)
- [불변성의 중요성](#11.6)
- [TodoList 컴포넌트 최적화하기](#11.7)
- [react-virtualized를 사용한 렌더링 최적화](#11.8)

# 11.1
```js
function crateBulkTodos() {
    const array = [];
    for (let i = 1; i <= 2500; i++) {
        array.push({
            id: i,
            text: `할 일 ${i}`,
            checked = false,
        });
    }
    return array;
}
```
- createBulkTodos 함수를 이용하여 useState의 기본값에 넣어준다.
- useState(createBulkTodos)처럼 파라미터를 함수 형태로 넣어주면 컴포넌트가 처음 렌더링될 때만 createBulkTodos 함수가 실행된다.
- [목록으로](#INDEX)

# 11.2
- 성능을 분석해야 할 때에는 크롬 개발자 도구의 Performance 탭을 사용하여 측정하면 된다.

- [목록으로](#INDEX)

# 11.3
## 컴포넌트에서 리렌더링이 발생할 때
    1. 자신이 전달받은 props가 변경될 때
    2. 자신의 state가 바뀔 때
    3. 부모 컴포넌트가 리렌더링될 때
    4. forceUpdate 함수가 실행될 때

- 할 일 1 항목에 체크하는 경우 App 컴포넌트의 state가 변경되면서 App 컴포넌트가 리렌더링된다.
- 부모 컴포넌트가 리렌더링 됐으므로 TodoList 컴포넌트가 리렌더링되고 그 안의 무수한 컴포넌트들도 리렌더링된다.
- 할 일 2 ~ 할 일 2500까지는 리렌더링될 필요가 없는데 모두 리렌더링되는 것이 문제!
- [목록으로](#INDEX)

# 11.4
- 컴포넌트의 리렌더링을 방지할 때에는 React.memo라는 함수를 사용하면 된다.
```js
export default React.memo(TodoListItem);
```
- TodoListItem에서 위 코드만 수정해주면 된다.

- [목록으로](#INDEX)

# 11.5
## 또 다른 문제점
- todos 배열이 업데이트되면 onRemove와 onToggle 함수도 새롭게 바뀐다.
- onRemove와 onToggle은 배열 상태를 업데이트하는 과정에서 최신 상태의 todos를 참조하기 때문에 todos 배열이 바뀔 때마다 함수가 새로 만들어진다.
## 해결방법
    1. useState의 함수형 업데이트 기능을 사용한다.
    2. useReducer를 사용한다.

### useState의 함수형 업데이트
- setTodos 함수를 사용할 떄에는 새로운 상태를 파라미터로 넣어 주었다.
- setTodos를 사용할 때 새로운 상태를 파라미터로 넣는 대신, 상태 업데이트를 어떻게 할 지 정의해주는 업데이트 함수를 넣을 수도 있다.
```js
const [number, setNumber] = useState(0);
// preNumbers는 현재 number 값을 가리킨다.
const onIncrease = useCallback(
    () => setNumber(preNumber => preNumber + 1),
    [],
);
```
- 위 코드처럼 어떻게 업데이트할 지 정의해주는 업데이트 함수를 넣어준다.

### useReducer 사용하기
- useState의 함수형 업데이트를 사용하는 대신, useReducer를 사용해도 onToggle과 onRemove가 계속 새로워지는 문제를 해결할 수 있다.
- useReducer를 사용할 때에는 원래 두 번째 파라미터에 초기 상태를 넣어주어야 한다.
- useReducer를 사용하는 방법은 기존 코드를 많이 고쳐야 한다는 단점이 있지만, 상태를 업데이트하는 로직을 모아서 컴포넌트 바깥에 둘 수 있다는 장점이 있다.
- [목록으로](#INDEX)

# 11.6
- 리액트 컴포넌트에서 상태를 업데이트할 때 불변성을 지키는 것은 매우 중요하다.
- 12장에서 immer를 배워보자.
- [목록으로](#INDEX)

# 11.7
- 리스트에 관련된 컴포넌트를 최적화할 때에는 리스트 내부에서 사용하는 컴포넌트도 최적화해야 하고 리스트로 사용되는 컴포넌트 자체도 최적화해주는 것이 좋다.

- [목록으로](#INDEX)

# 11.8
- react-virtualized를 사용하면 리스트 컴포넌트에서 스크롤되기 전에 보이지 않는 컴포넌트는 렌더링하지 않고 크기만 차지하게끔 할 수 있다.
- 그리고 만약 스크롤되면 해당 스크롤 위치에서 보여주어야 할 컴포넌트를 자연스럽게 렌더링시킬 수 있다.
- [목록으로](#INDEX)
