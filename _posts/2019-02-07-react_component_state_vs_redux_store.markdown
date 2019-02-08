---
layout: post
title:      "React Component State vs. Redux Store"
date:       2019-02-08 03:28:24 +0000
permalink:  react_component_state_vs_redux_store
---


When building a React and Redux application, a key question that will arise is how the information that you will need for the application should be saved. More specifically, should the information be saved locally in the React component state or should the information be saved globally within the Redux store? The answer to this question will have a fundamental impact on the design of the application and the way that information will be managed within the application. 

The main difference between the React component state and the Redux store is whether the information will be used within the local component only or if the information will be needed throughout the application. If the information is needed only for a particular component or UI, then the information can be stored within the React component state. If, however, the information will be needed in multiple components throughout the application, then the information should be saved within the Redux store.

Another consideration is related to the amount of information that will be managed within the application. Redux is a powerful tool which creates a single global state object and which can therefore serve as a single source of truth. Redux provides a clear structure for updating state, including the connect function, actions and reducers, which work together in a sequential flow to update information within the application. As a result, an advantage to using Redux in an application is that it becomes easier to manage information and debug any errors. 

Whether or not Redux will be needed in an application is a decision that will vary with every application. If it is sufficient to maintain state within a local React component, then probably only React will be needed. Note also that it takes time to set up a Redux store and incorporate Redux into an application. Redux can be used to manage growing complexity within an application. It is important to consider, however, if the investment in time will provide a clear benefit in return. Redux provides greater control over the process of updating state within an application due to the various steps that are involved and the restrictions that can be placed as to when certain updates should take place. Some of these changes can include information updates, rendering of the UI to the browser, and network requests.

As a result, Redux is a powerful tool that can be used as an information management system within an application. Its use will vary on the amount of information within the application, whether the changes to the information will occur in multiple components, and if there is a need to have greater control over the display of the user interface and request for information from network requests, among other factors. 

Thank you for reading this blog post. For more information regarding state in React and Redux, please visit the official Redux documentation at: https://redux.js.org/introduction/getting-started. 

Stay tuned for future posts on other programming topics as well!
