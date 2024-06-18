# Multi-Threaded-Proxy-Web-Server-with-Cache

This project is implemented using C and Parsing of HTTP 

OS Component Used ​
Threading
Threadpool
Locks
Semaphore
Cache (LRU algorithm is used in it)

First part is the core basic idea💡 

Main Objective:- 
Build a proxy server (with all benifits of proxy) 
with help of socket programming in C 
- Accept HTTP request from client .
 - Parse the request and fetch the request from remote server (internet).
-  Resolve client request by forwarding the same response to client server.

Implementation: 
How to make connections? With help of sockets 
Proxy server:- Create a socket and bind it to a specific port number (on our machine) so this socket will "listen" and "accept" connection request from clients 
Client server:- Just Create a socket for client  (done by proxy server while accepting request) and send request to proxy server 
So proxy server and client will exchange data with sockets 
Remote server:- proxy will communicate with remote server also with help of socket

Now onto the main part! 

 
Sounds simple? But what if multiple client sends request at same time? 
Issue 1: Can only handle 1 request at a time?
To solve this problem we use multihreading model in C. Each connection request made to client is given a thread for execution by the proxy server. Thus in this way our server can resolve multiple requests at a time. 

Issue 2: No of requests=No of threads?
But in this way we will keep making unlimited threads which isn't good? So we limit max no of clients and We resolve this my implementing THREADPOOLs instead by creating an array (of threads) with size=MAX no of clients.
This is supported by queue data structure which stores the client requests.
Thus our threadpool will keep checking for any client request in the queue. 


Issue 3: unnecessary requests to remote server? 
Many a times we can get frequent same requests so we create a LRU cache with help of Linked List data structure . We store the timestamp of the request to decide which request should be removed if cache size exceeds a certain limit. 
So implemted add() remove() functions of LRU cache 

Issue 4: Concurrent requests to shared resources!
Cache and Queue are shared resources so we may run into RACE Condition as we have multiple threads. So to avoid that we use mutex locks, semaphore and conditions variables whenever shared resources are used.
