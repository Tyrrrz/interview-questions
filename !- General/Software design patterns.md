# Software design patterns

**WORK IN PROGRESS**

___


**Q: What software design patterns do you know?**

- Abstract factory

Instead of manually instantiating an object, it's retrieved from an abstraction (factory). This allows for complex and replaceable instantiation logic that is separate from the consuming code.

- Factory method

Instead of manually instantiating an object, it's created using a supplied function (method), passed by the calling code.

- Builder

Instead of manually instantiating an object, its construction is delegated to another type (builder) which allows for deferred instantiation of an object. This is useful with immutable types that have a large set of parameters, most of which are optional, or when an instance of an immutable type is partially constructed by different methods.

- Object pool

Instead of manually instantiating an object, an instance is requested from an object pool, which may return a previously instantiated object suitable for reuse. This is used in performance-critical paths where additional memory allocation is undesired.

- Prototype

Instead of manually instantiating an object, a previously instantiated and configured instance of an object is copied.

- Singleton

Instead of manually instantiating an object, a single instance is reused across different parts of the program.

- Adapter

Abstraction is implemented by wrapping an existing type to match the contract defined by the target interface.

- Bridge

???

- Composite

???

- Decorator

Abstraction is implemented by combining multiple types into a hierarchy of responsibilities.

- Facade

Abstraction is implemented by subclassing it to a higher-level abstraction with fewer dependencies.

- Flyweight

???

- Private class data

???

- Proxy

???

- Chain of responsibility

Behavior of a system is dynamically implemented by a chained sequence of handlers.

- Command

Behavior of a system is separated from the parameters associated with the request.

- Interpreter

Behavior of a system is based on a hierarchy of domain objects that represents a query formulated as a string of text.

- Iterator

Common abstraction for types that implement enumeration of elements. This allows traversing elements in a container without knowing the implementation of the container.

- Mediator

Communication between services through intermediary that allows publishing and subscribing to messages. This lets services communicate with each other without explicitly referring to one another and, as a result, not depending on one another.

- Memento

???

- Null object

Stub implementation of an abstraction that doesn't do anything. This is typically used in places where a dependency is optional, in order to avoid conditional logic that handles cases where it's not set.

- Observer

???

- State

Behavior of an object is dependent on its internal state.

- Strategy

Set of interchangeable algorithms.

- Template method

Public non-virtual method which is implemented by calling other (usually private) virtual or abstract methods on the same type. This can be used to let clients extend or override object's behavior by changing a subset of its workflow.

- Visitor

Behavior associated with data objects is extracted to a different type (visitor).
