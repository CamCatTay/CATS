# C/C++ Ambient Telemetry System (CATS) — Project Plan

> Sends telemetry of outdoor conditions while hiking back to a base station for analysis and logging

**Version:** 1.0  
**Author:** Cameron Taylor 
**Date:** 4/19/2026
**Status:** `Planning`

---

## 1. Problem Statement

This application exists to collect data about the conditions of a trail while on a hike. 
The analysis of this data will provide valuable insights about the conditions of the trail.

---

## 2. Goals

_What does success look like? Be specific._
The end goal of this project should be an indepedent device that wirelessely transmits condition 
data back to a base station from anywhere in the world.

**Primary goals** — the project fails if these aren't met:

- [ ] Goal 1
- [ ] Goal 2

**Secondary goals** — nice to have, but not blockers:

- [ ] Goal 1
- [ ] Goal 2

**Non-goals** — explicitly out of scope (important to write down):

- Thing you are NOT building
- Thing you are NOT trying to solve

---

## 3. Target User / Audience

_Who is this for? Even personal projects should answer this._

|Field|Answer|
|---|---|
|Primary user|Who uses this day-to-day?|
|Technical level|Beginner / intermediate / expert|
|Environment|Where do they use it? (terminal, browser, embedded device, etc.)|
|Key need|What's the one thing they need it to do reliably?|

---

## 4. Functional Requirements

_What must the system **do**? These are behaviors, not implementation details._ _Write each one as: "The system shall [verb] [thing]."_

|ID|Requirement|Priority|
|---|---|---|
|FR-01|The system shall ...|Must|
|FR-02|The system shall ...|Must|
|FR-03|The system shall ...|Should|
|FR-04|The system shall ...|Could|

**Priority scale:**

- **Must** — required for v1.0
- **Should** — strongly desired for v1.0
- **Could** — nice to have, punt to v2.0 if needed

---

## 5. Non-Functional Requirements

_How must the system **behave**? Performance, reliability, constraints._

|Category|Requirement|
|---|---|
|Performance|e.g. "Server must process packets within 10ms"|
|Reliability|e.g. "System must handle dropped packets without crashing"|
|Portability|e.g. "Server must compile on Linux and Windows (WSL)"|
|Security|e.g. "No sensitive data transmitted in plaintext"|
|Constraints|e.g. "No external libraries except X"|

---

## 6. System Architecture

_How does the system fit together at a high level? Draw it in ASCII or describe the components._

```
[Component A] ---protocol/method---> [Component B] ---protocol/method---> [Component C]
```

### Components

_One paragraph per major component. What does it do, what does it own, what does it talk to?_

**Component A:**

> Description of what it does and its responsibilities.

**Component B:**

> Description of what it does and its responsibilities.

---

## 7. Data Design

_What data does the system create, move, or store?_

### Data structures / schemas

_List the key structs, objects, or schemas. Don't need full code — just field names and types._

```
ExampleStruct {
    field_name   : type    // what it represents
    field_name   : type
}
```

### Data flow

_Where does data come from, where does it go, what format is it in at each step?_

```
Source → [format] → Component → [format] → Destination
```

### Storage

_What gets persisted? Where? How long?_

---

## 8. Interface Design

_How do humans or other systems interact with this project?_

### User interface

_Terminal? GUI? Web? Describe what the user sees and does._

### External interfaces

_APIs, hardware, protocols, file formats this connects to._

---

## 9. Build Plan

_Sequenced stages. Build in this order. Don't move to the next stage until the current one is done._

### Stage 1 — [Name]

**Goal:** One sentence on what this stage achieves.  
**Done when:** Specific, testable condition. Not "feels done" — actually verifiable.  
**Unknowns to resolve first:**

- Thing to look up or figure out before starting

### Stage 2 — [Name]

**Goal:**  
**Done when:**  
**Unknowns to resolve first:**

### Stage 3 — [Name]

**Goal:**  
**Done when:**  
**Unknowns to resolve first:**

_(add stages as needed)_

---

## 10. Version Roadmap

_What ships in v1.0 vs later? Decide this upfront so you don't scope creep._

|Version|What's included|Status|
|---|---|---|
|v1.0|Core features — the MVP|`Planned`|
|v2.0|First upgrade / enhancement|`Planned`|
|v3.0|Stretch goals|`Planned`|

---

## 11. Risks & Unknowns

_What could go wrong or block you? Better to think about it now._

|Risk|Likelihood|Impact|Mitigation|
|---|---|---|---|
|Risk description|Low/Med/High|Low/Med/High|How you'd handle it|

---

## 12. Testing Strategy

_How will you know each piece works?_

|Component|Test method|Pass condition|
|---|---|---|
|Component A|Unit test / manual test / simulation|What "pass" looks like|
|Component B|...|...|

---

## 13. Dependencies

_Tools, libraries, hardware, and accounts you need before you can start._

|Type|Name|Purpose|Have it?|
|---|---|---|---|
|Language|C / C++|Core implementation|✅|
|Build tool|CMake|Build system|✅ / ❌|
|Library|[name]|[why]|✅ / ❌|
|Hardware|[part]|[why]|✅ / ❌|
|Account|GitHub|Version control + portfolio|✅ / ❌|

---

## 14. Resume / Portfolio Notes

_Fill this out as you build. Makes writing your resume bullet points easy later._

**One-line project description:**

> [Project name] is a [what it is] built in [languages] that [what it does].

**Key technical decisions worth mentioning:**

- Decision and why you made it
- Decision and why you made it

**Skills demonstrated:**

- Skill → where it shows up in the project

**Metrics to capture** _(numbers make resume bullets stronger)_:

- e.g. packet throughput, latency, lines of code, test coverage %

---

_Template by [Your Name] — reuse freely across projects._
