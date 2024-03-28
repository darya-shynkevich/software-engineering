Circuit breakers, much like their hardware counterparts, are a vital component of their respective systems, helping to ***improve reliability and resilience by preventing failures from cascading and causing widespread damage***. 

When the circuit breaker "trips," it interrupts traffic flow to the affected component, redirects it elsewhere, or returns an unavailable error to the requesting client.
### Tripping The Circuit

At a high level, circuit breakers work by monitoring the performance of a component and "tripping" if the component starts to experience issues. The specific criteria for tripping the circuit breaker can vary (highly difficult to get write the first time), but it might include things like:
1. high error rate
2. slow response times, or a large number of timeouts.

> When tripping the circuit, it is also essential to remember that your systems have a period of high load and low load. If you have an error threshold of 10% over 10,000 requests, that's 1,000 failures in the window before the circuit trips, but if there are ten requests, that's one failure tripping the circuit. Test for both scenarios and adjust by setting absolute minimum errors as another gate.

### Resetting the Circuit

1. We can wait for a set amount of time, we can also incorporate a back off here too, but remember this can effect overall system time to recovery.
2. We can use some other metrics we know can caused the breaker to trip like load (don't do this, complexity inducing.) to see if the system is ready to work. I personally find this a bit janky unless you control the upstream system and ultimately ends up in a 'true engineering' task finding all the ways the metric you chose doesn't work in specific scenarios.
3. Upstream system healthchecks.

### Best Practices for Circuit Breakers

1. Test, test, test: It's important to thoroughly test circuit breakers to ensure that they are tripping and resetting at the right times and providing the desired level of protection. This might include testing the circuit breaker under different load and failure scenarios to ensure it is functioning as expected.
2. Monitor, monitor, monitor: Circuit breakers should be monitored to ensure they are functioning properly and to identify any potential issues. This might include tracking the number of times the circuit breaker has tripped, the length of time it takes for the circuit breaker to reset, and the impact of the circuit breaker on the system.
3. Regular maintenance: Circuit breakers should be regularly maintained to ensure they are functioning properly and to make any necessary updates or adjustments. This might include things like updating the thresholds at which the circuit breaker trips or adjusting the circuit breaker's behaviour in response to changes in the system.

# References:

1. [**Circuit Breaker**](https://martinfowler.com/bliki/CircuitBreaker.html)
2. [What is Circuit Breaker in Microservices?](https://medium.com/javarevisited/what-is-circuit-breaker-in-microservices-a94f95f5e5ae)
3. [Circuit Breaking in Distributed Systems](https://www.infoq.com/presentations/circuit-breaking-distributed-systems)
4. [Circuit Breaker for Scaling Containers](https://f5.com/about-us/blog/articles/the-art-of-scaling-containers-circuit-breakers-28919)
5. [Lessons in Resilience at SoundCloud](https://developers.soundcloud.com/blog/lessons-in-resilience-at-SoundCloud)
6. [Protector: Circuit Breaker for Time Series Databases at Trivago](http://tech.trivago.com/2016/02/23/protector/)
7. [Improved Production Stability with Circuit Breakers at Heroku](https://blog.heroku.com/improved-production-stability-with-circuit-breakers)
8. [Circuit Breaker at Zendesk](https://medium.com/zendesk-engineering/the-joys-of-circuit-breaking-ee6584acd687)
9. [Circuit Breaker at Traveloka](https://medium.com/traveloka-engineering/circuit-breakers-dont-let-your-dependencies-bring-you-down-5ba1c5cf1eec)
10. [Circuit Breaker at Shopify](https://shopify.engineering/circuit-breaker-misconfigured)
