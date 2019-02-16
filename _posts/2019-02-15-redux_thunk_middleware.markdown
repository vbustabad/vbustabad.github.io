---
layout: post
title:      "Redux Thunk Middleware"
date:       2019-02-15 19:34:21 -0500
permalink:  redux_thunk_middleware
---


Redux Thunk is a middleware that allows you to make asynchronous web requests in Redux. Redux Thunk solves an important timing issue that arises with asynchronous web requests by delaying the dispatch of an action. The middleware allows you to make the asynchronous web request, receive the data from the response, and return an action which will be sent to the reducer. We will explore the details of this with the following lines of code. See the code below:

*catActions.js File*

```
export function fetchCats() {
    const cats = fetch('http://localhost:4000/db').then(response => response.json());
    return {
        type: 'FETCH_CATS',
        payload: cats
    };
}
```

```
export function fetchCats() {
	  return (dispatch) => {
	      dispatch({type: 'LOADING_CATS'});
	  return fetch('http://localhost:4000/db').then(response => {
	      return response.json()
	      }).then(cats => {
	      return dispatch({
	        type: 'FETCH_CATS',
	        payload: catPics
	      })
	    });
	  }
	};
```

The above code is taken directly from the Flatiron School’s Redux Thunk lab. I will use the fetchCats() function as an example to demonstrate the importance of Redux Thunk middleware for asynchronous web requests in Redux. The main issue that can arise with asynchronous web requests in Redux is that an action can be sent to the reducer with data regarding the state which does not reflect the current state in the application. In order to avoid this, Redux Thunk middleware allows you to return a function, rather than an action object. The result of this is that you are able to execute the various lines of code in order. Any subsequent actions will only execute upon successful completion of the API web request.

In the first code snippet, the fetch request is saved into a cats variable. Note that the cats variable is later called in the action object as the payload. The action object is the plain Javascript object which will be returned on line 3 of the code which contains a type of ‘FETCH_CATS’. The code in this snippet would ultimately return an erroneous state. Note that the cats payload in the action object does not reflect the new data from the fetch request. Given this code, the fetchCats() function would immediately return a value for the cats state, regardless of whether or not the API web request has completed. It is therefore for this reason that a function should be returned, rather than an action object. We can invoke a function in order to address the issue with the timing of the API web request, which is asynchronous. We would then run the subsequent lines of code from the .then chain only after the web request is completed.

In the second code snippet, however, the store’s dispatch function is passed as an argument within the inner function. This function sends an action to the reducer which will indicate that the state of the application is currently loading data. Dispatch allows you the ability to send multiple actions. We are then able to make the fetch request and call various .then functions which will manipulate the data and convert it to JSON. Lastly, a final action object of type ‘FETCH_CATS’ with a payload of catPics is sent to the reducer and is used to update the state accordingly.

As a result, Redux Thunk middleware allows you the ability to not only return action objects, but also functions. A thunk is function that returns a function. We need to be able to return a function in order to resolve the issue related to the timing of the API web request. The API web request must complete and the data received before any further lines of code execute within the function. This will ensure that the data that is dispatched to the reducer will contain the current data that is needed to update the state in the Redux store. 

In order to incorporate Redux Thunk into an application, we would run the following command in the terminal:

npm install --save redux-thunk

Then, the thunk library is imported and applyMiddleware() is invoked with thunk passed in as an argument:

*index.js File*

```
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers/index';

const store = createStore(rootReducer, applyMiddleware(thunk));
```

Thank you for reading this blog post. If you have any comments or feedback, please let me know. 

Stay tuned for posts on other programming topics as well!

