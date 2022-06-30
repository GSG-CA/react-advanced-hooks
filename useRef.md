# useRef Hook
we’ll cover what the `useRef` Hook is, what we can use it for and some advice for using it.

`useRef` returns a mutable ref object whose `.current` property is  initialized with the passed argument (initialValue). The returned object will persist for the full lifetime of the component.


```js
    const refContainer = useRef(initialValue);
```

Essentially, `useRef` is like a “box” that can hold a mutable value in its `.current` property.
There are two main uses of `useRef` that are explained in the following sections: 
- Accessing the DOM nodes or React elements 
-  keeping a mutable variable.



### Accessing the DOM nodes or React elements
You might be familiar with refs primarily as a way to access the DOM. If you pass a ref object to React with `<div ref={myRef} />`, React will set its `.current` property to the corresponding DOM node whenever that node changes.

If you’ve worked with React for a while, you might be used to using refs for this purpose. Below there’s an example of the use of refs in class components:

```js
import React, { Component, createRef } from "react";

class CustomTextInput extends Component {
  textInput = createRef();

  focusTextInput = () => this.textInput.current.focus();

  render() {
    return (
      <>
        <input type="text" ref={this.textInput} />
        <button onClick={this.focusTextInput}>Focus the text input</button>
      </>
    );
  }
}
```

And its equivalent using a functional component:


```js
import React, { useRef } from "react";

const CustomTextInput = () => {
  const textInput = useRef();

  focusTextInput = () => textInput.current.focus();

  return (
    <>
      <input type="text" ref={textInput} />
      <button onClick={focusTextInput}>Focus the text input</button>
    </>
  );
}
```
‍Notice that with a functional component we are using `useRef` instead of `createRef`. If you create a ref using `createRef` in a functional component, React will create a new instance of the ref on every re-render instead of keeping it between renders.

### Keeping a mutable variable
Yes! The useRef() Hook isn’t just for DOM refs. The “ref” object is a generic container whose current property is mutable and can hold any value, similar to an instance property on a class.

You can write to it from inside useEffect:
```js
function Timer() {
  const intervalRef = useRef();

  useEffect(() => {
    const id = setInterval(() => {
      // ...
    });
    intervalRef.current = id;
    return () => {
      clearInterval(intervalRef.current);
    };
  });

  // ...
}
```
If we just wanted to set an interval, we wouldn’t need the ref (id could be local to the effect), but it’s useful if we want to clear the interval from an event handler:

```js
  // ...
  function handleCancelClick() {
    clearInterval(intervalRef.current);
  }
  // ...
```
Conceptually, you can think of refs as similar to instance variables in a class. Unless you’re doing lazy initialization, avoid setting refs during rendering — this can lead to surprising behavior. Instead, typically you want to modify refs in event handlers and effects.

    
    
### Resources
- [React useRef hook medium](https://medium.com/trabe/react-useref-hook-b6c9d39e2022)
- [useRef React doc](https://reactjs.org/docs/hooks-reference.html#useref)   
- [useRef as instance variables](https://reactjs.org/docs/hooks-faq.html#is-there-something-like-instance-variables)
    
