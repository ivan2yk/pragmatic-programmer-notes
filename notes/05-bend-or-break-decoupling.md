# Decoupling - Pragmatic Programmer Chapter 5

## Core Concepts

### 1. Coupling is the enemy of change
- Coupling links together things that must change in parallel
- When components are tightly coupled, changing one requires changes everywhere
- Decoupled code is easier to change

### 2. Design for flexibility
- Individual components should be coupled to as few components as possible
- This enables independent evolution of components

### 3. Responsibilities, not containers
- Do not let your objects be a container for a bunch of fields that anyone can query and update
- Each object should encapsulate its own behavior

### 4. Encapsulation prevents leaky updates
- **Problem**: You should not make decisions based on the internal state of an object and then update that object
- Doing this destroys the benefits of encapsulation
- It spreads the knowledge of the implementation throughout the code
- **Solution**: Let objects manage their own state updates

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

### 5. Avoid method call chains
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

### 6. Exception: Chains and pipelines
- Chains and pipelines are acceptable—they transform data passing it from one function to the next
- These introduce some coupling but in a form that is less a barrier to changing

### 7. Keep your code shy
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

---

## Domain Model

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

---

## Key Takeaways for This Project

1. ✅ `HighLevelClassDeCoupled` correctly delegates instead of making decisions based on internal state
2. ✅ `Order` and `Total` encapsulate their own behavior
3. ✅ `Customer.findOrderById()` is shy—it only exposes what clients need
4. ⚠️ `Customer.getOrders()` violates encapsulation—should be removed or return immutable list
5. ⚠️ Null handling: `findOrderById()` can return null—should use `Optional<Order>` or check for null
