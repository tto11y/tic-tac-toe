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

