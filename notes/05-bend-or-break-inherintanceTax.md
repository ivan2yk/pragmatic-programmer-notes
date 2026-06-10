# Chapter 5 - Inheritance Tax

## Key Ideas
- Inheritance introduces coupling — child classes are coupled to their parent and their ancestors
- Inheritance is rarely the answer to a problem, there are better alternatives: interfaces, delegation, mixins
- Prefer interfaces, delegation, and mixins over inheritance — they give flexibility without the coupling cost
- Before using inheritanve think if these alternatives could solve your problem and use them

## Alternatives to Inheritance

### Interfaces
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

// Car implements Divable and Locatable, do not inherit
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

### Delegation
- Delegation moves responsibility to other classes
- It requires thinking about who owns what responsibility
- Delegation is "has-a" instead of "is-a" — a class holds a reference to another class and delegates work to it, rather than inheriting from it
- By using delegation instead of inheritance we write more code but gain more control and flexibility
- It can generate boilerplate code, but that is the tradeoff for looser coupling

### Mixins
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