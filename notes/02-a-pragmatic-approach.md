# A Pragmatic Approach
## Key Ideas
### The Essence of Good Design
- A thing is well designed if it adapts to the people who use it, for code it mean it must adapt by changing.
- We believe in ETC principle: Easier To Change.
- A software is well designed if it is Easy To Change.
- Every design principle out there is a special case of ETC:
  - decoupling.
  - single responsability.
  - naming conventions: make the code easy to read so easy to change it.
- Always ask yourself "did the thing I just did make the overall system easier or harder to change?".
### DRY - The Evils of Duplication
- As programmers we work with knowledge: functional and non functional specification that we make it come alive in running code.
- Knowledge is not stable, it changes everytime, and programmers are constantly maintaining knowledge (on maintenance node).
- When we perform maintenance we have to find and change knowledge embedded in the application, the problem is that it is easy to duplicate knowledge on the programs we develop, when we do so, we invite a maintenance nightmare since we have to remember all the places where the knowledge is spread around.
- We believe in DRY principle: Do not Repeate Yourself.
- DRY means: Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.
- More than duplication of code, DRY is about duplication of knowledge, it is about expressing the same thing in two different places, possibly in two different ways.
### Orthogonality
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
## Tips
14. Good design is easier to change than bad design
15. DRY - Do not Repeat Yourself
16. Make it easy to reuse
17. Eliminate effects between unrelated things
