# Part II, Serving Patterns

Chapters 8 and 9 cover multi-node distributed patterns for long-running serving systems like web applications. Patterns for replicating, scaling, and master election are discussed.

Microservices

A monolithic service with all functions in a single container
vs
A microservice architecture with each function broken out as a separate microservice

## Replicated Load-Balanced Services

In such services, every server is identical to every other server and all are capable of supporting traffic. Consists of a load balancer in front of servers.

#### Stateless Services

Ones that don't require saved state to operate correctly.

### Session Tracked Services

Often there are reasons for wanting to ensure that a particular user's requests always end up on the same machine. =>

1. Caching that user's data in memory
2. Interaction is long running and some amount of state is maintained b/w requests

The session tracking is performed by hashing the source and description IP addresses and using that key to identify the server that should service the requests. Often via a ***consistent hashing function***.

For external session tacking, application-level tracking(cookies, e.g.,) is preferred.

### Caching Layer

A cache exists b/w your stateless application and the end-user request.
**All requests will be sent from the IP addresses of the cache, not the end user of your service.**

#### Rate Limiting and Denial-of-Service Defense

If you are deploying an API, it is generally a best practice to have a relatively  small rate limit for anonymous access and then force users to log in to obtain a higher rate limit. Requiring a login provides auditing to determine who is responsible for the unexpected load, and also offers a barrier to would-be attackers who need to obtain multiple identities to launch a successful attack.

HTTP header: X-RateLimit-Remaining

#### SSL Termination

A secure socket layer (SSL) connection uses a certificate for authentication before sending encrypted data from a client computer to the web server. SSL termination, a form of SSL offloading, shifts some of this responsibility from the web server to a different machine.


## Sharded Services

In contrast to replicated services, with sharded services, each replica, or shard, is only capable of serving a subset of all requests. A load-balancing node, or root, is responsible for examining each request and distributing each request to the appropriate shard or shards for processing.

Sharded services are generally used for building ***stateful*** services. The primary reason for sharding the data is because the size of the state is too large to be served by a single machine. Sharding enables you to scale a service in response to the size of the state that needs to be served.

### Sharded Caching

A sharded cache is a cache that sits b/w the user requests and the actual front end implementation.

#### The Role of the Cache in System Performance
The end user performance is determined by number of requests it can process, as well as the latency.

It's always recommended that you load test your system both with and without caches to understand the impact of the cache on the overall performance of your system.

#### Replicated, Sharded Caches

Rather than having a single server implement each shard in the cache, a replicated service is used to implement each cache shard.

### Sharding Function

How do you determine which shard in the range should be used for the request?

Sharding function is very similar to a hashing function, with the characteristics of:

* Determinism
* Uniformity: The distribution of outputs across the output space should be equal

#### Selecting a key

Determining the appropriate key for your sharding function is vital to designing your sharded system well. Determining the correct shard key requires an understanding of the requests that your expect to see.

### Sharded, Replicated Serving

Sharding is useful when considering any sort of service where there is more data than can fit on a single machine. 

### Hot Sharding Systems

Ideally the load on a sharded cache will be perfectly even, but in many cases this is not true and "hot shards" appear because organic load patterns drive more traffic to one particular shard.

When this happens, with a replicated sharded cache, you can scale the cache shard to respond to the increased load. If you set up autoscaling for each cache shard, you can grow and shrink each replicated shard as the organic traffic to your service shifts around.

