# React Learning

## Basic concepts

* **JSX**

    JavaScript with XML

* **Components**

    A component takes in parameters, called `props`, and returns a hierarchy of views to display via the `render` method.
    Components must implement the `render()` method.
    
    The `render` method returns a description of what you want to render, and then React takes that description and renders it to the screen. In particular, `render` returns a **React element**, which is a lightweight description of what to render. 

    ```javascript

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
    
        React components can have state. 
        
        State is considered internal/private. 
        
        State is a JavaScript object.

        Whenever `this.setState` is called, an update to the component is scheduled, causing React to merge in the passed state update and rerender the component along with its descendants.

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