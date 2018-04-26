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

