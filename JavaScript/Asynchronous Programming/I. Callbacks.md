# **Callback**

A **callback function** is a function that is passed into another function as an argument. They may be invoked synchronously or asynchronously.

## **I. Synchronous callbacks**

- Synchronous callback - they are called _immediately after_ the invocation of the outer function.

### **Call Stack**

Call stack is a data structure which records the function that is currently being called.

- If a function returns a callback, the callback is pushed on top the call stack.
- If we return from a function it is popped out of the call stack.

When an exception occurs, it returns a stack trace which is the state of the stack when the error happened.

## **II. Asynchronous callbacks**

- Asynchonous callback - they make a request to the an internal/external API and are added to the _message queue_ once the request is completed.

## **III. Event loop**

The event loop looks at the call stack and message queue. If the call stack is empty the next task in the message queue is pushed on top of the call stack.

### **A. Message queue**

The message queue is where the macrotasks and microtasks are queued. Microtasks are prioritized over macrotasks when it comes to getting pushed on the call stack.

#### **Microtasks**

- Microtasks - Promises and Mutation observer callbacks.

When another microtask gets queued while one is being processed, they are all processed until completion before the event loop can continue resulting in rendering being blocked.

#### **Macrotasks**

- Macrotasks - User generated events and `setTimeout`.

When another macrotask gets queued, it just goes to the end of the message queue.

### **B. Render and Animation Callbacks**

Each animation callback gets processed until at once until completion except the ones that were queued while processing animation callbacks, they are deferred to the next frame.
