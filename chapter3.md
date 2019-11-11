# 클래스형 컴포넌트
- 클래스형 컴포넌트는 state 기능 및 라이프사이클 기능을 사용할 수 있고 임의 메서드를 정의할 수 있다.
- 클래스형 컴포넌트 내에 render 함수가 꼭 있어야 하고, JSX를 반환해야 한다.

## 함수형 컴포넌트 장점
- 클래스형 컴포넌트보다 선언하기 훨씬 편하다.
- 메모리 자원을 덜 사용한다.
- 빌드한 후 배포할 떄 결과물의 파일 크기가 더 작다.

## 함수형 컴포넌트 단점
- state와 라이프사이클 API의 사용이 불가능하다.  
=> Hooks라는 기능이 도입되면서 해결되었다.

# 컴포넌트 코드 설명

## export
- 모듈 내보내기
```js
export default MyComponent;
```
- 이 코드는 다른 파일에서 이 파일을 import할 때, 위에서 선언한 MyComponent 클래스를 불러오도록 설정한다.
## import
- 모듈 불러오기

# props
- properties를 줄인 표현으로 컴포넌트 속성을 설정할 때 사용하는 요소
- 해당 컴포넌트를 불러와 사용하는 부모 컴포넌트에서 설정할 수 있다.
- props는 컴포넌트가 사용되는 과정에서 부모 컴포넌트가 설정하는 값이며, 컴포넌트 자신은 해당 props를 읽기 전용으로만 사용할 수 있다.

## props 렌더링
- props값은 컴포넌트 함수의 파라미터로 받아와서 사용할 수 있다.
```js
{/* MyComponent.js */}
import React from 'react';

const MyComponent = (props) => {
    return <div> 나의 새롭고 멋진 컴포넌트 {props.name}</div>;
};

export default MyComponent;
```

## props 값 지정
- 부모 컴포넌트에서 값을 지정해 줄 수 있다.
```js
{/* App.js */}
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return <MyComponent name="React" />;
}

export default App;
```

## props 기본값 설정 : defaultProps
- defaultProps를 사용하면 된다.
- 위에 MyComponent.js에 다음과 같은 코드를 추가하면 된다.
```js
MyComponent.defaultProps = {
    name: '기본 이름'
};
```
## children
- 컴포넌트 태그 사이의 내용을 보여주는 props이다.
- MyComponent 태그 사이에 작성한 문자열을 MyComponent 내부에서 보여주려면 props. children 값을 보여주어야 한다.
```js
{/* App.js */}
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return <MyComponent>리액트</MyComponent>;
}

export default App;

{/* MyComponent.js */}
import React from 'react';

const MyComponent = (props) => {
    return (
        <div> 
        나의 새롭고 멋진 컴포넌트 {props.name}<br />
        children 값은 {props.children}
        입니다.
        </div>;
    );
};
MyComponent.defaultProps = {
    name: '기본 이름'
};
export default MyComponent;
```

## propTypes를 통한 props 검증
- 컴포넌트의 필수 props를 지정하거나 타입을 지정할 때에 사용한다.
- import 구문을 사용하여 'props-types'를 불러와야 한다.
- MyComponent.js에 name을 무조건 String 형태로 전달하고 싶다면 다음과 같은 코드를 추가해야 한다.
```js
MyComponent.propsTypes = {
    name: PropTypes.string
};
```

## isRequired를 사용하여 필수 propTypes 설정
- propTypes를 지정하지 않았을 때 경고 메시지를 띄워주는 역할을 한다.
```js
MyComponent.propsTypes = {
    name: PropTypes.string
    favoriteNumber: PropTypes.number.isRequired
};
```
- 위에 코드를 추가하면 favoriteNumber를 설정하지 않으면 경고문이 뜨게 된다.

# state
- 컴포넌트 내부에서 바뀔 수 있는 값을 의미한다.
- 클래스형 컴포넌트가 지니고 있는 state
- 함수형 컴포넌트에서 useState라는 함수를 통해 사용하는 state

## 클래스형 컴포넌트의 state
