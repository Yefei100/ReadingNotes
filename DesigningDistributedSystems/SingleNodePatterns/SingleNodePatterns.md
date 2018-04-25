# Part I, Single-Node Patterns

Chapters 2 through 4 discuss reusable patterns and components that occur on individual nodes within a distributed system. It covers the side-car, adapter, and ambassador single-node patterns.

In contrast to multi-node, distributed patterns, all of these patterns assume tight dependencies among all of the containers in the pattern. In particular, they assume that all of the containers in the pattern can be reliably coscheduled onto a single machine. They also assume that all of the containers in the pattern can optionally share volumes or parts of their filesystems as well as other key resources.   

## Motivations

**Break up the components running on a SINGLE MACHINE into different containers.**

**Keywords: Modularity, Reusability**

In general, the goal of a container is to establish boundaries around specific resources. Likewise, the boundary delineates team ownership. Finally, the boundary is intended to provide separation of concerns.

### 1. The Sidecar Pattern

Made up of two containers: application container, which has the core logic for the application. sidecar pattern, to augment and improve the application container, without application's container knowledge often.

Example: SSL Proxy to a legacy http service in order to provide HTTPS service.

#### Modular Application Containers

Using sidecar pattern's advantage: modularity and reuse of the components used as sidecars. They can be used to create modular utility containers that standardize implementations of common functionality.

To be a modular, reusable artifact, the sidecar should be reusable across a wide variety of applications and deployments.

To achieve this, we need to focus on developing three areas:

* Parameterizing your containers
* Creating the API surface of your container
* Documenting the operation of your container

### 2. Ambassadors

An ambassador container brokers interactions b/w the application container and the rest of the world.

Using an Ambassador to:

* Shard a Service
* Service Brokering
* Do Experimentation or Request Splitting

### 3. Adapters

The adapter container is used to modify the interface of the application container so that it conforms to some predefined interface that is expected of all applications.
