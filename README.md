# Fluxy - yet another state management library
Using React Hooks and Context API.

## Usage example

Creating initial store:
```javascript
let initialStore = {
	counter: 0,
	power: true
}
````

Creating reducers and adding them to the root reducer:
```javascript
let counterReducer = (state, {type} = {}) => {
	switch(type) {
		case 'INCREMENT':
			return state + 1;

		default:
			return state;
	}
}

let powerReducer = (state, {type} = {}) => {
	switch(type) {
		case 'TOGGLE':
			return !state;

		default:
			return state;
	}
}

let rootReducer = ({counter, power}, action) => ({
	counter: counterReducer(counter, action),
	power: powerReducer(power, action)
});
```

Creating the main app:

```javascript
import StoreProvider from 'fluxy';

const App = () => (
	<StoreProvider initialStore={initialStore} rootReducer={rootReducer}>
		<Counter />
		<Power />
	</StoreProvider>
);
```

Creating sub-components and connecting them to the store:
```javascript
import {connect} from 'fluxy';

const Counter = connect(({counter}) => ({counter}))(({counter, dispatch}) => (
	<button onClick={() => dispatch({type: 'INCREMENT'})}>
		{counter}
	</button>
));
```

```javascript
import {connect} from 'fluxy';

let mapStateToProps = ({power}) => ({power}),
	mapDispatchToProps = dispatch => ({
		toggleState: () => dispatch({type: 'TOGGLE'})
	});

const Power = connect(mapStateToProps, mapDispatchToProps)(({power, toggleState}) => (
	<button onClick={toggleState}>
		{power ? 'ON' : 'OFF'}
	</button>
));
```