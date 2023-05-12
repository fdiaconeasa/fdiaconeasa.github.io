---
tags: slo, capacity, load, stress, performance
share: true
---
> [!note]
> These concepts have been shamelessly stolen from [Martin Kleppmann's book Designing Data-Intensive Applications](https://learning.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/ch01.html#:-:text=Describing%20Load).

## Why have this discussion?

Load parameters link to multiple aspects of the system analysis: #performance, #scaling, #capacity planning, long term resource #optimisation.

- identify current capacity for serving requests. *Ex: with the current compute resources we can support 1000 sales / minute / instance*
- decide to scale out/in and with how much. *Ex: if the number of sales / minute / instance goes beyond 900 by 500, add 50% more instances (assuming the ratio is linear)*
- plan for more hardware purchases. *Ex: we will get a new client that we expect will increase the number of sales by 30% so we need to add 30% more instances when they join.*

## Load Parameters

![[correlation.excalidraw | 100%]]
  
What is a **load parameter**? Load parameters are indicators that are linked directly to the **stress** that a system sees, but do not represent the resource usage. In other words, you can tell if the system load has increased or decreased based on a set of numbers that represent the values of the load indicators, more or less *without* considering computing resource usage.

These indicators MUST not refer to resource usage, but MUST mirror the resource usage from a somewhat business perspective. For example, the CPU usage might be high because the number of bet placements has increased, so the load parameter is actually the **number of bet placements/minute (or second)** and *that* has lead to the increase of CPU usage.  
  
Load parameters are a measure of stress, **not** of performance and they can be used by asking yourself the following questions:  
- How does performance change when you increase a load parameter value, without adding more resources?  
- How much do you need to increase resources to maintain performance when a load parameter value is increasing?  
  
Load parameters should be fairly easy to use in order to decide when and with how much you should scale.

### How do you identify the load parameters of your system?

When setting the load parameters it's important to consider what is actually **putting the highest stress** on your service and for this you can start by considering the actions that the clients are able to call on your service. For example, the clients could read data, write data, trigger some background processing, queue data, etc. However, the load parameters might not necessarily be this straightforward because services do more than one internal operation per API endpoint. 

Let's consider an example where the load is not strictly related to the number of requests: A service provides a simple API that allows batch processing. So the batch size could be starting from 1 and be up to several thousands. In this case, the number of requests per minute is not really telling of the stress on the service, as the size of the batch is important as well. So there are at least 2 scenarios in which the service is under stress:
1. **very high number of small requests in 5 minutes** which leads to **high cpu usage**
2. **average (maybe even low) number of requests in 5 minutes, but with very large batches** which leads to **high memory usage**

![[scenario2.excalidraw | 100% | left]]

Having these 2 scenarios above it's clear that the *number of requests / 5 minutes* is not enough to determine whether the load is high or not. You could very well have a 3rd scenario in which you get a really high number of requests with very large batches. This will drive the stress on the service even higher. 

One potential solution could be to consider the *average batch size per request in 5 minutes*, however, if the number of connections doesn't really matter (your service is async, for example) then it's actually *number of elements being processed in 5 minutes*.

There is no silver bullet for all services in terms of picking the load parameters and many times it could be that there are multiple options to pick from as your system could be put under stress for various reasons. In the end, what you care for are those load parameters that drive the highest stress on the service and that's the list of load parameters you should rely on.

### Multiple load parameters
There are situations in which multiple load parameters are identified for the same service. 

This, in general, leads to two ideas: either you need to combine the multiple load parameters into a single one or your service is actually providing multiple functionalities that leads to stress on your service due to multiple different reasons. 

Ideally, as per the idea of microservices, you would split the service into multiple ones, however this is not always doable without considerable effort, nor wanted due to maintenance/design issues. In this case, your service will simply have multiple reasons to scale out/in. So, instead of increasing the number of instances based on a certain load parameter, you will increase it whenever a load parameter requires you to. Obviously this will lead to a slightly higher complexity when you are scaling in (decrease the number of instances), but should be doable.

## Acceptable performance limits  
  
Now that we know the load parameters (indicators) we can correlate them with the performance metrics, chart them and draw a line representing the acceptable performance below which we don't want to go.
  
![[correlation-slo.excalidraw|100%]]
  
For example, let's say we are looking at the *Betting Service* and we have decided that the load parameter to watch over is the **number of bet placements / minute**. What happens when this load parameter value increases dramatically? It can be that we start seeing an increase in the number of 5xx errors, but also an increase in the response time. It might be that it is acceptable to have 1% error rate and a response time p95 over the last 5 minutes of 2 seconds. If that is the case, we can check what was the load parameter value when we hit the metrics values mentioned before (error rate and p95 over last 5 min). This way we identify the load parameter value corresponding to the lowest performance that we find acceptable.   
  
>[!note]  
> This is basically the highest stress to which the service is still performing acceptably.  
  
  
### Using the tools  
  
Now that we know the load parameters of the service and the performance limits we can say that a service instance can acceptably serve a particular value of the load parameters and once we go above that value we need to scale out/up. Obviously, this works the other way around for scaling in/down.