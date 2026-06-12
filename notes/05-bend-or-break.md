# Chapter 5 - Bend, or Break

[Back to README](../README.md)

## Index
- [28. Decoupling](#28-decoupling)
- [29. Juggling the Real World](#29-juggling-the-real-world)
- [30. Transforming Programming](#30-transforming-programming)
- [31. Inheritance Tax](#31-inheritance-tax)
- [32. Configuration](#32-configuration)
- [Appendix: Domain Model (#28 examples)](#appendix-domain-model-28-examples)

## 28. Decoupling

### 28.1. Coupling is the enemy of change
- Coupling links together things that must change in parallel
- When components are tightly coupled, changing one requires changes everywhere
> **Tip 44. Decoupled code is easier to change**

### 28.2. Design for flexibility
- Individual components should be coupled to as few components as possible
- This enables independent evolution of components

### 28.3. Responsibilities, not containers
- Do not let your objects be a container for a bunch of fields that anyone can query and update
- Each object should encapsulate its own behavior

### 28.4. Encapsulation prevents leaky updates
> **Tip 45. Tell, Do not Ask**
- **Problem**: You should not make decisions based on the internal state of an object and then update that object
- Doing this destroys the benefits of encapsulation
- It spreads the knowledge of the implementation throughout the code
- **Solution**: Let objects manage updates to their own state

#### Example from Project:

**❌ Bad (Coupled):**
```java
public void applyDiscount(Customer customer, String orderId, double discount) {
    List<Order> orders = customer.getOrders();  // exposes internal state
    orders.forEach(order -> {
        if (order.getOrderId().equals(orderId)) {  // makes decision based on Order internals
            Total total = order.getTotal();         // reaches into Total
            double totalAmount = total.getTotalAmount();
            double discountAmount = totalAmount * discount;
            total.setDiscountAmount(discountAmount);  // updates Total directly
            total.setGrandTotal(totalAmount - discountAmount);
        }
    });
}
```

**✅ Good (Decoupled):**
```java
public void applyDiscount(Customer customer, String orderId, double discount) {
    customer
        .findOrderById(orderId)
        .applyDiscount(discount);
}
```

The decoupled version:
- Delegates search responsibility to `Customer`
- Delegates discount application to `Order`
- `Order` then delegates to `Total` (where price logic belongs)

### 28.5. Avoid method call chains
> **Tip 46. Do not Chain Method Calls**
- Try not to have more than one "." when accessing something
- **Bad**: `amount = customer.orders.last().totals().amount` (too many dots, tight coupling)

#### Example from Project:

Even in the decoupled version, there are 2 dots:
```java
customer.findOrderById(orderId).applyDiscount(discount);
// ↑ dot 1            ↑ dot 2
```

This is acceptable because:
- We're delegating responsibility to the objects
- Not making internal decisions based on their state
- The chain represents data flow through responsibilities, not data exploration

**Worse chain** (from coupled version):
```java
customer.getOrders().forEach(order -> {
    if (...) {
        Total total = order.getTotal();
        double totalAmount = total.getTotalAmount();
        // Many dots + reaching into internal state + making decisions outside the object
    }
})
```

### 28.6. Exception: Chains and pipelines
- Chains and pipelines are acceptable—they transform data passing it from one function to the next
- These introduce some coupling but in a form that is less a barrier to changing

### 28.7. Keep your code shy
- Have code only deal with things it directly knows about
- Your application will stay decoupled

#### Example from Project:

**Shy code** (good):
```java
// HighLevelClassDeCoupled only knows about:
// - Customer (it directly asks for an order)
// - The discount value
// It doesn't know about Order internals, Total structure, or discount calculation

public void applyDiscount(Customer customer, String orderId, double discount) {
    customer.findOrderById(orderId).applyDiscount(discount);
}
```

**Chatty code** (bad):
```java
// This code knows about:
// - Customer structure (has List<Order>)
// - Order structure (has Total)
// - Total structure (has totalAmount, discountAmount, grandTotal)
// - Discount calculation logic
// - How to update Total fields

public void applyDiscount(Customer customer, String orderId, double discount) {
    List<Order> orders = customer.getOrders();
    orders.forEach(order -> {
        if (order.getOrderId().equals(orderId)) {
            Total total = order.getTotal();
            double totalAmount = total.getTotalAmount();
            double discountAmount = totalAmount * discount;
            total.setDiscountAmount(discountAmount);
            total.setGrandTotal(totalAmount - discountAmount);
        }
    });
}
```
### 28.8. Global Data
>- **Tip 47. Avoid Glogal Data** - Including singletons
>- **Tip 48. If It's Important Enough to Be Glogal, Wrap It in an API**

## 29. Juggling the Real World

### 29.1. Events
- Represent the availability of information, may come from the outside world or could be internal
- External events: a user clicks a button, a stock quote updates, a delivery arrives
- Internal events: the result of a calculation is ready, a search finishes, a report is ready

### 29.2. Better Applications
- Events are everywhere, so applications that react to events and adapt their behavior will be more efficient, maintainable, and decoupled
- Some strategies to write applications based on events: finite state machines, observer pattern, publish/subscribe, reactive programming
- The strategies mentioned are not competing — they are layered: FSMs handle local state, Observer handles direct notification, Pub/Sub handles decoupled communication, and Reactive programming handles composing all of it at scale

### 29.3. Finite State Machines (FSM)
- An FSM is a model where a system can be in exactly one state at a time, and it transitions to another state only when a specific event occurs
- An FSM defines which transitions are valid — invalid events in a given state are simply ignored or flagged, which is a key benefit over ad-hoc if/else logic
- Actions can be attached to transitions: logic that runs when moving between states
- Useful not only for domain state modeling but also for parsing and input validation
- We can use this model to graphically represent the states of our domain and then code accordingly
- FSMs are underused by developers

### 29.4. The Observer Pattern
- Is a classic design pattern
- The source of events is called the Observable; the clients interested in the events are called Observers
- The observer registers its interest with the observable, usually by passing a function that the observable will call when the event occurs
- When the event occurs, the observable notifies all registered observer functions
- Limitation: the observable needs to know its observers directly, and if observers are slow or crash, it can affect the source — this is why Pub/Sub exists as an evolution

### 29.5. Publish/Subscribe (Pub/Sub)
- Also a classic design pattern for decoupling systems, using publishers, subscribers, and channels
- Channels are implemented in a separate body of code: a library, a process, or distributed infrastructure
- Subscribers register interest in one or more channels by subscribing to them
- Publishers write events into channels
- Introduces asynchrony: publishers and subscribers don't need to be running at the same time, which is a step beyond the observer pattern

### 29.6. Reactive Programming, Streams, and Events
- Reactive programming is a paradigm — it provides a unified way to handle streams of data from multiple sources
- The key idea is that you treat events as a collection you can manipulate: filter, map, combine, throttle — using the same operations you would use on a list
- Streams of data can come from many sources: application data from a server, user-provided data, client-side event data, asynchronous responses, etc.
- These streams can have many formats requiring different processing mechanisms — reactive programming solves this by providing a single composable model
- Unifies the other three strategies into a single composable model at scale
- RxJS is the reactive library for JavaScript; the paradigm also exists in Java (RxJava, Project Reactor) and .NET (Rx.NET)

**Example — combining a static list with a time-based stream using `zip`:**

```javascript
// from() converts an array into an observable stream
// interval() emits an incrementing number every 500ms
// zip() pairs each animal with each tick — controlling the emission pace
let animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
let animals$ = from(animals);
let ticker$  = interval(500);
let combined$ = zip(animals$, ticker$);

combined$.subscribe(
  next     => console.log(next),
  err      => console.log('Error: ' + err),
  ()       => console.log('Completed')
);
// Output: ['ant', 0], ['bison', 1], ['camel', 2] ... every 500ms
```

## 30. Transforming Programming

### 30.1. Key Ideas

- Think of your program not as a series of instructions, but as a pipeline that transforms data
- Programs are data transformers whose job is to transform data input into data output
- A pipeline example where each step takes data in, transforms it, and passes it on:
  ```bash
  cat log.txt | grep "ERROR" | sort | uniq -c
  ```
- **Pipelines over nested logic:** chain small, focused transformations instead of writing deep nested logic or long methods — each step does one thing and passes the result forward
- **Transformations make state explicit:** data goes in and comes out, it does not hide state inside objects
- In a pipeline, each step is independently testable
- **Error handling fits naturally:** if one step fails, the failure travels through the rest of the pipeline without intermediate steps needing to handle it explicitly
- **It's a mindset, not a framework:** you can apply this in Java, TypeScript, C#, or any language you already use

## 31. Inheritance Tax

### 31.1. Key Ideas
- Inheritance introduces coupling — child classes are coupled to their parent and their ancestors
- Inheritance is rarely the answer to a problem, there are better alternatives: interfaces, delegation, mixins
- Prefer interfaces, delegation, and mixins over inheritance — they give flexibility without the coupling cost
- Before using inheritance think if these alternatives could solve your problem and use them

### 31.2. Alternatives to Inheritance

#### 31.2.1. Interfaces
- Interfaces define contracts without implementations
- We can use interfaces to define multiple behaviors
- We can use interfaces as types — any class that implements a specific interface will be compatible with that type
- Interfaces give us polymorphism without using inheritance

```java
public interface Drivable {
    void setSpeed(int speed);
    int getSpeed();
}

public interface Locatable {
    String getLocation();
    void setLocation(String location);
    boolean locationIsValid(String location);
}

// Car implements Drivable and Locatable, do not inherit
public class Car implements Drivable, Locatable {
    // Car must implement all methods from both Drivable and Locatable
}

// Any class implementing Locatable is compatible with the Locatable type
// Car and Phone are different classes, but both can be treated as Locatable
public void processLocatables() {
    List<Locatable> locatables = new ArrayList<>();
    locatables.add(new Car());
    locatables.add(new Phone());
    locatables.forEach(locatable -> {
        if (locatable.locationIsValid(locatable.getLocation())) {
            System.out.println("Valid location: " + locatable.getLocation());
        } else {
            System.out.println("Invalid location: " + locatable.getLocation());
        }
    });
}
```

#### 31.2.2. Delegation
- Delegation moves responsibility to other classes
- It requires thinking about who owns what responsibility
- Delegation is "has-a" instead of "is-a" — a class holds a reference to another class and delegates work to it, rather than inheriting from it
- By using delegation instead of inheritance we write more code but gain more control and flexibility
- It can generate boilerplate code, but that is the tradeoff for looser coupling

#### 31.2.3. Mixins
- The basic idea: give a class new functionality without using inheritance
- Each mixin carries isolated, ready-made behavior that any class can pick up
- Unlike inheritance, each class picks only the behaviors it needs independently
- Unlike interfaces, mixins bring the implementation already done — you don't write it yourself
- Useful when multiple unrelated classes need the same behavior (e.g. loading, error handling, pagination)

```typescript
// Each mixin carries one isolated, ready-made behavior
const WithLoading = (Base: any) => class extends Base {
    showLoading() { console.log("Loading..."); }
    hideLoading() { console.log("Done."); }
};

const WithError = (Base: any) => class extends Base {
    handleError(err: any) { console.error("Error:", err); }
};

const WithPagination = (Base: any) => class extends Base {
    nextPage() { console.log("Next page"); }
    prevPage() { console.log("Previous page"); }
};

// Each class picks only the behaviors it needs — no forced hierarchy
class UserComponent extends WithPagination(WithLoading(WithError(class {}))) {
    loadUsers() {
        this.showLoading();
        // fetch users...
        this.hideLoading();
    }
}

class ProductComponent extends WithLoading(WithError(class {})) {
    loadProducts() {
        this.showLoading();
        // fetch products...
        this.hideLoading();
    }
}

class OrderComponent extends WithLoading(class {}) {
    loadOrders() {
        this.showLoading();
        // fetch orders...
        this.hideLoading();
    }
}
```

## 32. Configuration

### 32.1. Key Ideas
- Keep values that may change (configuration) outside the app
- Parameterize your app using external configuration
- Common configuration: credentials, logging levels, ports, IP, etc
- Instead of loading configuration as a global data structure, wrap it behind an API — this way decoupling your code from the configuration
- Exposing configuration as an API has these benefits:
  - Multiple apps can share configuration
  - Configuration can be maintained via a UI
  - Configuration becomes dynamic — changes apply at runtime without rebuilding or restarting the application
- In Spring Boot, configuration is externalized via `application.yml` and environment variables — in Kubernetes, via ConfigMaps and Secrets, making it fully dynamic and environment-independent
- In microservices architecture, **Spring Cloud Config Server** is the full implementation of this pattern — configuration lives in a Git repo, the Config Server exposes it as a REST API, and all microservices pull from it at startup or at runtime via `/actuator/refresh` without restarting

---

## Appendix: Domain Model (#28 examples)

### Customer
```java
public class Customer {
    private String name;
    private String email;
    private List<Order> orders;

    // Shy: doesn't expose internal list
    public Order findOrderById(String orderId) {
        for (Order order : orders) {
            if (order.getOrderId().equals(orderId)) {
                return order;
            }
        }
        return null;
    }
}
```

### Order
```java
public class Order {
    private String orderId;
    private Customer customer;
    private double amount;
    private Total total;

    // Delegates responsibility to Total (where it belongs)
    public void applyDiscount(double discount) {
        this.total.applyDiscount(discount);
    }
}
```

### Total
```java
public class Total {
    private double totalAmount;
    private double taxAmount;
    private double grandTotal;
    private double discountAmount;

    // Encapsulates discount calculation logic
    public void applyDiscount(double discount) {
        this.discountAmount = this.totalAmount * discount;
        this.grandTotal = this.totalAmount - this.discountAmount + this.taxAmount;
    }
}
```

### HighLevelClassCoupled
```java
import java.util.List;
import domain.Customer;
import domain.Order;
import domain.Total;

public class HighLevelClassCoupled {

    public void applyDiscount(Customer customer, String orderId, double discount) {
        List<Order> orders = customer.getOrders();
        orders.forEach(order -> {
            if (order.getOrderId().equals(orderId)) {
                Total total = order.getTotal();
                double totalAmount = total.getTotalAmount();
                double discountAmount = totalAmount * discount;
                total.setDiscountAmount(discountAmount);
                total.setGrandTotal(totalAmount - discountAmount);
            }
        });
    }

}
```

### HighLevelClassDeCoupled
```java
import domain.Customer;

public class HighLevelClassDeCoupled {

    public void applyDiscount(Customer customer, String orderId, double discount) {
        customer
        .findOrderById(orderId)
        .applyDiscount(discount);
    }

}
```