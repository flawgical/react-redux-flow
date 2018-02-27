React REDUX

https://redux.js.org/basics/usage-with-react

Intro to redux

Redux is a state container for react and it is an attempt to do a flux flow for data and events. It is abstract but it is cool.

This is a simple application. For small applications redux is over kill. We are going to be learning the idea of how redux works and move forward with more complex redux patterns.

We are creating a single component increment click counter via a button.

Step 1: create-react-app clicker

Run it and make sure it’s pulling up on local host

Step 2: npm install redux. Redux is the library for state management.

then npm install react-redux (this is a binding library that glues react and redux)

Step 3: Create a new component called Counter.js

In here, let’s import react and write a function Counter

```
//pure functional component
import React from 'react'

function Counter(props) {
  return (
    <div>
      <h1> We be counting </h1>
    </div>
  )
}

export default Counter;

```
Step 4: Go to app.js and import Counter from ./counter
and add it in the div <Counter />

Check to see if everything is rendering.

Step 5: Event handler and button

Counter.js

```
//pure functional component
import React from 'react'

function Counter(props) {
  return (
    <div>
      <h1> We be counting </h1>
      <p> Count: {props.count} </p>
      <button> increment </button>
    </div>
  )
}

export default Counter;

```

Step 6: Go to app.js and pass a value in since there is no value.

app.js

```
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
import Counter from './Counter'

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
          <Counter count="5" />
      </div>
    );
  }
}

export default App;

```

## Now let’s talk redux.

Whenever we click on the button we are going to create an ACTION. Then we will DISPATCH that action to a STORE and the STORE will run through the REDUCER. That will change the STATE and the component will re Render.

Step 7: We are going to import react redux now.

import { connect } from ‘react-redux’ in Counter.js

Step 8: Now we will create a function that will map the state to the props.

Counter.js

```
//pure functional component
import React from 'react'
import {connect}  from 'react-redux'

function Counter(props) {
  return (
    <div>
      <h1> We be counting </h1>
      <p> Count: {props.count} </p>
      <button> increment </button>
    </div>
  )
}

function mapStateToProps(state) {
  console.log('mapStateToProps', state);
  return {
    count: state.count
    //count will be passed into props on line 5
  }
}

export default Counter;
```

Step 9: Now we need to use the connect function we imported

we are going to connect mapStateToProps to the component.

Creating a new component and wrapping our component in export default line

```
function mapStateToProps(state) {
  console.log('mapStateToProps', state);
  return {
    count: state.count
    //count will be passed into props on line 5
  }
}

export default connect(mapStateToProps)(Counter) ;
```

Will now get an error. Cannot find “store” …

Step 10: Make the STORE

Create a store folder and in it create index.js

In index.js
import { createStore } from ‘redux’ ;

Now we need to define what the state of our application is

AND then we need to create a reducer function that takes 2 arguments. A State and an Action. Then create a Store

A basic shell for a store

store/index.js

```
import {createStore} from 'redux'

const initialState = {
  count: 3
}

const reducer = (state = initialState, action) => {
  console.log('reducer running', action);
  return state;
}

const store = createStore(reducer);

export defaul store;

```

Step 11: Now import store in App.js

In order to solve the error we go in App.js and change the count=‘5’ to  store = {store}

app.js

```
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
import Counter from './Counter';
import store from './store';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
          <Counter store={store} />

      </div>
    );
  }
}

export default App;

```


Step 12: So what we have defined our store but have not created an action to dispatch.

Now we will create a new function in Counter.js to dispatch the action .

We will create an onClick in the button and create a onIncrementClick

```
import React from 'react'
import {connect}  from 'react-redux'

function Counter(props) {
  console.log('render', props);
  return (
    <div>
      <h1> We be counting </h1>
      <p> Count: {props.count} </p>
      <button onClick = {props.onIncrementClick}>increment </button>
    </div>
  )
}

function mapStateToProps(state) {
  console.log('mapStateToProps', state);
  return {
    count: state.count
    //count will be passed into props on line 5
  }
}

function mapDispatchToProps(dispatch){
  return {
    onIncrementClick: () => {
      console.log('clicking');
    }
  }
}

export default connect(mapStateToProps,mapDispatchToProps)(Counter) ;

```


Notice that we added mapDispatchToProps in the connect mix in the export default.

Step 13: Now we create an action. in redux actions are objects.
So let’s create one.

We will define an action and give it a type.

```
//pure functional component
import React from 'react'
import {connect}  from 'react-redux'

function Counter(props) {
  console.log('render', props);
  return (
    <div>
      <h1> We be counting </h1>
      <p> Count: {props.count} </p>
      <button onClick = {props.onIncrementClick}>increment </button>
    </div>
  )
}

function mapStateToProps(state) {
  console.log('mapStateToProps', state);
  return {
    count: state.count
    //count will be passed into props on line 5
  }
}

function mapDispatchToProps(dispatch){
  return {
    onIncrementClick: () => {
      console.log('clicking');
      const action = { type: 'INCREMENT'};
      dispatch(action);
    }
  }
}

export default connect(mapStateToProps,mapDispatchToProps)(Counter) ;

```

Now the reducer is running. Check console.log in inspector.

Step 14: Now that we have an action, how do we close the circle of flow.

We use switch statements.
in it we will use Object.assign that will copy all the values in an empty object.

Start with a brand new object, copy all the values out of the state into the brand new object, and then overwrite it with state.count +1.

store/index.js

```
import {createStore} from 'redux'

const initialState = {
  count: 3
}

const reducer = (state = initialState, action) => {
  console.log('reducer running', action);

  switch (action.type) {
    case 'INCREMENT':
    return Object.assign({}, state, { count: state.count + 1} );
    default:
    return state;
  }

  return state;
}

const store = createStore(reducer);

export default store;

```
