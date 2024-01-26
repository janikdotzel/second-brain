
# Book Summary
## Part 1 Introduction

### Why Reactive?
You want your application to work reliably, even though parts of the software or hardware fail.
You want it to keep working when you have more users.

In order to achieve that, the Reactive Manifesto defines Reactive Systems with the following four requirements:
- It must react to its users (responsive)
- It must react to failure and stay available (resilient)
- It must react to variable load conditions (elastic)
- It must react to inputs (message-driven)

> Message-Driven vs Event-Driven:
> A message is an item of data that is sent to a specific destination. In a message-driven system addressable recipients await the arrival of messages and react to them.
> An event is a signal emitted by a component. So in an event-driven system the sender is known opposed to the receiver in the message-driven system.

**Get Reactive**
1. Find a way to cope with increasing load like vertical or horizontal scaling
2. Find a way to cope with failures like Actor Supervision
3. Be responsive in any case. That means that we should respond in a certain amount of time.

### Reactive Manifesto Walkthrough

#### Responsive:
Reactive applications are defined by the axiom that you can choose any two of the following three characteristics:  
- High throughput  
- Low latency, but also smooth and not jittery  
- Small footprint


Keep the latency as low as possible to be responsive.
This can be achieved by:
- adding caches for IO-Requests
- analysing the latency of a shared resource
- limiting the maximum latency with a queue.
- use Parallelism to reduce latency

Find tasks that don't need to be executed sequentially and parallelise those.
Futures are a great fit here. Instead of going through the tasks sequentially:
```java
ReplyA a = computeA(); 
ReplyB b = computeB(); 
ReplyC c = computeC(); 
Result r = aggregate(a, b, c);
```
We can use futures to compute the tasks at the same time on different threads:
```java
Future a = taskA(); 
Future b = taskB(); 
Future c = taskC(); 
Result r = aggregate(a.get(), b.get(), c.get());
```
An even nicer solution is this:
```scala
val fa: Future[ReplyA] = taskA() 
val fb: Future[ReplyB] = taskB() 
val fc: Future[ReplyC] = taskC() 
val fr: Future[Result] = for (a <- fa; b <- fb; c <- fc) yield aggregate(a, b, c)
```

**Why is the asynchronous result composition with the future needed?**
Without it, you would have had a latency of the maximum of the three individual latencies, but the call would be blocking and you could not have done any other operation in the meantime.
With the composition you can combine the results of all the 3 futures without the need to block execution and therefore be async and non-blocking.

**Limits of parallel execution**
Amdahls Law defines the limit of parallel execution
![[Pasted image 20240126132614.png]]

The unisversal scalability law also takes the overhead of coordination and synchronizing into account
![[Pasted image 20240126132742.png]]

#### Resiliency
We need to accept that failure can happen and we should embrace and handle it instead of trying to avoid it:
- Software will fail
- Hardware will fail
- Humans will fail

> What does resilience mean?
> Resilience is the ability to recover to a non-failed state after a failure

**Ways of getting resilient**

We can *bulkhead* like a ship by splitting our system in smaller parts. Our application should still be able to function even if some component has failed.
![[Pasted image 20240126133215.png]]

We can use *circuit breakers* to fail fast. If we know that something is not working correctly this component should respond to the caller that it's not available. This grants us a fast response time and it gives the problematic component time to recover from a high load or to restart.

We can use *supervision* to define actions of what to do if a component has failed.
We can respond with a failure message, define that we should restart or just skip an invalid message. Akka can define a supervision strategy to restart a component.

We need to *lose strong consistency*. The CAP theorem states that we can either achieve consistency or high availability in case of a network partition
- Consistency (C) equivalent to having a single up-to-date copy of the data  
- High availability (A) of that data (for updates)  
- Tolerance to network partitions (P)

Systems with an inherently distributed design are built on a different set of principles. 
One such set is called BASE:  
- Basically available  
- Soft state (state needs to be actively maintained instead of persisting by default) 
- Eventually consistent

#### Elasticity
Is achieved by *Message-Passing* and *Location Transparency*.

#### Message-Driven
More information in later chapters.

### Tools for reactive systems
There are reactive tools and libraries that will make our lives easier. 
And functional programming techniques are also very good to know.

#### FP
Functional Programming helps us to avoid side-effects which leads to less problems in a multicore parallelised application.
The goal here is to write functions that are completely isolated and can be composed together to more complex functions. Isolation means that all the variables that are needed in the function will be passed in. Nothing in the function changes something outside this function.
The isolation of a function avoids a lot of problems that comes with multicore apps.

Here are some concepts:
1. **Immutability**: One a variable is instantiated, its value can't change anymore.
2. **Referential transparency**: A function is ref. transparent if it can be replaced with a single value. That proves that it has no-side effects.
3. **Side-Effects**: Side effects are things that change something outside your function. Like a global variable called counter which you increment.
4. **Functions as first-class citizens**: In FP everything is a function. you can functions pass in as parameters and therefore compose multiple functions together.

#### Event loops
1. **Waiting for Events**: The event loop runs continuously to check for incoming events. This can include anything from user actions (like clicks or keyboard inputs), system events (like timers expiring or software interrupts), or external events (such as incoming network messages).
2. **Event Queue**: When an event occurs, it's placed into an event queue. This queue stores all the events that have happened but have not yet been processed. The event loop takes events from this queue one at a time.
3. **Dispatching Events**: For each event, the event loop calls a callback function or event handler that's been registered to handle that type of event. This is where the actual processing of an event takes place, such as updating the user interface, reading from or writing to a database, or sending a request over the network.

#### Futures
A Future is a read-only handle to a value or failure that may become available at some point in time. A Promise is the corresponding write-once handle that allows the value to be provided.
All Future implementations provide a mechanism to turn a code block for example, a lambda expression into a Future such that the code is dispatched to run on a different thread, and its return value fulfills the Future’s Promise when it becomes available. 
Futures therefore provide a simple way to make code asynchronous and implement parallelism. 
Futures return either the result of their successful evaluation or a representation of whatever error may have occurred during evaluation.

Futures and Promises are asynchronous and nonblocking, but they do not support message passing. They do scale up to use multiple cores on one machine. Current implementations do not scale outward across nodes in a network. They do provide mechanisms for fault tolerance when a single Future fails, and some implementations aggregate failure across multiple Futures such that if one fails, all fail.

#### Reactive Streams
Rx provides facilities to process streams of data in an asynchronous and nonblocking fashion. The current implementations scale up to use multiple cores on a machine but not outward across nodes in a network. Rx does not provide a mechanism for delegating failure handling, but it does include provisions to reliably tear down a failed stream-processing pipeline via dedicated termination signals. RxJava in particular is a useful building block for implementing components of a Reactive system.

#### Actor model
**INHERENTLY ASYNCHRONOUS** 
The definition of Reactive states that interactions should be message-driven, asynchronous, and nonblocking. Actors meet all three of these criteria. Therefore, you do not have to do anything extra to make program logic asynchronous besides creating multiple Actors and passing messages between them. You need only avoid using blocking primitives for synchronization or communication within Actors, because these would negate the benefits of the Actor model.

**FAULT TOLERANCE VIA SUPERVISION** 
Most implementations of Actors support organization into supervisor hierarchies to manage failure at varying levels of importance. When an exception occurs inside an Actor, that Actor instance may be resumed, restarted, or stopped, even though the failure occurred on a different asynchronous thread.

**LOCATION TRANSPARENCY** 
Erlang and Akka provide proxies through which all Actor interactions must take place: a PID in Erlang and an ActorRef in Akka. So, an individual Actor does not need to know the physical location of the Actor to which it is sending a message—a feature called location transparency.
This makes message-sending code more declarative, because all the physical “how-to” details of how the message is actually sent are dealt with behind the scenes. Location transparency enables you to add even such sophisticated features as starting a new Actor and rerouting all messages to it if a receiving Actor goes down mid-conversation, without anyone needing to alter the message-sending code.

**NO CONCURRENCY WITHIN ONE ACTOR** 
Because each Actor contains only a single thread of execution within itself, and no thread can call into an Actor directly, there is no concurrency within an Actor instance unless you intentionally introduce it by other means. Therefore, Actors can  encapsulate mutable state and not worry about requiring locks to limit simultaneous access to variables. This greatly simplifies the logic inside Actors.

**REACTIVE EVALUATION OF ACTORS** 
Actors are asynchronous and nonblocking, and support message passing. They scale up to use multiple cores on one machine, and they scale outward across nodes in both the Erlang and Akka implementations. They provide supervision mechanisms, as well, in support of fault tolerance. They meet all the requirements for building Reactive applications. This does not mean Actors should be used for every purpose when building an application, but they can easily be used as a backbone, providing architectural support to services that use other Reactive technologies.

## Part 2 The Philosophy in a nutshell

### Message passing



### Location Transparency





## Part 3 Patterns



# Questions to test the understanding

## Part 1 - Chapter 1

No questions.

## Part 1 - Chapter 2

- Explain the following terms Responsive, Resilient, Elastic, Message-driven

## Part 1 - Chapter 3

- What are the costs of building an application that is not Reactive? 
- What is functional programming and how does it relates to Reactive applications? 
- What are the trade-offs involved in choosing between high throughput, low/smooth latency, and small footprint?
- What are the pros and cons of all application toolkits that support the Reactive model? 
- How does the Actor model simultaneously addresses both fault tolerance and scalability?

## Part 2 - Chapter 1

