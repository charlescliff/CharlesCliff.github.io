---
title: "React Ref"
date: 2022-10-01T22:39:33+08:00
draft: false 
categories: 
- technology
- frontend
- react
tags: 
- react
- react.ref
- javascript
showToc: true
TocOpen: false
---

React Ref is a relative high level skill in React. It's easy to pass data from parent to child through props, but sometimes child may want to expose something to be used in parent, and this requirement is more common in designing a common component which may want to expose data/functionality to parent.  
Today we are going to talk about the common usage of ref in react, including the basic usage of accessing DOM elements, exposing function/data to parent, and data reference to pass around.

## Rules of Thumb

* You can store information between re-renders (unlike regular variables, which reset on every render).
* Changing it does not trigger a re-render (unlike state variables, which trigger a re-render).
* The information is local to each copy of your component (unlike the variables outside, which are shared). Changing a ref does not trigger a re-render
* Don't change Ref or read Ref during render. use it in a callback function or hooks like useEffect.
* Regular function or class components don’t receive the ref argument, and ref is not available in props either.

## Manipulating DOM elements

Using Ref to access the dom element in react component is the most common and basic usage.

```javascript
function TextInput() {
 const inputRef = React.useRef();
 const handleChange = () => {
  inputRef.current.focus();
 }
 return <input ref={inputRef} onChange={handleChange} />;
}
```


## Ref with Class Component

use React.createRef in class component to initalize a ref and pass it to child to obtain the instance.

```javascript
class MyComponent extends React.Component {
    constructor() {
        super(this.props);
        this.inputRef = React.createRef();
    }
    render() {
        return <Input ref={this.inputRef} />
    }
}
```

## Ref with Function Component

function component doesn't support ref because it doesn't have a instance like class component. React.forwardRef hoc is intended for this case. 

```javascript
function TextInput() {
 const inputRef = React.useRef();
 const handleChange = () => {
  inputRef.current.focus();
 }
 return <input ref={inputRef} onChange={handleChange} />;
}

const TextInputWithRef = React.forwardRef(TextInput);

function ParentComponent() {
 const inputRef = React.useRef();
 return <TextInputWithRef ref={inputRef} />;
}

```

## useImperativeHandle

```javascript
const ChildComponent = React.forwardRef((props, ref) => {
 const inputRef = React.useRef();
 React.useImperativeHandle(ref, () => {
   setFocus: () => inputRef.current.focus();
 });
 const handleChange = () => {
  inputRef.current.focus();
 }
 return <input ref={inputRef} onChange={handleChange} />;
});


function ParentComponent() {
 const inputRef = React.useRef();
 const handleClick = () => {
  inputRef.current.setFocus();
 }
 return <ChildComponent ref={inputRef} />;
}

```

## Callback Ref

React also supports another way to set refs called “callback refs”, which gives more fine-grain control over when refs are set and unset. Instead of passing a ref attribute created by createRef(), you pass a function. The function receives the React component instance or HTML DOM element as its argument, which can be stored and accessed elsewhere. This is useful if you want to do something when ref is set.

## When is the Ref.current set?

React will assign the current property with the DOM element when the component mounts, and assign it back to null when it unmounts. ref updates happen before componentDidMount or componentDidUpdate lifecycle methods.

## References
[useRef](https://beta.reactjs.org/apis/react/useRef)  
[Refs and the DOM](https://reactjs.org/docs/refs-and-the-dom.html)  
[forwarding refs](https://reactjs.org/docs/forwarding-refs.html)  
