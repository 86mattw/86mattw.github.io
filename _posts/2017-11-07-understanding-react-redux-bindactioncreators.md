---
title: Understanding react-redux bindActionCreators
description: Breaking down the bindActionCreators method from react-redux
tags: react redux
---

The bindActionCreators method "turns an object whose values are action creators, into an object with the same keys, 
but with every action creator wrapped into a dispatch call so they may be invoked directly".

This is useful when you have one or more action creators which you want to be able to call from your component.

The method is typically used when constructing a mapDispatchToProps function to be passed to the connect method.

Assuming you have an action creator called addTodo, the following code would make that function available as a prop of 
your component, only wrapped in a Redux dispatch call.

```js
const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({ addTodo }, dispatch);
}

export default connect(null, mapDispatchToProps)(TodoList);
// results in this.props.addTodo() being available to TodoList.
```

The function above is the equivalent of writing:

```js
const mapDispatchToProps = (dispatch) => {
  return {
    addTodo: () => {
      dispatch(addTodo());
    }
  }
}
```

So when the component calls `this.props.addTodo()` it is executing the action creator, which returns an action, which 
is then in turn dispatched to the store by Redux.

### mapDispatchToProps shorthand

The connect method accepts either a function or an object as the mapDispatchToProps argument. The API states that when
an object is provided it does the following:

"If an object is passed, each function inside it is assumed to be a Redux action creator. An object with the same 
function names, but with every action creator wrapped into a dispatch call so they may be invoked directly, will be 
merged into the component’s props."

This is exactly what we are trying to achieve in the first example, so it can be re-written as follows, removing the 
need for an explicit mapDispatchToProps function.

```js
export default connect(null, { addTodo })(TodoList);
```

Note, I'm using ES6 object literal shorthand in the above examples to reduce typing an improve readability.