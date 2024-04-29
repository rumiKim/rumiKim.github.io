---
layout: post
tags: [React]
---

> 
1. React.ReactElement 란
2. JSX.Element 란
3. React.ReactNode 란
4. React.ReactElement vs React.ReactNode vs JSX.Element

## 1. React.ReactElement
- React.createElement 로 생성된 컴포넌트만을 받는다.
- React가 렌더링하는 HTM element, 사용자 정의 컴포넌트 등과 같은 특정 노드의 타입이며, 
  이는 type, props, key 등을 갖는다.

```javascript
  interface ReactElement< P = any, T extends string 
  | JSXElementConstructor<any> = string | JSXElementConstructor<any>,
  > {
      type: T;
      props: P;
      key: string | null;
    }
    /*
    * @template P The type of the props object
    * @template T The type of the component or tag
    * // ex
    * const element: ReactElement = <div />;
    */
```

## 2. JSX.Element

- 함수형 컴포넌트의 리턴타입.
- `ReactElement`의 타입을 any로 확장한 인터페이스
- `global` `namespace` 에 선언되어 있어 파일 내부에서 `import` 하지않고 바로 사용할 수 있다.

```javascript
declare global {
  namespace JSX {
    interface Element extends React.ReactElement<any, any> {}
  }
}
```

## 3. React.ReactNode
참고 사이트 : https://react-typescript-cheatsheet.netlify.app/docs/react-types/reactnode/
- `ReactNode`는 렌더링할 컴포넌트의 타입을 말하며, 주로 `children`을 `props`로 받아 타입을 명시할 때 사용한다.
- `ReactElement` 는 JSX 만을 나타내는데 반해, `ReactNode`는 `ReactElement`를 포함한 대부분의 타입을 아우른다. (그래서 타입이 불분명한 경우나, custom component에서 주로 사용하는듯하다.)

```javascript
  type ReactNode =
    | ReactElement
    | string
    | number
    | Iterable<ReactNode>
    | ReactPortal
    | boolean
    | null
    | undefined
    | DO_NOT_USE_OR_YOU_WILL_BE_FIRED_EXPERIMENTAL_REACT_NODES[
        keyof DO_NOT_USE_OR_YOU_WILL_BE_FIRED_EXPERIMENTAL_REACT_NODES
    ];
  ```

## 4. React.ReactElement vs React.ReactNode
- 결론적으로 `React.ReactNode` > `React.ReactElement` > `JSX.Element` 의 관계성을 띄고 있다.
- ReactElement와 ReactNode 차이의 핵심은 'level of specificity'이다.
  - single React element 를 리턴하는 함수의 리턴타입으로는 ReactElement를 사용
  - React가 렌더링시키는 어떠한 타입의 노드라도 받을 것을 명시할 경우에는 ReactNode를 사용

```javascript
function MyComponent(): React.ReactElement {
  return <div>Hello, world!</div>;
}

function MyFlexibleComponent(): React.ReactNode {
  return this.props.shouldRender ? <div>Hello, world!</div> : null;
}
```




