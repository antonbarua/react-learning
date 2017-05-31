# React Reference

## Basic concepts

* **JSX**

    JSX produces React **elements**.


    ```javascript
    function add(a, b){
        return a + b;
    }

    const calc = (
        <span>{add(1,2)}</span> //JSX
    );

    ReactDOM.render(
        calc,
        document.getElementById("root")
    );
    ```
    After compilation, JSX expressions become regular JavaScript objects.

* **Elements**

    Applications built with React usually have a root DOM node. 
    

    ```HTML
    <div id="root"></div>
    ```

    To render a React element into a root DOM node, pass it to `ReactDOM.render()`.

    ```javascript
    const element = <h1>Hello, world!</h1>
    ReactDOM.render(element, document.getElementById('root'));
    ```

    React elements are immutable. Once you create an element, you can't change its children or attributes. The only way to update the UI is to create a new element.
    **React DOM compares the new element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.**


* **Components**

    Components are the building blocks of UI. Components split the UI into reusable pieces.

    A component takes in parameters, called `props`, and returns a hierarchy of views to display.

    Components can be implemented as a function, or as ES6 classs. Class-based components must implement the `render()` method. The `render()` method returns a description of what you want to render, and then React takes that description and renders it to the screen. In particular, `render()` returns a **React element**, which is a lightweight description of what to render. 

    * **Always start component names with a capital letter.**

    * Components must return a **single root** element.

    * Props are read-only. A component must not modify the props that are passed to it.
    ```javascript
    function HelloWorld(props) {
        return (
            <div className="hello">Hello, World!</div>
        );
    }

    //OR 

    class HelloWorld extends React.Component {
        render() {
            return (
                <div className="hello">Hello, World!</div>
            );
        }
    }

    ReactDOM.render(<HelloWorld/>, document.getElementById("root"));

    ```


    * **Props**
        
        Property values are passed to components at instatiation.

        Notice the curly braces. These ensure the text inside them is code, not just text.

        ```javascript
        class HelloWorldProps extends React.Component {
            render () {
                return (
                    <div>{this.props.message}</div>
                );
            }
        }

        ReactDOM.render(<HelloWorldProps message="Hello, World"/>,
                        document.getElementById("root"));
        ```
    * **State**
    
        React components can have state. State is like props, but is internal/private and can be changed. State is a JavaScript object.

        **Only class-based components can have state.**

        Whenever `this.setState` is called, an update to the component is scheduled, causing React to merge in the passed state update and rerender the component along with its descendants.

        **If you don't use something in render(), it shouldn't be in the state.**


        Notice the `onClick` handler. Arrow function is used to pass current scope `('this')` to the function.

        A common convention in React apps to use on* names for the handler prop names and handle* for their implementations.

        ```javascript
        class HelloWorldState extends React.Component {
            constructor () {
                super();
                this.state = {
                    message: 'Hello, World!'
                };
            }

            handleClick () {
                let newMessage = this.state.message === 'Hello, World!' ? 'Bye, World!' : 'Hello, World!';
                this.setState({
                    message: newMessage
                });
            }

            render () {
                return (
                    <button onClick={()=>{this.handleClick();}}>
                        {this.state.message}
                    </button>
                );
            }
        }
        ReactDOM.render(<HelloWorldState message="Hello, World"/>,
                        document.getElementById("root"));
        ```

    * **Do not modify state directly, use `setState()`.**
    * [**State updates may be asynchronous**](https://facebook.github.io/react/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)
    * **[State updates are merged](https://facebook.github.io/react/docs/state-and-lifecycle.html#state-updates-are-merged). As state is a JS Object, different fields of the object can be updated individually without affecting the other.**
    * Lifecycle methods

        ```javascript
            componentDidMount(){
                //setup something when the component is rendered in the DOM for the first time.
            }
            componentWillUnmount(){
                //clear up when the component is removed from the DOM
            }
        ```
* **Lifting State Up/Controlled components**

    See: [Lifting State Up](https://facebook.github.io/react/tutorial/tutorial.html#lifting-state-up)

    When you want to aggregate data from multiple children or to have two child components communicate with each other, move the state upwards so that it lives in the parent component. The parent can then pass the state back down to the children via props, so that the child components are always in sync with each other and with the parent.

    Pass down functions from parent to child so that child components can communicate/update parent state by calling those functions.

* **Why immutability is important**

    See: [Why immutability is important](https://facebook.github.io/react/tutorial/tutorial.html#why-immutability-is-important) (Hint: Performance)

* **Keys**

    See: [Keys](https://facebook.github.io/react/tutorial/tutorial.html#keys)

    ```javascript
        <li key={user.id}>{user.name}: {user.taskCount} tasks left</li>
    ```

    `key` is a special property that's reserved by React (along with `ref`, a more advanced feature). When an element is created, React pulls off the key property and stores the key directly on the returned element. Even though it may look like it is part of props, it cannot be referenced with `this.props.key`. React uses the key automatically while deciding which children to update; there is no way for a component to inquire about its own key.

    When a list is rerendered, React takes each element in the new version and looks for one with a matching key in the previous list. When a key is added to the set, a component is created; when a key is removed, a component is destroyed. Keys tell React about the identity of each component, so that it can maintain the state across rerenders. If you change the key of a component, it will be completely destroyed and recreated with a new state.

    **It's strongly recommended that you assign proper keys whenever you build dynamic lists.** If you don't have an appropriate key handy, you may want to consider restructuring your data so that you do.
