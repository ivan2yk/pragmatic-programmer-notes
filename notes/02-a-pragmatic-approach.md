# A Pragmatic Approach

[Back to README](../README.md)

## Index
- [Key Idea](#key-idea)
- [8. The Essence of Good Design](#8-the-essence-of-good-design)
- [9. DRY - The Evils of Duplication](#9-the-evils-of-duplication)

## Key Idea
Pragmatic programmers should take responsibility for the quality of their work. The following principles will help them achieve this goal, making development more effective and enjoyable.

## 8. The Essence of Good Design

ETC principle: Design and develop software in a way that it easily adapts to changes over time.

Software that is easy to change is better than software that is hard to change, to achieve better software we must actively apply the ETC principle.

Applying ETC principle requires deliberate practice until it becomes second nature. Asking the following question will help us internalize the principle: "**Did the thing I just did make the overall system easier or harder to change?**" Always make decisions that makes the software Easy To Change over time.

Most design principles such as Single Responsibility, Decoupling, and Naming Conventions are special cases of ETC.

> **Tip 14. Good Design Is Easier To Change Than Bad Design**

## 9. The Evils of Duplication

> **Tip 15. DRY - Do not Repeat Yourself**: Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

> **Tip 16. Make It Easy to Reuse**

**❌ Bad (Duplication of Knowledge)**: The goal of the next code is to determine the shipping cost, if the total is greather or equal than 100 there is not cost for shipping. The number 100 and the comparison rule is spread on three classes, this makes the code harder to change since we should change three classes.
```java
// In CheckoutService.java
double shippingCost = (order.total() >= 100) ? 0 : 15.0;

// In OrderSummaryView.java
String message;
if (order.total() >= 100) {
    message = "You qualify for free shipping!";
} else {
    message = "Spend " + (100 - order.total()) + " more for free shipping";
}

// In InvoiceGenerator.java
boolean freeShipping = order.total() >= 100;
```

**✅ Good (DRY)**: The rule lives on one place and every consumer asks the policy instead of redefining it, this will be easier to change since rule are a single location on the system.
```java
public class ShippingPolicy {
    private static final double FREE_SHIPPING_THRESHOLD = 100.0;
    private static final double STANDARD_SHIPPING_COST = 15.0;

    public boolean qualifiesForFreeShipping(Order order) {
        return order.total() >= FREE_SHIPPING_THRESHOLD;
    }

    public double shippingCostFor(Order order) {
        return qualifiesForFreeShipping(order) ? 0 : STANDARD_SHIPPING_COST;
    }

    public double amountUntilFreeShipping(Order order) {
        return Math.max(0, FREE_SHIPPING_THRESHOLD - order.total());
    }
}

// In CheckoutService.java
double shippingCost = shippingPolicy.shippingCostFor(order);

// In OrderSummaryView.java
String message = shippingPolicy.qualifiesForFreeShipping(order)
    ? "You qualify for free shipping!"
    : "Spend " + shippingPolicy.amountUntilFreeShipping(order) + " more for free shipping";

// In InvoiceGenerator.java
boolean freeShipping = shippingPolicy.qualifiesForFreeShipping(order);
```

## 10. Orthogonality

Orthogonality is a critical concept if you want to produce systems that are easy to design, develop, test and extend.

In software development orthogonality means a kind of **independence** or **decoupling**: two or more components are orthogonal if a change in one does not affect the others.

Orthogonality does not mean absence of dependency between components, it means independence of axes of change.

### Examples of Orthogonality

- The UI component and the business logic component should be orthogonal. The UI depends on the business logic, it has to, it calls it, but if that dependency goes through a stable interface then a change in the interest rate logic does not affect the UI, and changing the UI from Angular to a mobile app does not affect the business logic. The two components sit on separate axes, so a move in one does not force a move in the other, and change only propagates when the shared contract changes.
- The UI and the database code should be orthogonal too. Adding a column to a table may affect the data access component, but that change should never reach the UI.

### Benefits

1. Gain productivity: changes stay localized, reuse is promoted, combining independent components produces more functionality, and defects are easier to fix.
2. Reduce risk: diseased sections stay isolated so a sick component does not spread the bad around the system, components are easy to test, the system is less fragile, and you stay less coupled to third party vendors.

### How to apply Orthogonality

1. Design: keep the system modular, component based and layered. Test your design by asking how many modules are affected if the requirements behind a particular function change; the answer should be one.
2. Toolkits and libraries: they should not impose changes on the system they are added to, if they do then they are not orthogonal to your system.
3. Coding: write shy code, modules that do not reveal unnecessary details and do not rely on the implementation of others. Avoid global data and avoid similar functions.
4. Testing: unit testing individual components should be easy, and the tests should run automatically as part of the regular build process.
5. Documentation: content and presentation should be independent.
6. Living with orthogonality: constantly monitor the overall system, looking for duplication and keeping components independent as the code grows.

> **Tip 17. Eliminate Effects Between Unrelated Things**

### 11. Reversibility
- The environment changes all the time, and there is usually more than one way to implement something, also there is usually more than one vendor available to provide a third-party product.
- We should be prepare for the changes.
- What we can do is make things easy to change, by: applying DRY, decoupling, use of external configuration, hiding third party APIs behind our own abstraction layers, breaking our code into components.
### 12. Tracer Bullets
- We use the term tracer bullet development to visually illustrate the need for immediate feedback under actual conditions with a moving goal: vague requirements, new technology, large number of unknowns.
- Like the gunners, you are trying to hit a target in the dark.
- Tracer bullets work because they operate in the same environment and under the same constraints as the real bullets. To get the same effect in code, we look for something that gets us from a requirement to some aspect of the final system quickly, visibly, and repeatably.
- Look for the important requirements, the ones that define the system. Look for the areas where you have doubts, and where you see the biggest risks. Then prioritize your development so that these are the first areas you code.
- Tracer code is not disposable.
- Tracer development is consistent with the idea that a project is never finished.
- Tracer code approach has many advantages:
  - Users get to see something working early
  - Developers build a structure to work in
  - You have an integration platform
  - You have something to demonstrate
  - You have a better feel for progress
- Tracer development do not always hit the target: since you use this technique in situations where you are not 100% certain of where you are going, you should not be surprised if your first couple of attempts miss, adjust the target until you hit it, that is the point.
- Tracer code is different than prototyping: prototyping generates disposable code, tracer code is lean but complete and forms part of the skeleton of the final system.
### 13. Prototypes and Post-it Notes
- We build software prototypes to analyze and expose risk, and to offer chances for correction at a greatly reduced cost
- Prototypes are designed to answer just a few questions, so they are much cheaper and faster to develop than applications that go into production
- We can prototype: architecture, new functionality in an existing system, structure or contents of external data, thrid-party tools or components, performance issues, user interface design
- Prototyping is a learning experience, Its value lies not in the code produced, but in the lessons learned.
- When building prototypes we can ignore details such as: correctness, completeness, robustness, style
- You can prototype architecture on a whiteboard, with post-it notes or index cards
- Some areas we can focus when prototyping architecture:
  - Are the responsabilities of the major areas well defined and appropiate?
  - Are the collaborations between major components well defined?
  - Is coupling minimized?
  - Can you identify potential sources of duplication?
  - Are interface definitions and constraints acceptable?
  - Does every module have and access path to the data it needs during execution? Does it have that access when it needs it?
- Before you embark on prototyping make sure that everyone understands that you are writing disposable code, because prototypes can be attractive to people that do not know they are just prototype
### 14. Domain Languages
- Computer languages influence how you think about a program
- Designing a solution with C++ in mind will produce different results than a solution based on Haskell-style thinking
- The language of the problem domain may also suggest a programming solution
- We always try to write code using the vocabulary of the application domain. In some cases, Pragmatic Programmers can go to the next level and actually program using the vocabulary, syntax, and sematics of the language of the domain
- Some domain languages: RSpec, Cucumber, Phoenix Routes, Ansiblei
- There are internal and external domain languages
- RSpec and Phoenix are examples of internal languages
- Cucumber and ansible are examples of external languages
- Internal domain languages can take advantages of the features of its host language, but the downside is that you are bound by the syntax and semantics of the language
- External domain languages have no host-language syntax restrictions since they are an independent language, but you need to write a good parser or use an existing (as ansible did by using yaml)
- There is a simpler way to create internal DSLs: accept some host language syntax and just use functions instead of complext metaprogramming
## Tips


16. Make it easy to reuse
17. Eliminate effects between unrelated things
18. There are no final decisions
20. Use tracer bullets to find the target
21. Prototype to learn
22. Program close to the problem domain
