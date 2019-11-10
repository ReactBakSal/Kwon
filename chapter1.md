# React 이해
- MVC 구조 중, 오직 V(View)만 신경 쓰는 라이브러리이다.

## Component(컴포넌트)
- 리액트 프로젝트에서 특정 부분이 어떻게 생길지 정하는 선언체
- 재사용이 가능한 API로 수많은 기능들을 내장하고 있으며, 컴포넌트 하나에서 해당 컴포넌트의 생김새와 작동 방식을 정의한다.

## 렌더링
- 사용자 화면에 뷰를 보여주는 것

### 초기 렌더링
- render() 함수로 컴포넌트가 어떻게 생겼는지 정의하는 역할을 한다.
- 뷰가 어떻게 생겼고 어떻게 작동하는지에 대한 정보를 지닌 <b>객체를 반환</b>한다.
- 모든 렌더링이 끝나면 지니고 있는 정보들을 사용하여 HTML 마크업을 만들고, 이를 우리가 정하는 실제 페이지의 DOM 요소 안에 주입한다.

### 조화(업데이트) 과정
- 리액트에서 중요한 부분이다.
- 컴포넌트에서 데이터에 변화가 있을 때 새로운 요소로 갈아 끼운다고 생각하면 된다.
- 새로운 데이터를 가지고 render 함수를 또 다시 호출한다.
- 그렇게 되면 새로운 데이터를 지닌 뷰를 생성해낸다. (이 뷰를 new render라 칭한다.)
- 이 때 new render를 곧바로 DOM에 반영하지 않고, 이전에 render 함수가 만들었던 컴포넌트 정보와 new render를 비교한다.
- 두 가지 뷰를 최소한의 연산으로 비교한 후, 둘의 차이를 알아내 최소한의 연산으로 DOM 트리를 업데이트를 한다.

# React 특징

## Virtual DOM
- 리액트의 주요 특징 중 하나이다.
- 위에서 말한 new render가 바로 Virtual DOM이다.

### DOM이란?
- Document Object Model의 약어이다.
- 객체로 문서 구조를 표현하는 방법으로 XML이나 HTML로 작성한다.
- DOM은 트리 형태라서 특정 노드를 찾거나 수정하거나 제거하거나 원하는 곳에 삽입할 수 있다.

### DOM은 느리다?
- DOM 자체는 빠르다.
- DOM에 변화가 일어나면 웹 브라우저가 CSS를 다시 연산하고, 레이아웃을 구성하고, 페이지를 리페인트할 때 비용이 많이 들어간다.

### 해결법
- DOM을 최소한으로 조작하여 작업을 처리하는 방식으로 개선할 수 있다.
- 리액트에서는 Virtual DOM 방식을 사용하여 DOM 업데이트를 추상화함으로써 DOM 처리 횟수를 최소화하고 효율적으로 진행한다.

### 오해
- Virtual DOM을 사용한다고 해서 무조건 빨라지는 것은 아니다.
- 작업이 매우 간단할 떄에는 리액트를 사용하지 않는 편이 더 나은 성능을 보인다.

## 기타 특징
- 리액트는 프레임워크가 아니라 라이브러리이다.
- 기타 기능은 직접 구현하여 사용해야 한다.
- 다른 라이브러리를 사용하면 되므로 취향대로 스택을 설정할 수 있다는 장점을 지닌다.
- 여러 라이브러리를 접해야 한다는 단점을 가지고 있다.