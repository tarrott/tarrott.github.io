# Best Practices

## Program Composistion (Design/Architecture)
From John DeNero (UC Berkeley)
- **Names:** To a computer, names are arbitrary symbols: "xegyawebpi" and "foo" are just as meaningful as "tally" and "denominator". To humans, comprehensible names aid immensely in comprehending programs. Choose names for your functions and values that indicate their use, purpose, and meaning. See the lecture notes section on choosing names for more suggestions.
- **Functions:** Functions are our primary mechanism for abstraction, and so each function should ideally have a single job that can be used throughout a program. When given the choice between calling a function or copying and pasting its body, strive to call the function and maintain abstraction in your program. See the lecture notes section on composing functions for more suggestions.
- **Purpose:** Each line of code in a program should have a purpose. Statements should be removed if they no longer have any effect (perhaps because they were useful for a previous version of the program, but are no longer needed). Large blocks of unused code, even when turned into comments, are confusing to readers. Feel free to keep your old implementations in a separate file for your own use, but don't turn them in as your finished product.
- **Brevity:** An idea expressed in four lines of code is often clearer than the same idea expressed in forty. You do not need to try to minimize the length of your program, but look for opportunities to reduce the size of your program substantially by reusing functions you have already defined.

---
## Programmer Competency Matrix
[Competency Matrix](https://sijinjoseph.com/programmer-competency-matrix/)

---
## Manage Growth of Requirements
- track scope creep
- communicate expectations continiously
- maintain a project glossary that defines all terms and how they are used in the project
- go the extra mile and deliver quality of life features

---
## Tracer Bullets
- prototype with a real and simplified version of the project
- it will contain all of the structure, error checking, documentation, and self-checking that production code has, it will not be fully-functional however
- once an end-to-end connection among components of the system is achieved, testing with users can begin
- adding additional functionality afterwards becomes straightforward


---
## Temporal Coupling
- allow for concurrency of tasks not dependent on each other
- reduces time-based dependency and increase flexibility


---
## Dynamic Configuration
- allow for the system to be highly configurable with metadata
    - from screen colors and prompt text to algorithms, database and UI style

---
## Testing

### Unit Tests
- shows examples of how to use all the functionality of a module
- provides a means to build regressions tests to validate further changes to the code
- test subcomponents of a module first before testing the entire module
- use test coverage
    - most importantly test every state of the program rather than every line of code
    - x / (0 to 999) only has two states - testing 0 will cause an error while all 999 other tests will not

### Test Harness
- standard way to specify setup and cleanup
- method for selecting indivdual tests or all available tests
- means of analyzing output for expected or unexpected results
- standardized form of failure reporting and logging status
- xUnit (Kent Beck & Erich Gamma)

### Integration Tests
- shows that major subsystems sthat make up the project work with each other

### End-to-end tests
- does it meet the functional requirements of the system

### Performance testings
- stress test with real-world conditions (number of users/connections/transactions per second)
- how well does it scale?
- test resource exhaustion, errors, and recovery

### Usability testing
- sit down with the end user who will use the system to understand their tasks/goals that the system is trying to help them accomplish
- record requirements, use cases and scenarios
- Alistair Cockburn template:
    - characteristic information (goal, scope, level, preconditions, success/failed end condition, primary actor, trigger)
    - main success scenario
    - extensions
    - variations
    - related information (priority, performance target, frequency, superordinate/subordinate use cases, channel to primary actor, secondary actors)
    - schedule
    - open issues

### TDD

### BDD

### DDD

### Testing in Production
- the final release is sometimes compiled/configured differently from earlier versions and should be tested all over again

### Testing Tests
- introduce bugs and ensure that the tests complain about them
- catch bugs once and then add them into the tests, do not let them reappear uncaught

---
## Refactoring
1. don't refactor and add functionality simultaneously
2. ensure that good tests are present and run them as often as possible
3. take short, deliberate steps and test after each change


### DRY Code

---
## Source Control Management

---
## Documentation & Comments
- comments should discuss *why* something is done, the code already shows *how*
- documentation is another view of the same underlying model
    - incorporate it into the development process, don't let it become an afterthought
- automate html documentation using code generators

---
## API Naming Conventions


---
## Code Generators

#### Passive
- templates for creating new source files
- performing one-off conversions from one language to another
- producing lookup tables and other resources that are expensive to compute at runtime

#### Active
- adhearing to the DRY principle
- allows for a single model to produce multiple views
- getting two disparate environments to work together can benefit greatly from an active code generator
- e.g. a schema ran through a code generator to produce both the software code and documentation in html


---
## Estimating
- check requirements
- analyze risk
- design, implement, integrate
- validate with users
- iterate

1. understand what is being asked
    - understand the scope (assuming these conditions, then...)
    - ask someone else who has done something similar before
    - come back with an answer after making calculations
2. estimate based on a simplified model of the problem
    - the simplified model will inevitably introduce inaccuracies
    - doubling the effort on refining the model might only give a slight increase to accuracy however
3. break the model into components
    - identify their parameters that affect how the component contributes to the overall model
4. assign values to the parameters
    - work out which ones have the most impact on the result and concentrate on getting those right
5. calculate the answers multiple times with varying parameter values
    - hedge the final answer based on conditions
6. the units used make a difference in the interpretation of the result
    - 1-15 days: days
    - 3-8 weeks: weeks
    - 8-30 weeks: months
    - 30+ weeks: think hard before delivering an estimate
7. go back and evaluate the estimate with the reality