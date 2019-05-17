# Next Generation JavaScript:

## Let and Const
- `let` - use for variable values
- `const` - use for constant values

```js
const myName = 'Max';
console.log(myName)

myName = 'Manu'; // THROWS ERROR due to reassignment
console.log(myName);
```

## Arrow Functions

```js
// Usual way of using a function
function printMyName(name) {
  console.log(name)
}
printMyName("Max");


// ES 6 version allows storing a function within a variable, which can then be used.
const printMyName2 = (name) => {
  console.log(name);
}
printMyName2("Max");
```

### Syntactic Sugar
```js
// ------------------------------------------------------------------------------------------------
// if there's only one argument, brackets are not needed for it
const printMyName2 = name => {
  console.log(name);
}

// ------------------------------------------------------------------------------------------------
// if the function doesn't have any arguments, then empty brackets need to be used
const printMyName2 = () => {
  console.log('Max');
}

// ------------------------------------------------------------------------------------------------
// multiple arguments require brackets
const printMyName2 = (name, age) => {
  console.log(name, age);
}

// ------------------------------------------------------------------------------------------------
// If a function returns a value as its only line, the whole function can be written on one line:

// -- OLD --
const multiply = (number) => {
  return number * 2;
}

// -- NEW --
const multiply = number => number * 2

console.log(multiply(2))
```

## Classes
- Classes contain:
  - Properties: which are like variables
  - Methods: which are like functions

```js
class Human {
  constructor() {
    this.gender = "male"
  }

  printGender() {
    console.log(this.gender)
  }
}

class Person extends Human{
  constructor() {
    super() // initialises parent class, without which parent properties will not be available and will throw an error if attempting to access
    this.name = "Max"
    this.gender = "female"
  }

  printMyName() {
    console.log(this.name)
  }
}

const person = new Person()
person.printMyName()
person.printGender()
```

## More Classes
- properties can now be defined without wrapping inside of a constructor
- super classes are initialised implicitly
- methods can now use arrow syntax instead

```js
class Human {
  gender = "male"

  printGender = () => {
    console.log(this.gender)
  }
}

class Person extends Human{
  name = "Max"
  gender = "female"

  printMyName = () => {
    console.log(this.name)
  }
}

const person = new Person()
person.printMyName()
person.printGender()
```

## Spread & Rest Operators

- Spread operator: is used to unpack array elements, e.g: `const newArray = [...oldArray, 1, 2]` -> appends oldArray with new elements
```js
const numbers = [1,2,3]
const newNumbers = [...numbers, 4,5]
console.log(newNumbers) // OUTPUT: [1,2,3,4,5]

// USE 2
const person = {
  name: "Max"
}

const newPerson = {
  ...person,
  age: 28
}

console.log(newPerson)
/* OUTPUT:
[object Object] {
  age: 28,
  name: "Max"
}
*/
```

- Rest operator: acts like varargs in java and allows a function to take a variable number of arguments and merge them into an array

```js
const filter = (...args) => {
  return args.filter(element => element === 1);
}

console.log(filter(1,2,3)) // OUTPUT: [1]
```

## Destructuring:
- Array Destructuring: similar to tuple unpacking in Python:
```js
const numbers = [1,2,3];
[num1, num2] = numbers;

console.log(num1, num2); // OUTPUT: 1
                         //         2
```

- Object Destructuring: allows extracting certain properties of an object
```js
{name} = {name: "Max", age: 28}
console.log(name) // OUTPUT: Max
console.log(age) // OUTPUT: undefined

// age is undefined since the variable doesn't exist
```

## Reference and Primitive Types:
```js
const num1 = 1; // is a primitive type
const num2 = num1; // is a copy of the VALUE of 'num1'


const person = {
  name: 'Max'
};
const secondPerson = person; // is a copy of the REFERENCE of 'person'

// This creates a real copy of the value of 'person' and not just a copy of the pointer to it
const realPersonCopy = {
  ...person;
};
```

## Array Functions
```js
const numbers = [1,2,3];
// returns a new array of elements (num * 2)
const doubleNumArray = numbers.map((num) => {
  return num * 2;
});

console.log(doubleNumArray) // OUTPUT: [2, 4, 6]
```


# Base Features and Syntax

## Component Basics
- npm creates a project structure for the app. This includes a `public` folder which contains a <div> for id="root".
- `src/index.js` contains `ReactDOM.render(<App />, document.getElementById('root'))`
- This points to `src/App.js` and will render the React code we enter within it

- A component, in its simplest form, is just some javascript that returns jsx (i.e. some html)

## Event listeners:
https://reactjs.org/docs/events.html#supported-events


## State and Props
- If state or probs change, they update the dom wherever it needs to be updated.
- State and props are the only two things that will update the dom in React

# Styling
- Using CSS modules allows the css selectors adding within a .css file to apply only where that file is imported. I.e. they're no longer global

```js
// before CSS modules
import './App.css';
<div className="App"></div>

// after CSS modules
import classes from './App.css';
<div className={classes.App}></div>
```

- To still use a CSS selector globally, it can simply be defined with the prefix `:global`
```
:global .Post {
  ...
}
```

# Error handling
- Error Bounderies can be used to catch exceptions that may occur within an app, and handle them elegantly
- These, as usual, should be used sensibly and only in place where the app has a chance to unexpectedly break

# Good Project Structure
- App.js (or similar container files) are there to control state and contain the handlers used for controlling this state. They however shouldn't be for component logic
- Container files should be as lean as possible with minimal use of styling and jsx
- Components should be functional whenever possible (i.e. not in a class where they are allowed to have their own state objects)
- Individual components should:
  - have clear responsibilities
  - be narrowly focused
  - be functional where possible

# Class-based vs Functional Components:
- class-based:
  - any component that extends `Component`
  - e.g. `class XY extends Component`
  - Access to State
  - Have access to Lifecycle Hooks
  - Access State and Probs via `this`

- Functional:
  - e.g. `const XY = props => {...}`
  - Access to State (useState())
  - Do not have access to Lifecycle Hooks
  - Access Props via `props`


- Use *class-based* if:
  - need to manage State
  - access to Lifecycle Hooks (and don't want to use React Hooks)
- Use *Functional* in all other cases

## Class-base Components Lifecycle Hooks
- These are a list of Lifecycle Hooks (i.e. methods that run implicitly during the course of a class component) in order of when they are run (started from first-to-run)
- *constructor()*
  - should be used to setup some initial state
- *render()*
  - prepare and structure your jsx code
  - Also all child components included within render are executed here (and their lifecycle hooks are played out)
- *getDerivedStateFromProps()*
  - for syncing state when props change
  - is quite a niche hook and isn't used very much
- *componentDidMount()*
  - Typical hook used for causing side-effects and making web requests
  - State SHOULD NOT be updated here


- Others (Though not sure which order they appear in..):
  - getSnapshotBeforeUpdate()
  - componentDidCatch()
  - componentWillUnmount()
  - shouldComponentUpdate()
  - componentDidUpdate()

## Component Lifecycle Hooks - On Update
- These lifecycle hooks are used when either props or state are updated

- *getDerivedStateFromProps(props,state)*
  - For syncing state to Props
  - is a niche hook and isn't used too much
- *shouldComponentUpdate(nextProps, nextState)*
  - can decide whether or not to continue re-rendering for components
  - can be used for performance by fine-tuning what needs to be re-rendered
- *render()*
  - prepare and structure your jsx code
  - Also all child components included within render are executed here (and their lifecycle hooks are played out)
- *getSnapshotBeforeUpdate(prevProps, prevState)*
  - takes previous props and previous state as input and returns a snapshot object that can be freely configured
  - is a niche hook and isn't used too much
  - used for last-minute Dom modifications (e.g. resetting a user's scroll position to before the update happened)
- *componentDidUpdate()*
  - signals that the component is done with updating
  - can cause side-effects once again inside of this

## React Hooks
- `useEffect` is the most important hook used after `setState`
- `useEffect` is a React Hook (not related to Lifecycle hook) which allows functional components to use all the Lifecycle hooks available within class components, all within just `useEffect`
- `useEffect` is a combination of `componentDidMount` and `componentDidUpdate` in one

- Since `useEffect` may end up running to often due to it being a combination of `componentDidMount` and `componentDidUpdate`, we can configure it so it only runs when needed,
- e.g: the following `useEffect` is passed in a dependency of `props.person`, so will only run if `props.person` is changed
```js
const Cockpit = (props) => {
    useEffect(() => {
        console.log('[Cockpit.js] useEffect');
        setTimeout(() => {
            alert('Saved data to cloud!');
        }, 1000)
    }, [props.persons]);
}
```

- e.g2: the following `useEffect` will only run once since an empty list of dependencies is passed in
- const Cockpit = (props) => {
    useEffect(() => {
        console.log('[Cockpit.js] useEffect');
        setTimeout(() => {
            alert('Saved data to cloud!');
        }, 1000)
    }, []);
}

## Clean-up:
- For class-based components, use *componentWillUnmount()*:
  - is run just before a component is removed from the DOM

- For functional components, `useEffect` can be modified to provide a clean-up sequence through adding an anonymous function:
```js
useEffect(() => {
    console.log('[Cockpit.js] useEffect');
    setTimeout(() => {
        alert('Saved data to cloud!');
    }, 1000);
    return () => {
        console.log('[Cockpit.js] cleanup work in useEffect');
    };
}, [props.persons]);
```

## Optimisation

- For class components:
  - `shouldComponentUpdate()` can be used for fine-tuning performance by ensuring we only render the components that need to be rendered (i.e. only the ones that are being updated and not ones that aren't)
  - React works by having a virtual DOM to which our changes are committed to. After changes, React compares the virtual DOM to the real DOM and checks if there's been any difference. React would then render any changes that occur between the two DOM's
  - As a result, React already handles performance issues caused by our code attempting to re-render things that haven't changed. However these methods are still called before committed to the virtual DOM, therefore it's best to handle them anyway.

- For functional components:
  - we can wrap the export of a functional component with `React.memo()` which caches the component. If the component hasn't changed, the cached component is returned instead of re-rendering it.

### PureComponent
- PureComponent can be imported and extended for a class (instead of Component) if wanting a `shouldComponentUpdate` method which checks for all possible props for which the component should be updated for, all out-the-box.

# How React updates the DOM:
- Since updating the real DOM in the browser is very expensive, React does this as sparingly as possible.
- React uses two virtual DOMs: an old Virtual DOM and a newer 'Re-rendered' DOM. A comparison is made between these two virtual DOMs and the Old Virtual DOM is updated if needed (this is fast in comparison to updating the read DOM)
- The difference is then compared to the real DOM, where another comparison is made. If there is something to update in the real DOM, only that is updated.
![](./notes_images/react_rendering_to_DOM.PNG)


# Higher Order Components
- HoC's can be used to wrap components to then add additional logic to them
- By convention HoC classes are prefixed with `With`, e.g. `WithClasses.js`