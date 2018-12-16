---
layout: post
title:      "React and Redux Portfolio Project"
date:       2018-12-15 19:49:16 -0500
permalink:  react_and_redux_portfolio_project
---

I developed an e-commerce application called Home-Deluxe-Application for the React and Redux Portfolio Project. This application enables users to purchase quality home products directly from the website. The home furnishings range from living room, dining room, bedroom, and bathroom products, among others. 

This project required me to build an application that utilizes a Rails API back-end and a React and Redux front-end. Through the various lessons and labs for the curriculum, I learned how React works and how to incorporate Redux as a data management system for the application. As with any portfolio project, the most fundamental lesson is understanding how the various pieces of what was learned fits together, as well as understanding the overall flow of information in the application. As a result, I learned about how components can be created as a way to organize the code for the application. I learned about props and state, specifically the difference between state that is stored in a local component (i.e. local state) and state that is stored in the Redux store which makes the information accessible throughout the application (i.e. global state). I also learned how to create buttons in React and associate a callback function which will execute the functionality that is required upon click of the button by the user.

Furthermore, I learned about component lifecycle methods in React and how to subscribe a component to the Redux store. We would subscribe a component to the Redux store by using a connect function which utilizes mapStateToProps and mapDispatchToProps. The mapStateToProps and mapDispatchToProps functions will make certain information accessible to the local component by creating a prop object which can be used in the component. See the code below:

```
import React, { Component } from 'react';
import { connect } from 'react-redux';
import IndividualProduct from '../components/IndividualProduct';
import { fetchCurrentProduct } from '../actions/products';

class IndividualProductContainer extends Component {

    componentDidMount() {
        const id = this.props.match.params.id;
        this.props.fetchCurrentProduct(id)
    }

    render() {
        console.log("products", this.props)
        return (
          <div className="IndividualProductContainer">
            <IndividualProduct product={this.props.product} />
          </div>
        )
      }
}

const mapStateToProps = state => {
  return { product: state.products.product }
}
        
export default connect(mapStateToProps, { fetchCurrentProduct })(IndividualProductContainer);
```

```
import React from 'react';

const IndividualProduct = (props) => (

    <div key={props.product.id} className="IndividualProduct">
      <h4>Product</h4>
      <p>{props.product.name}</p>
      <p>Price: ${props.product.price}</p>
      <img className="ProductImage" src={props.product.image_url} alt="Product" />
      <p>Category: {props.product.category}</p>
      <br /><br />          
    </div>
);

export default IndividualProduct;
```

The mapStateToProps creates a product object which will be accessible via the props for the component.  Note that the product information is passed down to the IndividualProduct child component via this.props.product. Similarly, the mapDispatchToProps function will make the action fetchCurrentProduct available to the component via this.props.fetchCurrentProduct(). Note here that the fetchCurrentProduct is included in the connect function located at the bottom of the IndividualProductContainer component as a shorthand for the mapDispatchToProps function. After the component is mounted, the fetchCurrentProduct() function will execute which will dispatch an action to the corresponding reducer. See the code below:

```
import { resetProductForm } from './productForm';

const setProducts = products => {
    return {
        type: 'GET_PRODUCTS_INFORMATION',
        products
    };
};

const setIndividualProduct = product => {
    return {
        type: 'GET_INDIVIDUAL_PRODUCT_INFORMATION',
        product
    };
};

export const getProducts = () => {
  return dispatch => {
      return fetch(`http://localhost:3001/api/products`)
        .then(response => response.json())
        .then(products => dispatch(setProducts(products)))
  }
}

export const fetchCurrentProduct = (id) => {
    return dispatch => {
        return fetch(`http://localhost:3001/api/products/${id}`)
        .then(response => response.json())
        .then(product => {
            console.log(product)
            return dispatch(setIndividualProduct(product))})
    }
}

export const addProduct = product => {
    return {
        type: "ADD_PRODUCT", 
        product
    }
}

export const createProduct = product => {
    console.log(product)
    return dispatch => {
        return fetch(`http://localhost:3001/api/products`, {  
          method: "POST",  
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          }, 
          body: JSON.stringify({product: product})
        })
          .then(response => response.json())
          .then(product => dispatch(addProduct(product), resetProductForm()))
    };
}
```

```
const initialState = {products: [], product: {}};

export default function productsReducer(state = initialState, action) {
    switch (action.type) {
      case "GET_PRODUCTS_INFORMATION":
        return  {...state, products: action.products};

      case "GET_INDIVIDUAL_PRODUCT_INFORMATION":
        return  {...state, product: action.product};

      case "ADD_PRODUCT":
        return  {...state, products: state.products.concat(action.product) };
  
      default:
        return state;
    }
}
```

After the fetchCurrentProduct() function is invoked, the function will fetch the product information corresponding to the particular product and will dispatch the setIndividualProduct action to the reducer. The setIndividualProduct action will then send an action type and a payload of product which contains the individual product information. Subsequently, the productsReducer function will match the action type of GET_INDIVIDUAL_PRODUCT_INFORMATION to the case statement within the function and return the updated state.

As you can see, the aforementioned is a brief overview of the overall flow of information in the application. React and Redux complement each other and are powerful tools that can be used to create the visual interface of an application. I look forward to continuing to build React and Redux applications and utilizing React and Redux throughout my professional career. 

Thank you for reading this blog post. In order to view the Home-Deluxe-Application, please visit the following links: https://github.com/vbustabad/home-deluxe-application-client and https://github.com/vbustabad/home-deluxe-application-api.

Stay tuned for future posts regarding React and Redux as well!
