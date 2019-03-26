---
layout: post
title:      "Node.js Runtime Environment"
date:       2019-03-26 19:10:47 +0000
permalink:  node_js_runtime_environment
---


Node.js is a cross-platform JavaScript environment that is used to build scalable and responsive applications. Node.js is a server-side runtime environment that is built on the Google Chrome JavaScript V8 engine. This blog post will explore the features of Node.js and provide an overview of the fundamentals of the platform. 

Node.js has non-blocking input/output (I/O) which allows for the handling of multiple requests simultaneously. More specifically, Node.js is single threaded and registers a callback which allows the program to continue rather than wait for the response from a request before being able to execute the remainder of the code. This is a key feature of Node.js that allows Node applications to run fast and enables scalability due to the high number of connections that the environment is able to process.

The single-threaded nature of Node.js is in contrast to the traditional web server model which uses multiple threads to service multiple requests. With Node.js, however, a single thread is responsible for servicing multiple requests. 

Additionally, Node.js uses event-driven programming which enables a constant flow of information without having to wait to receive a response back from an API request. Through the use of callback functions, Node.js ensures that there is a continuity of action in which the function is being executed while it is also being passed down as a parameter to the next callback function. This feature of Node.js is also in contrast to traditional web techniques which have blocking input/output.

Lastly, another feature of Node.js is that the runtime environment executes asynchronously, as opposed to traditional web processes which are synchronous. This feature of Node.js enables the non-blocking input/output behavior of the runtime environment. The asynchronous nature of Node.js allows for the development of complex applications and operations that are not necessarily sequential in nature.

*Application Components*

The first step in a Node application is to import the required module where our HTTP server will live. We use the require directive in order to load the http module and save this into an http variable.

var http = require("http");

We would then create the HTTP server which can be placed in a server.js file and which can be used as the root directory for the project. The listen method is used to bind the particular port location that will be used for the server. The file should contain the following code: 

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8000);

Once this is complete, the node server.js command can be used to start the server.

