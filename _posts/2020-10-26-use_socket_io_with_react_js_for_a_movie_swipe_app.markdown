---
layout: post
title:      "Use Socket.io with React.js for a movie swipe app"
date:       2020-10-26 00:46:09 -0400
permalink:  use_socket_io_with_react_js_for_a_movie_swipe_app
---


The benefits of Socket.io has been summarized in[ this blog](https://itnext.io/building-a-node-js-websocket-chat-app-with-socket-io-and-react-473a0686d1e1).

The idea is the websocket is a server that can deliver messages in real-time, without the heavy lifting of fetching from an API.

Each user at the website is a client connected with the mutual websocket server.

A client can send messages to the server with a tag and receive messages either through a callback send through the message or constantly listen to the server broadcasting messages with sepcial tags.

A callback is used only for the client that send the mesage to the server

A broadcasting message can be sent to whichever clients or whiever groups of clients.

The level of closure for the client and server need to be differentiated. In order to share the messages, there need to be a variable to store info outside of the client's closure.

In React.js, if you conditional render in a component, the lifecycle events such as componentDidMount or useEffect will be called multiple times and can cause receiving duplicate messages from the server.

The structure of the app is very important. The library relies on certain convention of file structures just like ActiveRecord.
