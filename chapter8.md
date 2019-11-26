# Chapter 8 - Hooks

# Index
- [Hooks](#Hooks)
- [useState](#useState)
- [useEffect](#useEffect)
- [useReducer](#useReducer)
- [useMemo](#useMemo)
- [useCallback](#useCallback)
- [useRef](#useRef)

# Hooks
- 함수형 컴포넌트에서도 useState, useEffect 기능을 제공하여 다양한 작업을 가능케 해준다.
- 리액트 내장 Hooks를 배운 뒤 커스텀 Hooks를 만들어본다.

[목록으로](#INDEX)

# useState
- 가장 기본적인 Hook
- 함수형 컴포넌트에서도 state를 지닐 수 있게 해준다.
```js
{/* Counter.js */}
import React, {useState} from 'react';

const Counter = () => {
    const [value, setValue ] = useState(0); // 카운터의 기본값이 0임을 의미한다.
    return (
        <div>
            <p>
                현재 카운터 값은 <b> {value}</b>입니다.
            </p>
            <button onClick= {() => setValue(value + 1)}>+1</button>
            <button onClick= {() => setValue(value - 1)}>-1</button>
        </div>
    );
};

export default Counter;
```

``` js
{/* App.js */}
import React from 'react';
import Counter from './Counter';

function App() {
  return <Counter />;
};

export default App;

```
## useState를 여러 번 사용하기
- 하나의 useState 함수는 하나의 상태 값만 관리할 수 있다.
- useState를 여러 번 사용하여 여러 개의 state 값을 관리할 수 있다.
```js
{/* Info.js */}
import React, { useState } from 'react';

const Info = () => {
    const [name, setName] = useState('');
    const [nickname, setNickname] = useState('');

    const onChangeName = e => {
        setName(e.target.value);
    };

    const onChangeNickname = e => {
        setNickname(e.target.value);
    };

    return (
        <div>
            <div>
                <input value={name} onChange={onChangeName} />
                <input value={nickname} onChange={onChangeNickname} />
            </div>
            <div>
                <div>
                    <b>이름:</b> {name}
                </div>
                <div>
                    <b>닉네임:</b> {nickname}
                </div>
            </div>
        </div>
    );
};

export default Info;
```

``` js
{/* App.js */}
import React from 'react';
import Info from './Info';

function App() {
  return <Info />;
};

export default App;

```
[목록으로](#INDEX)



# useEffect
- 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook
```js
{/* Info.js 에 추가 */}
import React, { useState, useEffect } from 'react';

const Info = () => {
    const [name, setName] = useState('');
    const [nickname, setNickname] = useState('');

    useEffect(() => {
        console.log('렌더링이 완료되었습니다.');
        console.log({
            name,
            nickname
        });
    });
...
```
## useEffect 원리
- state가 변경될 때마다 useEffect()를 항상 실행하는데 그 기준이 두 번째 파라미터 배열을 본다.
- 그 전에 있던 배열과 지금의 배열을 비교해서 바뀐게 있으면 바꿔준다.

## 마운트될 때에만 실행하고 싶을 때
- 업데이트될 때에는 실행하지 않으려면 함수의 두 번째 파라미터로 비어 있는 배열을 넣어주면 된다.
```js
{/* Info.js useEffect 에 추가 */}

    useEffect(() => {
        console.log('렌더링이 완료되었습니다.');
    }, []);
```

## 특정 값이 업데이트될 때만 실행하고 싶을 때
- 두 번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣어주면 된다.
```js
{/* Info.js useEffect 에 추가 */}

    useEffect(() => {
        console.log(name);
    }, [name]);
```
- 배열 안에는 useState를 통해 관리하고 있는 상태를 넣어 주어도 되고, props로 전달받은 값을 넣어 주어도 된다.

## cleanup
- 컴포넌트가 언마운트되기 전이나 업데이트 직전에 어떠한 작업을 수행하고 싶다면 useEffect에서 뒷정리(cleanup) 함수를 반환해 주어야 한다.
```js
{/* Info.js useEffect 에 추가 */}

    useEffect(() => {
        console.log('effect');
        console.log(name);
        return() => {
            console.log('cleanup');
            console.log(name);
        };
    });
```

``` js
{/* App.js */}
import React, {useState} from 'react';
import Info from './Info';

const App = () => {
  const [visible, setVisible] = useState(false);
  return (
    <div>
      <button onClick={() => {
        setVisible(!visible);
      }}
      >
        {visible ? '숨기기' : '보이기'}
      </button>
      <hr />
      {visible && <Info />}
    </div>
  );
};
export default App;

```
- 컴포넌트가 나타날 때 콘솔에 effect가 나타나고, 사라질 때 cleanup이 나타난다.
- 렌더링될 때마다 cleanup 함수가 계속 나타난다. 그리고 cleanup 함수가 호출될 때에는 업데이트되기 직전의 값을 보여준다.

[목록으로](#INDEX)

# useReducer
- useState보다 더 다양한 컴포넌트 상황에 따라 다양한 state를 다른 값으로 업데이트해 줄 때에 사용하는 Hook
- reducer는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션 값을 전달받아 새로운 상태를 반환하는 함수
- reducer 함수에서 새로운 상태를 만들 때에는 반드시 불변성을 지켜줘야 한다.
```js
function reducer(state, action) {
    return {...}; // 불변성을 지키면서 업데이트한 새로운 상태를 반환
}
```
- 액션 값은 다음과 같은 형태로 이루어진다.
```js
{
    type: 'INCREMENT',
    // 다른 값들이 필요하면 추가로 들어간다.
}
```
- useReducer에서 사용하는 액션 객체에서 반드시 type을 지니고 있을 필요가 없다.
## 카운터 구현하기
```js
{/* Counter.js 수정 */}
import React, {useReducer} from 'react';

function reducer(state, action) {
    // action.type에 따라 다른 작업 수행
    switch (action.type) {
        case 'INCREMENT':
            return { value: state.value + 1 };
        case 'DECREMENT':
            return { value: state.value - 1 };
        default:
            return state;        
    }
}

const Counter = () => {
    const [state, dispatch] = useReducer(reducer, { value: 0 }); // 카운터의 기본값이 0임을 의미한다.
    return (
        <div>
            <p>
                현재 카운터 값은 <b> {state.value} </b>입니다.
            </p>
            <button onClick= {() => dispatch({type: 'INCREMENT'})}>+1</button>
            <button onClick= {() => dispatch({type: 'DECREMENT'})}>-1</button>
        </div>
    );
};

export default Counter;
```
- state : 현재 가리키고 있는 상태
- dispatch : 액션을 발생시키는 함수
- dispatch(action)과 같은 형태로, 함수 안에 파라미터로 액션 값을 넣어주면 리듀서 함수가 호출되는 구조이다.
- useReducer를 사용했을 때의 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 점이다.
``` js
{/* App.js */}
import React from 'react';
import Counter from './Counter';

function App() {
  return <Counter />;
};

export default App;

```
## 인풋 상태 관리하기
```js
{ /* Info.js 수정 */}
import React, { useReducer } from 'react';


    function reducer(state, action) {
        return {
            ...state,
            [action.name]: action.value
        };
    }

    const Info = () => {
        const [state, dispatch] = useReducer(reducer, {
            name: '',
            nickname: ''
        });
        const { name, nickname }  = state;
        const onChange = e => {
            dispatch(e.target);
        };
        return (
            <div>
                <div>
                    <input value={name} onChange={onChange} />
                    <input value={nickname} onChange={onChange} />
                </div>
                <div>
                    <div>
                        <b>이름:</b> {name}
                    </div>
                    <div>
                        <b>닉네임:</b> {nickname}
                    </div>
                </div>
            </div>
        );
    };

export default Info;
```
[목록으로](#INDEX)

# useMemo
- 함수형 컴포넌트 내부에서 발생하는 연산을 최적화할 수 있다.
```js
{/* Average.js
    리스트에 숫자를 추가하면 추가된 숫자들의 평균을 보여주는 컴포넌트
 */}
import React, {useState} from 'react';


    const getAverage = (numbers) => {
        console.log('평균값 계산 중...');
        if (numbers.length === 0 ) return 0 ;
        const sum = numbers.reduce( (a,b) => a + b);
        return sum / numbers.length;
    };
    
    const Average = () => {
        const [list, setList] = useState([]);
        const [number, setNumber] = useState('');

        const onChange = e => {
            setNumber(e.target.value);
        };
        const onInsert = e => {
            const nextList = list.concat(parseInt(number));
            setList(nextList);
            setNumber('');
        };
        return (
            <div>
                <input value={number} onChange={onChange} />
                <button onClick={onInsert}>등록</button>
                <ul>
                    {list.map((value, index) => (
                        <li key={index}>{value}</li>
                    ))}
                </ul>
                <div>
                    <b>평균값:</b> {getAverage(list)}
                </div>
            </div>
        );
    };

export default Average;
```
- 문제점 : 인풋 내용이 수정될 때에도 getAverage 함수가 호출된다.
- useMemo Hook을 사용하여 최적화할 수 있다.
- 렌더링하는 과정에서 특정 값이 바뀌었을 때만 연산을 실행하고, 원하는 값이 바뀌지 않았다면 이전에 결과를 다시 사용하는 방식이다.
```js
{/* Average.js 변경 */}
import React, {useState, useMemo} from 'react';


    const getAverage = (numbers) => {
        console.log('평균값 계산 중...');
        if (numbers.length === 0 ) return 0 ;
        const sum = numbers.reduce( (a,b) => a + b);
        return sum / numbers.length;
    };
    
    const Average = () => {
        const [list, setList] = useState([]);
        const [number, setNumber] = useState('');

        const onChange = e => {
            setNumber(e.target.value);
        };
        const onInsert = e => {
            const nextList = list.concat(parseInt(number));
            setList(nextList);
            setNumber('');
        };
        
        const avg = useMemo( () => getAverage(list), [list]);
        return (
            <div>
                <input value={number} onChange={onChange} />
                <button onClick={onInsert}>등록</button>
                <ul>
                    {list.map((value, index) => (
                        <li key={index}>{value}</li>
                    ))}
                </ul>
                <div>
                    <b>평균값:</b> {avg}
                </div>
            </div>
        );
    };

export default Average;
```
[목록으로](#INDEX)

# useCallback
- 최적화할 때 주로 사용하는 Hook
- 이벤트 핸들러 함수를 필요할 때에만 생성할 수 있다.
- Average 컴포넌트에서 onChange와 onInsert 함수는 컴포넌트가 리렌더링될 때마다 새로 생성된다.
- 이 부분을 useCallback을 통해 최적화할 수 있다.
```js
{/* Average.js 수정 */}
import React, {useState, useMemo, useCallback} from 'react';


    const getAverage = (numbers) => {
        console.log('평균값 계산 중...');
        if (numbers.length === 0 ) return 0 ;
        const sum = numbers.reduce( (a,b) => a + b);
        return sum / numbers.length;
    };
    
    const Average = () => {
        const [list, setList] = useState([]);
        const [number, setNumber] = useState('');

        const onChange = useCallback(e => {
            setNumber(e.target.value);
        }, []); // 컴포넌트가 처음 렌더링될 때에만 함수 생성
        const onInsert = useCallback(() => {
            const nextList = list.concat(parseInt(number));
            setList(nextList);
            setNumber('');
        }, [number, list]); // number 혹은 list가 바뀌었을 때에만 함수 생성
        
        const avg = useMemo( () => getAverage(list), [list]);
        return (
            <div>
                <input value={number} onChange={onChange} />
                <button onClick={onInsert}>등록</button>
                <ul>
                    {list.map((value, index) => (
                        <li key={index}>{value}</li>
                    ))}
                </ul>
                <div>
                    <b>평균값:</b> {avg}
                </div>
            </div>
        );
    };

export default Average;
```
- useCallback의 첫 번째 파라미터에는 생성하고 싶은 함수를 넣는다.
- useCallback의 두 번째 파라미터에는 배열을 넣으면 된다.
- 이 때, 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시해야 한다.

[목록으로](#INDEX)

# useRef
- ref를 쉽게 사용할 수 있도록 해준다.
```js
{/* Average.js 수정 */}
...
    const Average = () => {
        const [list, setList] = useState([]);
        const [number, setNumber] = useState('');
        const inputEl = useRef(null);
...
        const onInsert = useCallback(() => {
            const nextList = list.concat(parseInt(number));
            setList(nextList);
            setNumber('');
            inputEl.current.focus();
        }, [number, list]); // number 혹은 list가 바뀌었을 때에만 함수 생성
        const avg = useMemo( () => getAverage(list), [list]);
        return (
            <div>
                <input value={number} onChange={onChange} ref={inputEl}/>
...     
- useRef를 통해 만든 객체 안의 current 값이 실제 엘리먼트를 가리킨다.

## 로컬 변수 사용하기
- 로컬 변수 : 렌더링과 상관없이 바뀔수 있는 값
- ref 안의 값이 바뀌어도 컴포넌트가 렌더링되지 않는다.
```
[목록으로](#INDEX)
