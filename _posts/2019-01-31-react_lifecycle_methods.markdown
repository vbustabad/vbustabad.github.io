---
layout: post
title:      "React Lifecycle Methods"
date:       2019-02-01 00:10:33 +0000
permalink:  react_lifecycle_methods
---


React components have access to various lifecycle methods which can be used to run code at different stages of the lifecycle of a component. The lifecycle methods allow the user to execute greater control over the information that is displayed in a React application, by specifying the times when certain information should be displayed or updated in the application. The stages of the React component lifecycle include: mounting, updating, and unmounting.

*Mounting Phase*

The first method that is called during the mounting phase of a React component is the constructor. Note that the constructor is not required for a React component. The constructor is useful for setting an initial state for data that will be managed within the component. Another use case for the constructor is to bind the context of event handlers to a particular object. As shown in the example below, the attributes of name, price, image_url and category are used to define the initial state of product information in the component. The this.handleOnChange.bind(this) and this.handleOnSubmit.bind(this) is used to define the context for when the event handler methods, handleOnChange and handleOnSubmit, should execute. See the code below:

```
class ProductForm extends Component {
    constructor(props) {
        super(props) 

        this.state = {
            name: "", 
            price: 0, 
            image_url: "", 
            category: ""
        }
        
        this.handleOnChange = this.handleOnChange.bind(this); 
        this.handleOnSubmit = this.handleOnSubmit.bind(this) ;
    }

    handleOnChange(e) {
        const { name, value } = e.target;
        let state = this.state;

        state[name] = value;
        this.setState(state);
    }

    handleOnSubmit(e) {
        e.preventDefault();
        this.props.createProduct(this.state);
    }    

    render() {

      return (
        <div className="ProductForm">
          <h2>Add a New Product</h2>
          <form onSubmit={this.handleOnSubmit}>
              <div>
                <label htmlFor="name">Name:</label>
                <input
                    type="text"
                    name="name"
                    value={this.state.name}
                    onChange={this.handleOnChange}
                />
              </div>
              <div>
                <label htmlFor="price">Price:</label>
                <input
                    type="number"
                    name="price"
                    value={this.state.price}
                    onChange={this.handleOnChange}
                />
              </div>
              <div>
                <label htmlFor="image_url">Image URL:</label>
                <input
                    type="text"
                    name="image_url"
                    value={this.state.image_url}
                    onChange={this.handleOnChange}                
                />
              </div>
              <div>
                <label htmlFor="category">Category:</label>
                <input
                    type="text"
                    name="category"
                    value={this.state.category}
                    onChange={this.handleOnChange}
                />
              </div>
              <br />
              <button type="submit">Submit</button>
          </form>
        </div>
      )
    }
};
  
const mapStateToProps = state => {
    return { productFormData: state.productFormData }
  }

export default connect(mapStateToProps, { updateProductFormData, createProduct })(ProductForm);
```

The next method that is called in a React component is the *static getDerivedStateFromProps()*, which is called during the mounting phase before the render method. Note that static getDerivedStateFromProps() is rarely used as a lifecycle method. It is used to update the initial state in a component due to any changes that may have occurred to props. Note that getDerivedStateFromProps() should be used with caution due to its complexity and common bugs that can occur from its use.

The next method that is invoked in a React component is the *render method*, which is used to display information onto the browser. The render method is pure and is therefore not responsible for any changes to state. Its primary purpose is to render the interface to the user (UI). Note that the render method is pure because it will always display the same output given a certain input.

After the component is mounted onto the DOM, the *componentDidMount()* method is invoked. The componentDidMount() method is used to make API calls and load data into the application. Once the API requests are complete, the component will re-render with the newly acquired information that was obtained from the request. 

*Updating Phase*

The *shouldComponentUpdate()* is the next lifecycle method that follows during the updating phase. The shouldComponentUpdate() method is used for performance optimization performances. The default behavior of the component is to re-render if there are any changes to props and state. With the shouldComponentUpdate() method, the developer can specify whether or not they would like the component to re-render with the newly available information. If the method returns true, the component will update. If the method returns false, the component will not update with the new changes. 

The subsequent lifecycle method is the *getSnapshotBeforeUpdate()* method. The getSnapshotBeforeUpdate() method will occur before the updates are processed and reflected in the DOM. It is used to capture the previous status of props and state. The value that is returned from this method will be passed as a third parameter to the componentDidUpdate() method. This method will have access to both previous and current versions of props and state and is used to adjust the UI from the current DOM to the updated DOM. Some examples of UI features include scroll position, audio/video, text-selection, cursor position, and tool-tip position.

The last lifecycle method during the updating phase of a React component is the *componentDidUpdate()* method. This method is called immediately after the component updates. It accepts previous props, previous state, and snapshot as parameters (the latter of which depends on whether it is passed down from the getSnapshotBeforeUpdate() method). This method is used to ensure that any third-party libraries or UI is in sync with every update. 

*Unmounting Phase*

Lastly, the *componentWillUnmount()* method is the final lifecycle method which occurs during the unmounting phase of a React component. This method is responsible for removing and destroying the component instance from the DOM. Any cleanups that need to be performed will occur with the componentWillUnmount() method. Some examples of cleanup actions include cancelling network requests, invalidating timers, and removing any event listeners that were added.

As a result, React provides various lifecycle methods that can be utilized during the different phases of the lifecycle of a component. These lifecycle methods can be used to execute code or update information at particular points throughout the process. This blog post discu
sses some of the common use cases for the various lifecycle methods, but there are additional use cases as well.

For additional information regarding React lifecycle methods, please visit the official React.js documentation by clicking on the following link: https://reactjs.org/docs/react-component.html. 

Thank you for reading this blog post. Stay tuned for future posts on React and Redux!
 


