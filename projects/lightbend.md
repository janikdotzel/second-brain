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


