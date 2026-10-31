# Software as Art: Sculpting Elegant Solutions in C#

*A two-day, hands-on workshop for junior .NET developers. Draft outline v2.*

**Repo:** `software-as-art`\
**Audience:** Developers with some C# / .NET experience who are ready to move from "code that works" to "code that's crafted."\
**Format:** Two days, about 6.5 hours of content each, alternating short talks with immediate labs, with one running project and a capstone.

## 1. Workshop Overview

### Core Idea

Code is read far more often than it is written. Treating software as a craft means making deliberate choices about composition, restraint, and intent so the next person (often future you) can understand and change it. This workshop teaches the tools of that craft: interfaces, dependency injection, a handful of classic patterns, and SOLID as a diagnostic vocabulary.

### Learning Objectives

By the end, participants can:

1. Explain what makes code readable, changeable, and well-crafted, not just working.
2. Design and use interfaces to define seams between components.
3. Wire dependencies with constructor injection and the .NET DI container.
4. Recognize when Strategy, Adapter, Decorator, or Mediator fits, and when it doesn't.
5. Use SOLID as a diagnostic lens rather than a rulebook.
6. Package an abstraction and its implementations as separate NuGet packages.

### Prerequisites

- Comfortable writing classes, methods, and basic LINQ in C#
- Familiar with using NuGet packages (authoring is taught)
- Laptop with the current .NET LTS SDK (verify the version the repo targets), an IDE (Visual Studio, Rider, or VS Code with C# Dev Kit), and Git

### Teaching Principles

- **Problem first, pattern second.** Show the pain, then introduce the named solution.
- **Talk, then lab.** Every concept is practiced within minutes of being taught.
- **One running project.** Participants watch the same code transform over two days.
- **Restraint is part of the art.** Over-engineering is kitsch. Each pattern is introduced with when *not* to use it.
- **Tiered labs.** Core, Stretch, and Extra Credit tasks so faster and slower participants both stay engaged.

## 2. Running Project and Lab Domains

A small, relatable domain keeps cognitive load low. Proposed running project: **Atelier Orders**, a console app that takes orders, calculates prices and discounts, sends confirmations, and exports reports. It starts as one tangled class and is sculpted a little more each lab.

| Lab | Focus | Domain |
| --- | --- | --- |
| 1 | Find the Seam (interfaces) | Order confirmation: email, file output, DB access hard-wired |
| 2 | Wiring It Up (DI) | Compose Lab 1's code with the .NET DI container |
| 2b | Testing Through the Seams | Hand-rolled fakes and unit tests |
| 3 | Recognize and Implement Strategy | Discount/shipping calculator with a giant conditional |
| 4 | Wrap What You Don't Own (Adapter) | Awkward third-party "payment/email SDK" |
| 5 | Layer It On (Decorator, optional Mediator) | Logging, caching, retry |
| Capstone | Build a NuGet package ecosystem | `Atelier.Notifications` core + adapter packages |

## 3. Two-Day Schedule at a Glance

### Day 1: Foundations (9:00 to 16:00)

| Time | Block | Type |
| --- | --- | --- |
| 9:00 to 9:20 | Welcome and setup | Intro |
| 9:20 to 10:20 | Module 1: Software as Art + Gallery Walk | Talk and activity |
| 10:20 to 10:35 | Break |  |
| 10:35 to 11:15 | Module 2: Interfaces, the Seams of Your Code | Talk |
| 11:15 to 12:00 | Lab 1: Find the Seam | Lab |
| 12:00 to 12:45 | Lunch |  |
| 12:45 to 13:30 | Module 3: Dependency Injection | Talk |
| 13:30 to 14:30 | Lab 2: Wiring It Up | Lab |
| 14:30 to 14:45 | Break |  |
| 14:45 to 15:30 | Lab 2b: Testing Through the Seams | Lab |
| 15:30 to 16:00 | Reflection, homework, Q&A | Wrap-up |

### Day 2: Patterns and Craft (9:00 to 16:30)

| Time | Block | Type |
| --- | --- | --- |
| 9:00 to 9:25 | Recap and warm-up | Activity |
| 9:25 to 10:15 | Module 4: Strategy and Adapter | Talk |
| 10:15 to 10:30 | Break |  |
| 10:30 to 11:15 | Lab 3: Recognize and Implement Strategy | Lab |
| 11:15 to 12:00 | Lab 4: Wrap What You Don't Own | Lab |
| 12:00 to 12:45 | Lunch |  |
| 12:45 to 13:25 | Module 5: Decorator and Mediator | Talk |
| 13:25 to 14:00 | Lab 5: Layer It On | Lab |
| 14:00 to 14:30 | Module 6: SOLID as a Diagnostic Lens | Talk and activity |
| 14:30 to 14:45 | Break |  |
| 14:45 to 16:00 | Module 7: Capstone, Build a NuGet Package Ecosystem | Lab |
| 16:00 to 16:30 | Module 8: Showcase and closing | Presentations |

## 4. Day 1 in Detail

### Module 0: Welcome and Setup (20 min)

- Introductions. Icebreaker: *"Where does your code feel messiest right now?"* Capture answers on a whiteboard to revisit at the end of Day 2.
- Environment check: .NET SDK, IDE, clone `software-as-art`, run `dotnet build` and `dotnet test` on the smoke-test project.
- Explain the arc of the two days, the running project, and how labs and the `starter` / `solution` folders work.
- Ground rules: ask questions anytime, pair up, no shame in jumping to a solution folder.

### Module 1: Software as Art (45 min talk + 15 min activity)

**Goal:** Reframe code as something made with intention, for human readers.

**Talk outline**

1. **The case for the metaphor.** Code is read far more than written. Craft is about the reader's experience and the next person's changes.
2. **Elements of art in code**
   - *Composition:* how pieces fit (cohesion and coupling).
   - *Restraint and negative space:* what you leave out matters.
   - *Constraints:* art thrives within boundaries, and so do good designs.
   - *Intent:* names and structure that communicate *why*.
3. **Anti-art:** the god class, the 600-line method, `new` everywhere. Show a real, anonymized ugly example (or the Atelier Orders starting code).
4. **The caveat.** Over-engineering is also bad art (kitsch). Patterns serve the code, not the reverse.
5. **Roadmap:** preview the tools coming over the next two days.

**Activity: Code Gallery Walk (15 min)**

- Provide 3 or 4 snippets of varying quality (printed, or in `/gallery`).
- In pairs, critique them like art: What's the intent? What feels awkward? What would you change?
- Capture their vocabulary on a whiteboard. **Save the snippets and the notes**: you will re-critique them at the end of Day 2.

**Possible references:** *Hackers & Painters* (Paul Graham), *The Pragmatic Programmer*, Sandi Metz's talks on small objects, *A Philosophy of Software Design* (Ousterhout).

### Module 2: Interfaces, the Seams of Your Code (40 min talk)

- **What an interface is:** a contract and a boundary, not "a class with no body."
- **Programming to abstractions vs. concretions.** Show the same class both ways.
- **Designing good interfaces:** small, role-focused, named for capability (`IPriceCalculator`, not `IPriceCalculatorService`). Who owns the interface? The consumer.
- **Common mistakes:** interface-per-class by reflex, leaky abstractions (a SQL-shaped interface hiding behind "repository"), fat interfaces.
- **C# specifics:** default interface methods (use sparingly), explicit implementation, generic interfaces, interface vs. abstract class, `IDisposable`/`IAsyncDisposable` as familiar examples.
- **Testability preview:** a seam is a place where you can substitute behavior.
- **Live-coding segment (10 min):** take a method that calls `File.WriteAllText` and `DateTime.Now` directly and extract the seam.

### Lab 1: Find the Seam (45 min)

**Starter:** `OrderConfirmationService` directly creates and uses `SmtpClient`, `SqlConnection`, `File.WriteAllText`, and `DateTime.Now`.

| Tier | Task |
| --- | --- |
| Core | Identify the dependencies, extract small interfaces, refactor the class to depend on them (still created manually for now). |
| Stretch | Split any fat interface using the Interface Segregation idea. |
| Extra credit | Write a hand-rolled fake for each interface and run the class without real I/O. |

**Checkpoint:** "Show me your interface." A quick pair review before moving on.\
**Discussion prompt:** How did you decide the shape of each interface? What did you intentionally leave out?

### Module 3: Dependency Injection (45 min talk)

- **Define carefully (a common confusion):**
  - *Dependency Inversion Principle:* a design idea about the direction of dependencies.
  - *Dependency Injection:* a technique, supplying dependencies from outside.
  - *IoC container:* a tool that automates the wiring.
- **Constructor injection as the default;** why property and method injection are rarer.
- **`Microsoft.Extensions.DependencyInjection`:** `IServiceCollection`, `IServiceProvider`, registering and resolving.
- **Lifetimes:** Transient, Scoped, Singleton, and the **captive dependency** trap.
- **The composition root:** where wiring happens, and why the rest of the app shouldn't know the container exists.
- **Anti-patterns:** Service Locator, injecting `IServiceProvider` everywhere, constructor over-injection (a smell that a class does too much).
- **Brief mentions:** Options pattern (`IOptions<T>`), keyed services, `HostBuilder` / generic host.
- **Live-coding segment (10 min):** replace an implementation by changing one registration, with the consuming code untouched.

### Lab 2: Wiring It Up (60 min)

Continue the Atelier Orders project from Lab 1.

| Tier | Task |
| --- | --- |
| Core | Set up a console host, register services, inject via constructors, and run the app from a single composition root. |
| Stretch | Swap implementations through `appsettings.json` (for example, file confirmation vs. email confirmation). |
| Stretch | Deliberately create a captive dependency (a singleton holding a scoped service) and diagnose it with scope validation on. |
| Extra credit | Write an `AddAtelierOrders(this IServiceCollection)` extension method. This seeds the capstone. |

**Checkpoint:** "Show me your composition root." Can they point to exactly one place where `new` is used for services?

### Lab 2b: Testing Through the Seams (45 min)

Makes the payoff of interfaces and DI tangible.

- Add an xUnit test project (provided) and test `OrderConfirmationService` using fakes written by hand.
- Compare a hand-rolled fake with a mocking library (NSubstitute or Moq, instructor's choice; verify the current license and trust posture of whichever you choose).
- Discussion: What does it feel like to test the "before" version versus the "after"?

### Day 1 Wrap-Up (30 min)

- **Reflection (10 min):** Journal or sticky note: "What's one place in my codebase that needs a seam?"
- **Homework (optional, 30 to 45 min):** Find one real class at work or in a side project that does `new` on a dependency. Sketch (don't necessarily implement) the interface and injection. Bring it to Day 2 warm-up.
- **Preview Day 2:** patterns as named solutions to recurring problems.

## 5. Day 2 in Detail

### Recap and Warm-Up (25 min)

- Share homework in pairs (5 min), then 3 volunteers share with the room.
- Rapid-fire quiz: what's the difference between DIP, DI, and a container? What's a captive dependency?
- Preview: today's patterns are *named solutions*. We start from the problem each time.

### Module 4: Patterns Part 1, Strategy and Adapter (50 min talk)

Frame patterns as **named solutions to recurring problems**, introduced via the *problem first*.

**Strategy**

- *Problem signal:* growing `switch` / `if-else` chains choosing behavior; "this algorithm varies."
- *Structure:* a strategy interface plus interchangeable implementations.
- *C# with DI:* inject `IEnumerable<IDiscountStrategy>` and select by key or by `CanApply()`; a lighter-weight option using `Func<>` delegates.
- *When not to use it:* two cases that will never grow. A simple `if` is fine.

**Adapter**

- *Problem signal:* a class you don't own (third-party SDK, legacy code, framework class) has the wrong shape for your code.
- *Technique:* define the interface you wish you had, then wrap the awkward class to implement it. Keep vendor types out of your core.
- *Bonus value:* the wrapper is a place for extra functionality (validation, mapping, error translation, retries) and shields you from breaking changes.
- *When not to use it:* thin, stable, framework-owned types that are already abstractions.
- **Live-coding segment (10 min):** wrap a static, awkward class behind an interface and register the adapter in DI.

### Lab 3: Recognize and Implement Strategy (45 min)

**Starter:** a pricing/discount/shipping calculator dominated by a large conditional.

| Step | Task |
| --- | --- |
| 1. Smell spotting (pairs, 10 min) | Where does the code want to be a strategy? Mark the variation points. |
| 2. Refactor | Extract the strategy interface and implementations; register in DI. |
| 3. The Open/Closed moment | Add a new rule (for example, a seasonal discount) *without modifying existing code*. |
| Stretch | Compare an interface-based strategy to a `Func<>`-based one. When is each better? |
| Extra credit | Add a tie-break/priority mechanism for strategies that can overlap. |

**Discussion:** What would have happened if we'd added the fifth `else if`? Where is the line between "just use an if" and "use a strategy"?

### Lab 4: Wrap What You Don't Own (45 min)

**Starter:** `Vendor.LegacyPayments` (a local library participants cannot modify, ideally shipped as a compiled DLL or package) with sealed classes, static methods, odd naming, and surprising exceptions.

| Tier | Task |
| --- | --- |
| Core | Define your own interface, write the adapter, and map vendor types and exceptions to your own. |
| Stretch | Add extra behavior inside the adapter: input validation, retry on transient failure, friendlier exceptions. |
| Extra credit | Write tests for the adapter against the vendor class. |

**Discussion:** What does your adapter hide, and what did you choose to expose? What happens when the vendor ships a breaking change?

### Module 5: Patterns Part 2, Decorator and Mediator (40 min talk)

**Decorator**

- *Problem signal:* cross-cutting concerns (logging, caching, retry, metrics) cluttering core logic.
- *Technique:* wrap an interface with the same interface; stack decorators.
- *DI registration:* manual composition, or the Scrutor library's `Decorate` extension (worth mentioning).
- **Link back to Adapter:** the wrapping mechanics are the same, but the intent differs (translate vs. enhance). Ask participants to articulate this difference as a comprehension check.

**Mediator**

- *Problem signal:* many components talking directly to each other, with tangled dependencies.
- *Technique:* centralize communication; request/handler decoupling; pipeline behaviors as a decorator-like concept.
- **Practical note:** MediatR is the best-known library, but it has moved to a commercial license. Verify current licensing before using it in the lab. A hand-rolled mini-mediator (about 50 lines) is a great teaching exercise and avoids the dependency.
- *Honest tradeoffs:* indirection makes flow harder to trace. Don't use it just because it's popular.

### Lab 5: Layer It On (35 min)

| Tier | Task |
| --- | --- |
| Core | Add logging and caching to an existing service using decorators, without editing the service. |
| Stretch | Stack decorators and observe how registration order changes behavior. |
| Extra credit | Build a minimal mediator and move two components onto it, with a logging pipeline behavior. |

### Module 6: SOLID as a Diagnostic Lens (30 min)

Because participants have now *felt* these ideas, the principles land as names for things they already did.

| Principle | Where they experienced it |
| --- | --- |
| **S**ingle Responsibility | Why was the original class hard to change? (Lab 1) |
| **O**pen/Closed | Adding a strategy without modification (Lab 3) |
| **L**iskov Substitution | Demo: a decorator or implementation that quietly breaks the contract |
| **I**nterface Segregation | Slimming down fat interfaces (Lab 1) |
| **D**ependency Inversion | DI and abstractions (Lab 2) |

- **Activity (10 min):** map each principle to a moment from the two days on sticky notes.
- **Closing warning:** SOLID is a set of heuristics. Applied dogmatically, it produces over-abstracted code.

### Module 7: Capstone, Build a NuGet Package Ecosystem (75 min)

**Scenario:** participants are library authors. They design a core abstraction with swappable implementations shipped as separate packages.

**Example: `Atelier.Notifications`**

- `Atelier.Notifications`: core interfaces, models, `AddNotifications()` DI extension, and one or two decorators (retry, logging).
- `Atelier.Notifications.Email`: adapter package wrapping an email SDK.
- `Atelier.Notifications.Sms`: adapter package wrapping a second provider.
- A consumer console app references the core plus one adapter and swaps providers through configuration.

**Steps**

1. Design the core interface(s), using strategy and adapter thinking.
2. Create adapter packages that reference *only* the core.
3. Pack locally and consume from a **local NuGet feed** (no need to publish to nuget.org).
4. Provide `IServiceCollection` extension methods for each package (`AddEmailNotifications`, `AddSmsNotifications`).
5. Swap implementations in the consumer without changing consumer code.

| Tier | Task |
| --- | --- |
| Core | Steps 1 to 5 with the provided fake vendor SDKs. |
| Stretch | Add a retry or logging decorator in the core package and apply it automatically in `AddNotifications()`. |
| Extra credit | Add a third adapter, or multi-target the core package. |

**Discussion:** package boundaries, dependency direction (adapters depend on the core, never the reverse), and the versioning implications of changing an interface (semantic versioning, breaking changes).

### Module 8: Showcase and Closing (30 min)

- **Showcase (15 min):** groups present their capstone design in about 3 minutes each: the core interface, what they hid, and what they'd change.
- **Gallery revisited (5 min):** re-critique the Module 1 snippets using the new vocabulary. Compare with the original notes.
- **Revisit the "messy code" whiteboard** from the icebreaker. Which problems now have a named solution?
- **Closing reflection (5 min):** "What's one place in your current code where you'll apply this on Monday?"
- **Resources:** *Dependency Injection Principles, Practices, and Patterns* (Seemann and van Deursen), *Head First Design Patterns*, *Refactoring* (Fowler), *A Philosophy of Software Design* (Ousterhout), and links to follow-up reading.
- Feedback form.

## 6. Repository Design: `software-as-art`

### Repo Settings

- **Name:** `software-as-art`
- **Description:** "Software as Art: Sculpting Elegant Solutions in C#. A hands-on C# / .NET workshop on interfaces, dependency injection, design patterns, and SOLID."
- **Topics (tags):** `csharp`, `dotnet`, `design-patterns`, `solid`, `workshop`
  - Optional extras: `dependency-injection`, `clean-code`, `software-design`, `nuget`, `training`
- **License:** MIT (or your organization's standard) for code; consider CC BY 4.0 for slides and docs.
- **Template repository:** consider enabling this so attendees can create their own copy.

### Proposed Folder Structure

```
software-as-art/
├── README.md
├── LICENSE
├── global.json                  # pins the .NET SDK version
├── nuget.config                 # includes the local feed (./local-feed)
├── Directory.Build.props        # shared settings (nullable, analyzers, LangVersion)
├── Directory.Packages.props     # central package management
├── .editorconfig
├── .gitignore
├── SoftwareAsArt.sln
├── .github/
│   └── workflows/
│       └── build.yml            # builds and tests every starter and solution
├── docs/
│   ├── 00-setup.md
│   ├── agenda-day-1.md
│   ├── agenda-day-2.md
│   ├── facilitator-guide.md
│   ├── glossary.md
│   ├── resources.md
│   └── slides/                  # decks or links to them
├── gallery/                     # Module 1 code snippets for the Gallery Walk
│   ├── snippet-a/
│   ├── snippet-b/
│   └── snippet-c/
├── vendor/                      # "third-party" code participants can't modify
│   └── Vendor.LegacyPayments/
├── local-feed/                  # local NuGet feed for the capstone (gitignored contents)
├── labs/
│   ├── lab-00-smoke-test/
│   ├── lab-01-find-the-seam/
│   │   ├── README.md            # instructions, tiers, checkpoints
│   │   ├── starter/
│   │   └── solution/
│   ├── lab-02-wiring-it-up/
│   │   ├── README.md
│   │   ├── starter/
│   │   └── solution/
│   ├── lab-02b-testing-through-the-seams/
│   ├── lab-03-strategy/
│   ├── lab-04-adapter/
│   ├── lab-05-decorator-mediator/
│   └── lab-06-capstone-nuget-ecosystem/
│       ├── README.md
│       ├── starter/
│       │   ├── Atelier.Notifications/
│       │   ├── Atelier.Notifications.Email/
│       │   ├── Atelier.Notifications.Sms/
│       │   └── Atelier.Consumer/
│       └── solution/
└── examples/                    # instructor live-coding demos (before and after)
```

### Conventions

- **Consistent naming:** `lab-NN-short-name/starter` and `solution`, with a README in every lab.
- **Folders over branches for starters and solutions.** Participants can peek at the solution folder without switching branches, and each lab builds standalone. Optionally tag a release (`v1.0`) before each delivery so attendees can pin a version.
- **Chained labs:** each lab's starter is the previous lab's solution, so participants who fall behind can restart from a known-good point.
- **Namespaces:** `Atelier.*` for the running project and capstone packages, reinforcing the studio metaphor.
- **Lab README template:** Goal, Concepts Practiced, Before You Start, Core Tasks, Stretch, Extra Credit, Checkpoint Questions, Discussion Prompts, Hints (collapsible), and What Good Looks Like.
- **Hints:** use GitHub `<details>` blocks for progressive hints.
- **Pinning:** `global.json` for the SDK, `Directory.Packages.props` for package versions, so a fresh clone builds identically everywhere.
- **CI:** a GitHub Actions workflow that builds and runs tests for every starter (expected failures allowed, or tests marked as skipped) and every solution. This catches breakage when SDKs or packages change.
- **Local NuGet feed:** `nuget.config` points at `./local-feed` plus nuget.org; a script (`pack-all.ps1` / `pack-all.sh`) packs the capstone projects into it.

### README Outline

1. Title, one-line pitch, and the "Software as Art" mantra
2. Who it's for and prerequisites
3. Quick start: clone, `dotnet --version` check, `dotnet build`, smoke test
4. Agenda overview (Day 1 and Day 2) with links to labs
5. How labs work (starter, solution, tiers, checkpoints)
6. Running project explainer (Atelier Orders)
7. Facilitator notes link
8. Resources, license, and contributing

## 7. Facilitator Prep Checklist

- **Test every lab on a clean machine** (Windows, macOS, and Linux if mixed). NuGet and local-feed setup is the classic failure point.
- **Setup email one week before:** SDK version, IDE, repo link, and a request to run the smoke test in advance.
- **Offline fallback:** pre-seed a zip of the repo with restored packages in case venue Wi-Fi fails.
- **Pairing plan:** pair for Labs 1 to 4 to lower intimidation; allow solo or group work in the capstone.
- **Pacing guardrails:** keep each talk under about 45 minutes with a live-coding segment; if a lab runs long, cut Stretch tasks first, never the checkpoint.
- **Cut list if time runs short** (in order): Mediator extra credit, Lab 2b, Lab 5 stretch, capstone extra credit.
- **Licensing check:** verify the current licenses of MediatR, mocking libraries, and any other third-party packages before the workshop.
- **Materials:** whiteboard or flipchart, sticky notes, printed gallery snippets, and a feedback form.

## 8. Assessment and Feedback

- **During the workshop:** checkpoints in every lab ("Show me your interface," "Show me your composition root") serve as lightweight formative checks.
- **Comprehension checks:** the Adapter vs. Decorator distinction, and the DIP / DI / container distinction.
- **After the workshop:** a short survey (confidence before and after, most and least useful sections), plus an optional 30-day follow-up asking whether they've applied anything.

## 9. Variants and Future Ideas

- **Condensed one-day version:** Modules 1 to 4, Labs 1 to 4, and a lightweight capstone demo. Move Decorator, Mediator, and SOLID to a follow-up.
- **Four half-days over two weeks:** best for retention and remote delivery.
- **Extensions:** an Advanced Day 3 (Specification, Factory, Options pattern, Result types, hexagonal architecture), an ASP.NET Core track (middleware as decorators), and an Art-themed capstone showcase night.
- **Other language tracks:** the repo could later add Java or TypeScript variants.

## 10. Open Questions

1. In person, remote, or hybrid? (Affects pacing, pairing, and tooling.)
2. Expected group size? (Determines how many facilitators or TAs you need for lab support.)
3. Which IDE will participants use? (Instructions in the README should match.)
4. Slide tooling: PowerPoint, Reveal.js in the repo, or something else?
5. Do you want to run labs on the latest .NET LTS, or keep a version buffer for company machines?

## 11. Suggested Next Steps

1. Confirm the running project domain (Atelier Orders) and the capstone domain (Notifications).
2. Scaffold the repo structure and CI.
3. Write Lab 1 first, since it sets the tone and tests the format, then pilot it with one or two colleagues.
4. Draft slides module by module.
5. Run a full pilot, even compressed, before the first delivery.
