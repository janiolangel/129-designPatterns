hi 
# CMSC 129 MIDTERMS — MASTER CHEAT SHEET
### Coverage: Units 1–4 | Exam: April 24, 5:30–7:00PM

> ⚠️ **Prof tip: Majority of points = Design Patterns. All items from PPT slides. Bonus = supplemental repos.**

---

# UNIT 1 QUICK REVIEW — ARCHITECTURAL DESIGN

## The 5 NFRs and Their Architecture Rules

| NFR | Architecture Rule | Analogy |
|---|---|---|
| **Performance** | Fewer, larger components. Keep operations close together. Avoid splitting across many subsystems. | One chef does everything vs. assembly line |
| **Security** | Build in layers. Sensitive parts go deep. Outer layers act as guards. | Castle: outer wall → courtyard → keep |
| **Safety** | Isolate all safety-critical operations in one component. Don't spread safety logic. | One Safety Controller in a microwave |
| **Availability** | Duplicate (redundant) components. No single point of failure. | Spare tire; Load Balancer → Server A ‖ Server B |
| **Maintainability** | Small, self-contained components. Each does one job. No shared data structures. | LEGO bricks — replace one without rebuilding |

## 4 Architectural Views (Krutchen 1995)

| View | Shows | Key Question |
|---|---|---|
| **Logical** | Key abstractions: objects/classes and their relationships | What is the conceptual model? |
| **Process** | Runtime: what's running, what runs concurrently, how they communicate | What is actually executing? |
| **Development** | Code breakdown: modules, packages, team ownership | Who builds what, where does code live? |
| **Physical** | Hardware: where software is deployed, how distributed | Where does the system run? |

## 5 Architectural Patterns

| Pattern | Focuses On | Best For | Key Advantage | Key Disadvantage |
|---|---|---|---|---|
| **MVC** | Separation of responsibilities (who stores/shows/handles) | Web apps | Data and UI change independently | Extra complexity for simple apps |
| **Layered (LAP)** | Top-to-bottom responsibility; each layer only talks to layer below | Secure/maintainable systems | Easy to swap entire layers | Hard to maintain clean separation; performance cost |
| **Repository** | Central shared data store; components don't talk to each other | Data-driven systems, large data volumes | Consistent data, components independent | Repo fails = everything fails |
| **Client-Server** | Who requests vs. who provides services | Distributed systems (Netflix, Gmail, Banking) | Servers can be replicated for availability | Network dependency |
| **Pipe-and-Filter** | Step-by-step data transformation through filters | Data processing (compilers, bioinformatics) | Easy to add/remove steps | Not great for interactive systems |

### MVC vs Client-Server vs Repository (common trick question)
- **MVC** = defines *what each part does* (responsibility)
- **Client-Server** = defines *where things run* (distribution)
- **Repository** = defines *where data lives* (data organization)

---

# UNIT 2 — CREATIONAL DESIGN PATTERNS

## Pattern Overview

| Pattern | Purpose | Problem It Solves | Analogy |
|---|---|---|---|
| **Singleton** | Only ONE instance of a class exists | Multiple instances waste resources / cause data conflicts | ONE task manager for the whole app |
| **Factory** | Create different types of objects via a factory method | Hard-coded object creation; messy if-else for object creation | Notification factory: SMS / Email / Push |
| **Builder** | Build complex objects step-by-step | Too many constructor parameters; complex object initialization | Subway order: +lettuce, +tomato, -onion |

## Singleton — Deep Dive
- **When to use:** One shared resource needed everywhere (e.g., one database connection, one task manager)
- **Steps:** (1) Private instance variable → (2) Private constructor → (3) getInstance() method
- **Key idea:** Prevents multiple instances from being created

## Factory — Deep Dive
- **When to use:** When you need to create objects of different types based on input, and want it to be easily scalable
- **Steps:** (1) Dictionary/map linking types to classes → (2) Static method that looks up and creates → (3) Use factory everywhere instead of direct creation
- **Key idea:** Replaces direct object construction with a factory method call

## Builder — Deep Dive
- **When to use:** When objects have many optional parameters or require complex step-by-step construction
- **Steps:** (1) Builder class holding the object → (2) Setter methods returning self for chaining → (3) build() method that validates and returns → (4) Method chaining
- **Key idea:** Extracts construction code into a separate builder object with named steps

## SEMVER Quick Reference

| Version Part | When to increment | Example |
|---|---|---|
| **MAJOR (X)** | Breaking changes (removed function, changed API, changed params) | 1.4.2 → 2.0.0 |
| **MINOR (Y)** | New feature, backward compatible | 1.4.2 → 1.5.0 |
| **PATCH (Z)** | Bug fix, optimization, refactor | 1.4.2 → 1.4.3 |

- Start at 0.x.x during development
- Release 1.0.0 when stable and public-facing
- Every PR must answer: "Is this change breaking?"
- "^1.4.2" in npm = accepts MINOR and PATCH updates only, NOT MAJOR

---

# UNIT 3 — BEHAVIORAL DESIGN PATTERNS

*"Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects"* — Gang of Four

## Pattern Overview

| Pattern | Purpose | Problem It Solves | Analogy |
|---|---|---|---|
| **Observer** | Notify multiple objects when one object changes (one-to-many) | Customer polling store vs. store spamming all customers | YouTube subscription — notified only when subscribed |
| **Strategy** | Swap algorithms/behaviors AT RUNTIME | Complex if-else logic for different behaviors | Sorting algorithm swapper for To-Do list |
| **Command** | Encapsulate actions as objects | No undo/redo, actions scattered, hard to log/queue | TV remote — each button is a command object |

## Observer — Deep Dive
- **Key roles:** Publisher (Subject) + Subscribers
- **Mechanism:** Publisher has a subscription list; subscribers subscribe/unsubscribe; all subscribers implement the same interface (update())
- **Structure steps:** Publisher issues events → Publisher notifies all subscribers via update() → Subscriber interface declares update() → Concrete subscribers act on notification → Publisher passes context data → Client registers subscribers
- **Also called:** Listener pattern
- **Key idea:** One-to-many dependency; publisher doesn't care who the subscribers are as long as they implement the interface

## Strategy — Deep Dive
- **When to use:** When you need to switch between different algorithms/behaviors at runtime
- **Key idea:** Eliminates long if-else chains by encapsulating each behavior in its own class that can be swapped in/out
- **ITIL connection:** The Strategy pattern was discussed alongside ITIL Emergency Change Management — key principle: *"The risk of an emergency change must never exceed the risk it is trying to prevent."*

## Command — Deep Dive
- **When to use:** When you need undo/redo, action logging, queuing, or macros
- **Key roles:** Invoker (remote) + Command object + Receiver (TV)
- **Key idea:** The invoker doesn't know HOW to perform the action — it just executes the command object. The receiver knows how.
- **Powers:** Logging, undo/redo, macro (one button = multiple commands), queuing (like Karaoke reserve)

---

# UNIT 4 — STRUCTURAL DESIGN PATTERNS

*"Structural patterns are concerned with how classes and objects are composed to form larger structures."* — Gang of Four

## Pattern Overview

| Pattern | Purpose | Problem It Solves | Analogy |
|---|---|---|---|
| **Adapter** | Make incompatible interfaces work together | Can't integrate third-party library (XML app + JSON library) | Power plug adapter |
| **Decorator** | Add behaviors/features to objects dynamically at runtime | Subclass explosion when combining features | Clothes layering; Coffee + milk + foam = latte |
| **Façade** | Provide simple interface to complex subsystem | Code needs to know/interact with too many subsystems | TV remote (hides circuits); Lazada checkout() |

## Adapter — Deep Dive
- **When to use:** Third-party library integration, legacy systems, mismatched interfaces
- **Key idea:** Wrap the incompatible object in an adapter. The adapter translates calls. The wrapped object is unaware of the adapter (loose coupling).
- **To-Do example:** Google Calendar + Outlook + Apple Calendar each have different APIs → Create common CalendarInterface, write adapters for each

## Decorator — Deep Dive
- **When to use:** Adding features dynamically, avoiding subclass explosion, extending behavior at runtime
- **Key idea:** Instead of inheritance (which causes subclass explosion), use composition/aggregation — wrap the object in decorator objects
- **Solution:** Aggregation/Composition instead of OOP Inheritance
- **To-Do example:** EncryptedTask, LoggedTask, TimestampedTask — instead of making subclasses for every combination, wrap the task with decorator objects

## Façade — Deep Dive
- **When to use:** Simplify complex systems, provide a clean API, reduce dependencies
- **Key idea:** Create one simple interface that hides and coordinates all the complex subsystems behind it
- **To-Do example:** facade.create_and_schedule_task(name, date) — internally handles: create task + validate + save to db + sync calendar + send notif + log action (6 subsystems)

## Structural Summary Table

| Pattern | One Word | What It Hides/Provides |
|---|---|---|
| **Adapter** | Compatibility | Translates between incompatible interfaces |
| **Decorator** | Flexibility | Adds behavior dynamically via wrapping |
| **Façade** | Simplicity | Hides complex subsystems behind one clean interface |

---

# ALL 6 DESIGN PATTERNS — MASTER COMPARISON

| Category | Pattern | Purpose (1 line) | Key Word | Analogy |
|---|---|---|---|---|
| **Creational** | Singleton | One instance only | ONE | One task manager |
| **Creational** | Factory | Create objects by type | CREATE | Notification factory |
| **Creational** | Builder | Build complex objects step-by-step | BUILD | Subway order |
| **Behavioral** | Observer | Notify many when one changes | NOTIFY | YouTube subscription |
| **Behavioral** | Strategy | Swap behaviors at runtime | SWAP | Sorting algorithm |
| **Behavioral** | Command | Encapsulate actions as objects | ENCAPSULATE | TV remote |
| **Structural** | Adapter | Make incompatible things work | TRANSLATE | Power plug adapter |
| **Structural** | Decorator | Add behavior dynamically | WRAP | Clothes layering |
| **Structural** | Façade | Simple interface to complex system | SIMPLIFY | TV remote / checkout() |

---

# BONUS — CHECK THE REPOS
The prof said bonus questions are hidden in the supplemental repos. Check:
- `korielcode/CMSC129_Lecture_Unit1_FERN-MVC_Example`
- Look for Unit 2, 3, 4 repos in the same GitHub account — the slides say "check the repo for example" for Factory, Builder, Observer, and Strategy
