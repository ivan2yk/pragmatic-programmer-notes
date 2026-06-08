# Chapter 5 - Juggling the Real World

## Events
- Represent the availability of information, may come from the outside world or could be internal
- External events: a user clicks a button, a stock quote updates, a delivery arrives
- Internal events: the result of a calculation is ready, a search finishes, a report is ready

## Better Applications
- Events are everywhere, so applications that react to events and adapt their behavior will be more efficient, maintainable, and decoupled
- Some strategies to write applications based on events: finite state machines, observer pattern, publish/subscribe, reactive programming
- The strategies mentioned are not competing — they are layered: FSMs handle local state, Observer handles direct notification, Pub/Sub handles decoupled communication, and Reactive programming handles composing all of it at scale

## Finite State Machines (FSM)
- An FSM is a model where a system can be in exactly one state at a time, and it transitions to another state only when a specific event occurs
- An FSM defines which transitions are valid — invalid events in a given state are simply ignored or flagged, which is a key benefit over ad-hoc if/else logic
- Actions can be attached to transitions: logic that runs when moving between states
- Useful not only for domain state modeling but also for parsing and input validation
- We can use this model to graphically represent the states of our domain and then code accordingly
- FSMs are underused by developers

## The Observer Pattern
- Is a classic design pattern
- The source of events is called the Observable; the clients interested in the events are called Observers
- The observer registers its interest with the observable, usually by passing a function that the observable will call when the event occurs
- When the event occurs, the observable notifies all registered observer functions
- Limitation: the observable needs to know its observers directly, and if observers are slow or crash, it can affect the source — this is why Pub/Sub exists as an evolution

## Publish/Subscribe (Pub/Sub)
- Also a classic design pattern, also named the pub/sub pattern
- A good technique for decoupling systems
- There are publishers and subscribers connected via channels
- Channels are implemented in a separate body of code: a library, a process, or distributed infrastructure
- Subscribers register interest in one or more channels by subscribing to them
- Publishers write events into channels
- Introduces asynchrony: publishers and subscribers don't need to be running at the same time, which is a step beyond the observer pattern

## Reactive Programming, Streams, and Events
- Reactive programming is a paradigm — it provides a unified way to handle streams of data from multiple sources
- The key idea is that you treat events as a collection you can manipulate: filter, map, combine, throttle — using the same operations you would use on a list
- Streams of data can come from many sources: application data from a server, user-provided data, client-side event data, asynchronous responses, etc.
- These streams can have many formats requiring different processing mechanisms — reactive programming solves this by providing a single composable model
- Unifies the other three strategies into a single composable model at scale
- RxJS is the reactive library for JavaScript; the paradigm also exists in Java (RxJava, Project Reactor) and .NET (Rx.NET)