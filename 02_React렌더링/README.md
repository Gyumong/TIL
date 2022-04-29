# React에서의 렌더링

중요도: 상
태그: 반복학습

# Element

엘리먼트는 React 앱의 가장 작은 단위다. 엘리먼트는 화면에 표시할 내용을 기술한다.

또한 엘리먼트는 불변객체이다. 엘리먼트를 생성한 이후에는 해당 엘리먼트의 자식이나 속성을 변경할 수 없다.

엘리먼트를 업데이트하는 유일한 방법은 새로운 앨리먼트를 생성하고 이를 ReactDOM.render( )로 전달하는 것이다.

```jsx
const element = <h1>Hi</hi>;
ReactDOM.render(element, document.getElementById('root'));

// 참고
ReactDOM.render(element, container[, callback])
```

브라우저 DOM과는 달리 React 엘리먼트는 일반 객체이며 쉽게 생성가능하다. React DOM은 React 엘리먼트와 일치하도록 DOM을 업데이트한다.

React로 구현된 애플리케이션은 일반적으로 하나의 루트 DOM 노드가 있다. React 엘리먼트를 루트 DOM 노드에 렌더링하려면 둘 다 ReactDOM.redner( )로 전달하면 된다.

ReactDOM.render( )는 전달한 컨테이너 노드의 콘텐츠를 제어하고 처음 호출 할 때 기존의 DOM 엘리먼트를 교체하며 이후의 호출은 React의 DOM diffing 알고리즘을 사용하여 해당 엘리먼트와 그 자식 엘리먼트를 이전의 엘리먼트와 비교하고 필요한 부분만 업데이트한다.

# [재조정](<https://github.com/Gyumong/TIL/tree/main/03_%EC%9E%AC%EC%A1%B0%EC%A0%95(Reconciliation)>)

state나 props 상태가 업데이트 되면 React render( )함수는 새로운 element tree를 반환한다. 이 과정에서 효과적으로 UI를 업데이트 하는 방법이 필요.

# 가상돔이 뭐임 그래서?

Virtual DOM은 ReactDOM이 UI를 다시 그릴때 쓰이는 재조정 과정인 것 같다.

현재 React 16 버전부터는 [Fiber](https://github.com/acdlite/react-fiber-architecture)라는 새로운 재조정 엔진이 적용되었다.
