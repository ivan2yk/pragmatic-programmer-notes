# A Pragmatic Approach

[Back to README](../README.md)
## Key Ideas
### 8. The Essence of Good Design
- A thing is well designed if it adapts to the people who use it, for code it mean it must adapt by changing.
- We believe in ETC principle: Easier To Change.
- A software is well designed if it is Easy To Change.
- Every design principle out there is a special case of ETC:
  - decoupling.
  - single responsability.
  - naming conventions: make the code easy to read so easy to change it.
- Always ask yourself "did the thing I just did make the overall system easier or harder to change?".
### 9. DRY - The Evils of Duplication
- As programmers we work with knowledge: functional and non functional specification that we make it come alive in running code.
- Knowledge is not stable, it changes everytime, and programmers are constantly maintaining knowledge (on maintenance node).
- When we perform maintenance we have to find and change knowledge embedded in the application, the problem is that it is easy to duplicate knowledge on the programs we develop, when we do so, we invite a maintenance nightmare since we have to remember all the places where the knowledge is spread around.
- We believe in DRY principle: Do not Repeate Yourself.
- DRY means: Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.
- More than duplication of code, DRY is about duplication of knowledge, it is about expressing the same thing in two different places, possibly in two different ways.
### 10. Orthogonality
- Orthogonality is a critical concept if you want to produce systems that are easy to design, build, test and extend.
- In computing two or more thing are orthogonal if they are independent or decoupled (changes in one component do not affect any other): in a well designed system, the database code will be orthogonal to the user interface.
- You get two benefits if you write orthogonal systems: gain  productivity and reduce risk.
- Gain productivity: since changes are localized, development and testing time are reduced.
- Reduce risk: diseased sections of code are isolated, changes are localized, easier to design and run tests.
- Some ways you can apply orthogonality are:
  - Design: 
    - Systems should be composed of a set of cooperating modules, each of which implements functionality independent of the others.
    - Layering approach is a powerful way to design orthogonal systems.
    - Test if your design is orthogonal by asking this question: if i change the requirements behind a particular function, how many modules are affected?, the answer should be one.
  - Toolkit and libraries
    - Preserve independence and decoupling of your system as you introduce thrid-party toolkits and libraries.
    - If a toolkit or library imposes changes that should not be there then they are not orthogonal to your system.
    - EJB (enterprise java beans) is an example of orthogonality, it adds functionality behind the escenes without changing your code.
  - Code
    - Constantly monitor what you are doing and also the larger context of your application, verify you are not duplicating knowledge.
  - Documentation
    - Content and presentation should be independent.
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
14. Good design is easier to change than bad design
15. DRY - Do not Repeat Yourself
16. Make it easy to reuse
17. Eliminate effects between unrelated things
18. There are no final decisions
20. Use tracer bullets to find the target
21. Prototype to learn
22. Program close to the problem domain
