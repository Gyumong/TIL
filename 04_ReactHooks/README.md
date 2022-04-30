# React hooks(useRef, useCallback, useMemo)

중요도: 상
태그: 반복학습

# [useRef](https://dev.to/dylanju/useref-3j37?fbclid=IwAR0fl7xMjn_Hp6sImCU-EQt4gJ0ob_YY6hS3cwn4ARyClTUYD2KN0R6X-O0)

```jsx
const refContainer = useRef(initialValue);
```

useRef는 .current 프로퍼티에 전달된 인자(initialValue)로 초기화된 변경가능한 ref 객체를 반환한다. 반환된 객체는 컴포넌트의 전 생애주기를 통해 유지된다. 또한 useRef는 DOM ref만을 위한 것은 아니다. ref 객체는 현재 프로퍼티를 변경할 수 있고 어떤 값이든 보유할 수 있는 class의 인스턴스 프로퍼트와 유사한 일반 컨테이너다.

실제로 단순히 DOM 노드나 React 엘리먼트에 접근하기 위한 방법이 아니라 일종의 변수를 관리하기 위한 목적으로도 많이 사용되고 있다. ex) react-redux: useSelector

useRef는 매번 렌더링 할 때 동일한 ref 객체를 제공한다. 그러나 useRef는 내용이 변경될 때 리렌더링이 발생하지 않는다. 즉 .current 프로퍼티가 변경되어도 리렌더링은 발생하지 않는다.

돌아가서 정리해보자면

1. **useRef( )는 일반적인 자바스크립트 객체이다.** 즉 heap 영역에 저장되는 변수
2. **매번 렌더링할 때 동일한 객체를 제공한다**. heap에 저장되어 있기 때문에 어플리케이션이 종료되거나 GC처리 될때까지, 참조할때마다 같은 메모리 값을 가진다.
3. **값이 변경되어도 리렌더링 되지 않는다.** 같은 메모리 주소를 갖고있기 때문에 JS의 === 연산이 항상 true를 반환한다. 즉 변경사항을 감지할 수 없어서 리렌더링을 하지않는다.

## useRef의 존재 이유

useRef는 함수형 컴포넌트에서 변수를 쉽게 다루기위해 (마치 클래스의 인스턴스 변수처럼) 만들어진 API이다. 다른 변수선언 방법과 차이를 비교해보면 아래와 같다.

1. hook 기반의 useState 혹은 useContext로 선언
   이렇게 선언한 변수들은 값이 바뀔때마다 리렌더링을 유발한다. 렌더링과 상관없는 변수를 선언하기엔 적당하지 않다.
2. 함수형 컴포넌트 내부에 const 혹은 let, var로 선언
   렌더링 될 때마다 값이 초기화 된다. 컴포넌트의 생애주기 동안 관리해야하는 변수를 선언하기에 적당하지 않다.
3. 컴포넌트 바깥에 const 혹은 let, var로 선언
   불필요한 렌더링을 유발하지도 않고, 렌더링될 때 값이 초기화 되지도 않지만 컴포넌트를 재사용하면서 값을 각각 따로 관리하는게 불가능하다.
4. useRef를 통해 선언
   useRef를 통해 선언된 변수는 리렌더링을 유발하지도 않고, 리렌더링 될 때도 이전의 값을 기억하고 있으며, 컴포넌트마다 각각의 값을 가질 수 있다.

## useRef에 대한 결론

컴포넌트 내부에서 관리하는 변수인데, 값이 바뀔 때마다 렌더링이 필요하면 `useState`를 쓰면 되고 아닐 경우 `useRef`를 써야한다고 생각하면 간단할 것 같다.

## 레퍼런스

[useRef()가 순수 자바스크립트 객체라는 의미를 곱씹어보기](https://dev.to/dylanju/useref-3j37?fbclid=IwAR0fl7xMjn_Hp6sImCU-EQt4gJ0ob_YY6hS3cwn4ARyClTUYD2KN0R6X-O0)

[[React 코드 까보기] useRef는 DOM에 접근할 때 뿐만 아니라 다양하게 응용할 수 있어요.](https://flyingsquirrel.medium.com/react-%EC%BD%94%EB%93%9C-%EA%B9%8C%EB%B3%B4%EA%B8%B0-useref%EB%8A%94-dom%EC%97%90-%EC%A0%91%EA%B7%BC%ED%95%A0-%EB%95%8C-%EB%BF%90%EB%A7%8C-%EC%95%84%EB%8B%88%EB%9D%BC-%EB%8B%A4%EC%96%91%ED%95%98%EA%B2%8C-%EC%9D%91%EC%9A%A9%ED%95%A0-%EC%88%98-%EC%9E%88%EC%96%B4%EC%9A%94-f0359ad23f3b)

## [useCallback, useMemo에 대해](https://ideveloper2.dev/blog/2019-06-14--when-to-use-memo-and-use-callback/)

useCallback이나 useMemo를 사용하여 최적화하는 것을 도대체 어느시점에 사용해야하는가에 대해 고민이 많다. 결론부터 말하자면 최적화를 함으로써의 이점이 더 클 때, 즉 측정해보고 필요한 시점에 하는 것이 이득이다.

useCallback이나 useMemo를 무분별하게 사용하면 코드의 복잡도를 상승시키고 dependecies array에서 실수를 하게되면 결국에 메모리를 낭비하므로 성능 퍼포먼스를 떨어뜨린다.

그럼 이제 언제 사용하는게 좋을까?

### 1. 참조 동일성(\***\*Referential equality)\*\***을 따져야 할 때

```jsx
function Foo({ bar, baz }) {
  const options = { bar, baz };
  React.useEffect(() => {
    buzz(options);
  }, [options]); // we want this to re-run if bar or baz change
  return <div>foobar</div>;
}
function Blub() {
  return <Foo bar="bar value" baz={3} />;
}
```

useEffect는 options에 대해 참조 체크를 매 렌더마다 하게 되고, 자바스크립트 동작방식때문에 options는 매 렌더마다 새롭게 만들어진다. 그러므로 리액트는 options는 참조 변수기 때문에 항상 변경되었다고 판단하고 결국에 bar 이나 baz가 변화할 때 useEffect callback이 실행 되는 것이 아닌 매 렌더마다 실행된다.

이것의 파훼법으로 두가지 방법이 있다.

```jsx
// option 1
function Foo({ bar, baz }) {
  React.useEffect(() => {
    const options = { bar, baz };
    buzz(options);
  }, [bar, baz]); // we want this to re-run if bar or baz change
  return <div>foobar</div>;
}
```

이 방법은 bar이나 baz가 객체타입(객체, 배열, 함수)이 아닐 경우에만 사용할 수 있는 방법이다.

아래와 같이 적용 가능!

```jsx
function Foo({ bar, baz }) {
  React.useEffect(() => {
    const options = { bar, baz };
    buzz(options);
  }, [bar, baz]);
  return <div>foobar</div>;
}
function Blub() {
  const bar = React.useCallback(() => {}, []);
  const baz = React.useMemo(() => [1, 2, 3], []);
  return <Foo bar={bar} baz={baz} />;
}
```

## 컴포넌트를 React.memo로 감싸서 불필요한 리렌더링 없애기

```jsx
function CountButton({ onClick, count }) {
  return <button onClick={onClick}>{count}</button>;
}
function DualCounter() {
  const [count1, setCount1] = React.useState(0);
  const increment1 = () => setCount1((c) => c + 1);
  const [count2, setCount2] = React.useState(0);
  const increment2 = () => setCount2((c) => c + 1);
  return (
    <>
      <CountButton count={count1} onClick={increment1} />
      <CountButton count={count2} onClick={increment2} />
    </>
  );
}
```

매번 버튼을 누를때 DualCounter의 상태가 변화하고 따라서 CountButton 컴포넌트들이 모두 리렌더링이 발생한다. 하지만 React.memo로 CountButton을 감쌀 경우 해당 버튼의 prop이 바뀔 때만 리렌더링이 일어난다.

```jsx
const CountButton = React.memo(function CountButton({ onClick, count }) {
  return <button onClick={onClick}>{count}</button>;
});
```

그러다 1번 사례에서 설명했다 싶이 DualCounter가 리렌더링 될 때 마다 리렌더링되는 increment1과 increment2는 함수로 정의했고 이러한 함수들은 매번 새롭게 만들어지고 리액트는 매번 새로운 값이 들어왔다 생각하고 리렌더링이 일어날 것 이다. 그럴때 useCallback이나 useMemo를 사용해

```jsx
const CountButton = React.memo(function CountButton({ onClick, count }) {
  return <button onClick={onClick}>{count}</button>;
});
function DualCounter() {
  const [count1, setCount1] = React.useState(0);
  const increment1 = React.useCallback(() => setCount1((c) => c + 1), []);
  const [count2, setCount2] = React.useState(0);
  const increment2 = React.useCallback(() => setCount2((c) => c + 1), []);
  return (
    <>
      <CountButton count={count1} onClick={increment1} />
      <CountButton count={count2} onClick={increment2} />
    </>
  );
}
```

참조 동일성을 따져주면 불필요한 리렌더링을 막을 수 있다.

## [3. 함수를 의존성 배열에 넣고 싶을때](https://www.rinae.dev/posts/a-complete-guide-to-useeffect-ko#%ED%95%98%EC%A7%80%EB%A7%8C-%EC%A0%80%EB%8A%94-%EC%9D%B4-%ED%95%A8%EC%88%98%EB%A5%BC-%EC%9D%B4%ED%8E%99%ED%8A%B8-%EC%95%88%EC%97%90-%EB%84%A3%EC%9D%84-%EC%88%98-%EC%97%86%EC%96%B4%EC%9A%94)

- async 함수를 useEffect안에서만 쓴다면 useEffect안에 넣어서 사용하면 의존성 걱정할 필요 x
- 하나의 async 함수를 여러 useEffect안에서 쓸 때는 이러한 동작 no no, async 함수 선언시 useCallback으로 감싸서 useEffect 의존성 배열에 집어넣으면 됨.

```jsx
function SearchResults() {
  // ✅ 여기 정의된 deps가 같다면 항등성을 유지한다
  const getFetchUrl = useCallback((query) => {
    return "https://hn.algolia.com/api/v1/search?query=" + query;
  }, []); // ✅ 콜백의 deps는 OK
  useEffect(() => {
    const url = getFetchUrl("react");
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); // ✅ 이펙트의 deps는 OK
  useEffect(() => {
    const url = getFetchUrl("redux");
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); // ✅ 이펙트의 deps는 OK
  // ...
}
```

## 정리

- 리액트는 레퍼런스 비교를 하기 때문에 props가 참조형이면 레퍼런스가 매번 달라지므로, useEffect등 의존성 배열에 의미가 없다.
- useMemo나 useCallback을 사용하여 참조 동일성을 지켜주면 useEffect에서 의존성 배열이 올바르게 동작한다.
- 최적화는 꼭 필요하다 느낄때만 하는 것이 좋다. 최적화에 드는 비용등을 생각했을 때 이득인 상황은 많지않다. 필요하다 간절히 느낄때만 ㄱㄱ
