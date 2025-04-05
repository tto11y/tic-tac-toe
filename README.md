# tic-tac-toe

this is a demo app that leverages React

----

# General topics

## React components

In React, a component is a piece of reusable code that represents a part of a user interface. Components are used to render, manage, and update the UI elements in your application.

e.g.
```typescript jsx
/** default keyword tells other files using your code that it’s the main function in your file */
export default function Square() {
        
    /** <button> is a JSX element.
     A JSX element is a combination of JavaScript code and HTML tags that describes what you’d like to display.
     
     className="square" is a button property or prop that tells CSS how to style the button
     */
    return <button className="square">X</button>;
}
```

### index.js

The file index.js is the bridge between the component you created in the App.js file and the web browser.

e.g.
```typescript jsx
/**
 Lines 1-5 bring all the necessary pieces together:
     
 1 React
 2 React’s library to talk to web browsers (React DOM)
 3 the styles for your components
 4 the component you created in App.js.
 */

import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import './styles.css';
import App from './App';

/** The remainder of the file brings all the pieces together and injects the final product into index.html in the public folder. */
const root = createRoot(document.getElementById("root"));
root.render(
    <StrictMode>
        <App />
    </StrictMode>
);
```

### Passing data through Props

Variables that are passed into components are called __Props__. This can be done like follows:

```typescript jsx
/** the component 'Square' is passed the prop 'value' */
const Square = ({ value }) => {
    return <button className="square">{ value }</button>;
};
```

### Adding state to components

To “remember” things, components use state.

React provides a special function called _useState_ that you can call from your component to let it “remember” things.

e.g. Let’s store the current value of the Square in state, and change it when the Square is clicked.

```typescript jsx
import { useState } from 'react';

const Square = () => {
    
    /**
     value stores the value and setValue is a function that can be used to change the value. 
     The null passed to useState is used as the initial value for this state variable, so value here starts off equal to null.
     */
    const [value, setValue] = useState(null);

    function handleClick() {
        /** By calling this set function from an onClick handler, you’re telling React to re-render that Square whenever its <button> is clicked */
        setValue('X');
        //...
    }
}
```

### Sharing state between components / Lifting state up

> To collect data from multiple children, or to have two child components communicate with each other, _declare_ the _shared state in their parent component_ instead. 
> The parent component can pass that state back down to the children via props. 
> This keeps the child components in sync with each other and with their parent.

e.g.
At first, you might guess that the Board needs to “ask” each Square for that Square’s state. 
Although this approach is technically possible in React, we discourage it because the code becomes difficult to understand, susceptible to bugs, and hard to refactor.
Instead, the _best approach_ is to store the game’s state in the parent Board component instead of in each Square._ The Board component can tell each Square what to display by passing a prop, like you did when you passed a number to each Square.

## Naming conventions

In React, it’s conventional to use onSomething names for props which represent events and handleSomething for the function definitions which handle those events.

## Why immutability is important 

There are generally two approaches to changing data. 
1. to __mutate__ the data by directly changing the data’s values. 
2. to __replace__ the data with a new copy which has the desired changes.

```typescript
// mutating directly

const squares = [null, null, null, null, null, null, null, null, null];
squares[0] = 'X';
// Now `squares` is ["X", null, null, null, null, null, null, null, null];
```

```typescript
// replacing with a copy

const squares = [null, null, null, null, null, null, null, null, null];
const nextSquares = ['X', null, null, null, null, null, null, null, null];
// Now `squares` is unchanged, but `nextSquares` first element is 'X' rather than `null`
```

The result is the same but by not mutating (changing the underlying data) directly, you gain several benefits.

1. Immutability makes complex features much easier to implement. 
   - Avoiding direct data mutation lets you keep previous versions of the data intact, and reuse them later.
2. Immutability makes it very cheap for components to compare whether their data has changed or not. 
   - By default, all child components re-render automatically when the state of a parent component changes. This includes even the child components that weren’t affected by the change. 
   - Although re-rendering is not by itself noticeable to the user (you shouldn’t actively try to avoid it!), you might want to skip re-rendering a part of the tree that clearly wasn’t affected by it for performance reasons.

## Rendering multiple items in React

To render multiple items in React, you can use an array of React elements.

```typescript jsx
const moves = history.map((squares, move) => {
        let description;
        if (move > 0) {
            description = 'Go to move #' + move;
        } else {
            description = 'Go to game start';
        }
        return (
            <li key={move}>
                <button onClick={() => jumpTo(move)}>{description}</button>
            </li>
        );
    });
```

When you render a list, React stores some information about each rendered list item. 
When you update a list, React needs to determine what has changed. You could have added, removed, re-arranged, or updated the list’s items.

You need to specify a key property for each list item to differentiate each list item from its siblings.
For example, if your data was from a database, database IDs could be used as keys.

React determines changes in lists as follows:

When a list is re-rendered, React takes each list item’s key and searches the previous list’s items for a matching key.

1. If the current list has a key that didn’t exist before, React creates a component. 
2. If the current list is missing a key that existed in the previous list, React destroys the previous component. 
3. If two keys match, the corresponding component is moved.

Keys tell React about the identity of each component, which allows React to maintain state between re-renders. If a component’s key changes, the component will be destroyed and re-created with a new state.

NOTE: _key_ is a special and reserved property in React. When an element is created, React extracts the key property and stores the key directly on the returned element. Even though key may look like it is passed as props, React automatically uses key to decide which components to update. There’s no way for a component to ask what key its parent specified.

__It’s strongly recommended that you assign proper keys whenever you build dynamic lists.__ If you don’t have an appropriate key, you may want to consider restructuring your data so that you do.

ATTENTION: If no key is specified, React will report an error and use the array index as a key by default. __Using the array index as a key is problematic__ when trying to re-order a list’s items or inserting/removing list items. Explicitly passing key={i} silences the error but has the same problems as array indices and is not recommended in most cases.

__Keys do not need to be globally unique; they only need to be unique between components and their siblings.__

# Wrapping up
this demo app includes the following features

1. Lets you play tic-tac-toe
2. Indicates when a player has won the game
3. Stores a game’s history as a game progresses
4. Allows players to review a game’s history and see previous versions of a game’s board