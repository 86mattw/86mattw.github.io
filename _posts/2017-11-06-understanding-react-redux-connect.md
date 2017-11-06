---
title: Understanding react-redux connect
description: Breaking down the connect method from react-redux
tags: react redux
---

The API documentation tells us that the connect method "Connects a React component to a Redux store".

It took me a while to get my head around what this actually meant. A really helpful resource was Dan Abramov's 
[Getting Started with Redux](https://egghead.io/courses/getting-started-with-redux) course which does a great job of
introducing you to the connect method and the surrounding concepts.

The Redux store can be implicitly passed down to your components via the context. The `<Provider />` component, which is
also given to us by react-redux, takes care of this.

We can then write container components which pull the store off the context in order to then access state, dispatch 
actions and subscribe to changes. This requires quite a lot of boilerplate code though, as show below in an arbitrary 
example:

```js
class MyComponent extends Components {
  componentDidMount() {
    const { store } = this.context;
    this.unsubscribe = store.subscribe(() => {
      this.forceUpdate();
    });
  }

  componentWillUnmount() {
    this.unsubscribe();
  }

  render() {
    const { store } = this.context;
    const { state } = store.getState();

    return (
      <div>
        { state.message }
        <button onClick={() => {
          store.dispatch({
            type: 'UPDATE_MESSAGE',
            message: 'Hello world'
          });
        }}>
          Update
        </button>
      </div>
    );
  }
}
```

The connect method does this hard work for us by generating container components which are hooked up to the Redux 
store.

It typically takes two arguments, called by convention mapStateToProps and mapDispatchToProps. The former ensures that
your component can access the parts of the state that it needs. The latter enables your component to dispatch actions
so that the state can be updated.

```js
class MyComponent extends Component {
  render() {
    return (
      <div>
        { this.props.message }
        <button onClick={() => {
          this.props.onUpdateClick();
        }}>
          Update
        </button>
      </div>
    )
  }
}

const mapStateToProps = (state) => {
  return {
    message: state.message
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onUpdateClick: () => {
      dispatch({
        type: 'UPDATE_MESSAGE',
        message: 'Hello world'
      });
    }
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(MyComponent);
```

Note that here I'm dispatching the action directly and not using an action creator in order to try and keep the example 
as concise as possible.

Using the connect method we now have access to the `message` property of the state and the `onUpdateClick` dispatch 
function as props of the component.
