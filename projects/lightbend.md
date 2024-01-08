---
areas: career
projects: lightbend
---
# Goals

- Learn more about Distributed/Reactive Systems 
- Add value to my Clients
- Add internal value to Lightbend

# Tasks

### Learn more about Distributed/Reactive Systems 
- [ ] Read "Reactive Design Patterns from Roland Kuhn"
	- [x] Part 1
	- [ ] Part 2 
	- [ ] Part 3
- [x] Read "Reactive Microsystems from Jonas Boner" 
- [ ] Read "DDIA from Martin Kleppmann"
- [ ] Read "Functional Programming Simplified from Alvin Alexander"
- [ ] Create a blog series "How to create a Reactive System"
	- [ ] Part 1
	- [ ] Part 2
	- [ ] Part 3     
### Add value to my Clients
- [ ] SKY: Integrate the Scheduler PoC in SKYs Architecture
- [ ] NCL: Implement a Reservation System PoC with Kalix
- [ ] GM: Complete PetApp with Akka
- [ ] GM: Create PetApp with Kalix

### Add internal value to Lightbend
- [ ] Create an Engagement Report of GM

# Resources

### System Design

#### Scalability Definitions
[Link](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc101ac74-8273-4550-8d40-7bb88d17a26b_1280x1664.gif)

- High Availability - Means that a service is up and running and responding to requests.       For a high availability one should strive for redundancy and no single-point-of-failure
- High Throughput - Means that many requests can be processed in a given period of time. To get high throughput one can cache data, identify bottlenecks, use more threads/instances and spike management
- High Scalability - Means that we are able to process way more requests. It is measured by response time.

#### Acronyms
[Link](../../attachments/Pasted%20image%2020240107154116.png)
- CAP 
	- Consistency: Every read receives the most recent write or an error
	- Availability: Every request receives a response 
	- Partition Tolerance: The system continues to operate in network faults
- BASE
	- BA: Basically Available
	- S: Soft State
	- E: Eventual Consistency
- KISS
	- K: Keep
	- I: It
	- S: Simple,
	- S: Stupid

#### How Big Tech Ships Code to Production
[Link](https://www.youtube.com/watch?v=xSPA2yBgDgA)

- [ ] TODO: Watch video

### Reactive Design Patterns 

#### Part 1 Introduction

##### Why Reactive?
You want your application to work reliably, even though parts of the software or hardware fail.
You want it to keep working when you have more users.

In order to achieve that, the Reactive Manifesto defines Reactive Systems with the following four requirements:
- It must react to its users (responsive)
- It must react to failure and stay available (resilient)
- It must react to variable load conditions (elastic)
- It must react to inputs (message-driven)

> Message-Driven vs Event-Driven:
> A message is an item of data that is sent to a specific destination.
> An event is a signal emitted by a component.
> In a message-driven system addressable recipients await the arrival of messages and react to them.

![[../../attachments/Pasted image 20240107180855.png]]

**Get Reactive**
1. Find a way to cope with increasing load like vertical or horizontal scaling
2. Find a way to cope with failures like Actor Supervision
3. Be responsive in any case. That means that we should respond in a certain amount of time.

##### Reactive Manifesto Walkthrough

**Responsive:**
Keep the latency as low as possible to be responsive.
This can be achieved by adding caches for IO-Requests, analysing the latency of a shared resource or limiting the maximum latency with a queue.

Use Parallelism to reduce latency.
Find tasks that don't need to be executed sequentially and parallelise those.
![[../../attachments/Pasted image 20240107182744.png]]
![[../../attachments/Pasted image 20240107182753.png]]

Futures are a great fit here.
```scala
val fa: Future[ReplyA] = taskA() 
val fb: Future[ReplyB] = taskB() 
val fc: Future[ReplyC] = taskC() 
val fr: Future[Result] = for (a <- fa; b <- fb; c <- fc) yield aggregate(a, b, c)
```

![[../../attachments/Pasted image 20240107184224.png]]

**Why is the asynchronous result composition with the future needed?**
Without it, you would have had a latency of the maximum of the three individual latencies, but the call would be blocking and you could not have done any other operation in the meantime.
With the Aggregation you can combine the results of all the 3 futures without the need to block execution and therefore be async and non-blocking.



##### Tools for reactive systems



#### Part 2 The Philosophy in a nutshell

#### Part 3 Patterns




