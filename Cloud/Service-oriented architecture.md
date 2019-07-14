# Service-oriented architecture

___

**Q: What is SOA (service-oriented architecture)?**

SOA is an architectural pattern in which applications are formed from services decoupled over a network.

Each service in an application acts as a black box, its implementation unknown to the consumers. Ideally, each service takes care of exactly one business task, e.g. send a mail, retrieve data, etc.

Consumers can communicate with the services through a service bus which acts as a message broker between the two layers.

___

**Q: What benefit does SOA provide over monolithic applications?**

One of the benefits provided by SOA is that the services can each be developed using different technologies.

Loose coupling provided by SOA also makes it easier to scale individual services that have higher load than others. Also, since services are decoupled, failure in one of them is less likely to bring down an entire application.

Applications developed using SOA work best when services are owned by different teams, allowing them to have separate releases and development goals.

___

**Q: What's the difference between SOA and microservice architecture?**

Microservice architecture is considered to be the next step in evolution of SOA.

Individual services may communicate with each other, typically using REST, but they should not share resources between each other. Every service is solely responsible for its own state.

Consumers typically communicate with the services through a facade, ie. UI or publicly exposed API, which in turn relays messages to individual services in an application.
