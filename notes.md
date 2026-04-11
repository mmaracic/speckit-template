# Notes
This file contains more detailed notes to some of the topics, problems, errors and open questions that were encountered during Github spec-kit methodology application in a project.

## On speckit and initial project requirements
Answers obtained from Gemini 3.1 PRO on Google AI Studio on 08.04.2026

> **How would github speckit process (https://github.com/github/spec-kit) handle initial set of project requiremens? Are they all handled through the process itself or is it expected that the initial requirements are divided into manageable subsets before entering the speckit process?**

Based on the philosophy behind GitHub’s Spec-kit (and modern software engineering practices in general), **it is highly expected that initial project requirements are divided into manageable subsets before—or at the very beginning of—entering the Spec-kit process.**

Spec-kit is not designed to ingest a massive, monolithic list of requirements for an entire multi-month project in a single document. Because Spec-kit leverages the GitHub Pull Request (PR) model for review and approval, trying to handle all requirements in one go would break the process. 

Here is a detailed breakdown of how you should handle initial requirements using the Spec-kit methodology:

### 1. Pre-Spec: Product Decomposition (Outside Spec-kit)
Before you write your first Spec-kit markdown file, the raw project requirements usually live in a Product Requirements Document (PRD), a GitHub Issue, or a tracking tool (like Jira or Linear). 
*   **The Action:** Product managers and engineering leads review the initial requirements and break the larger project down into "Epics" or distinct features. 
*   **Why:** A Spec-kit document is meant to be a *technical design* for a solvable problem. If the problem is "Build an entire e-commerce platform," that is too large to design technically in one PR. 

### 2. The "Umbrella" Spec (Optional, for large projects)
If your initial requirements are very large, you can use the Spec-kit template to write a high-level "Umbrella" or "Architecture" spec. 
*   **What it does:** This spec defines the overarching architecture, the global **Goals**, and crucially, the **Non-Goals** for the whole project. 
*   **What it avoids:** It does *not* go into the granular database schemas or API payloads for every single sub-feature. Instead, its "Proposed Solution" section merely lists the sub-systems that will need to be built.

### 3. The Feature Specs (The Core Spec-kit Workflow)
Once the requirements are divided, you put each manageable subset through its own individual Spec-kit process. For example, if your project is a new notification system, you would have separate Spec-kit PRs for:
1.  *Email Delivery Service Spec*
2.  *In-App Notification UI Spec*
3.  *User Preferences Database Spec*

Each of these subsets enters the Spec-kit process independently:
*   **Drafting:** An engineer uses the Spec-kit template to define the context, proposed solution, alternatives, and rollout plan for *just that subset*.
*   **Reviewing:** The team reviews the PR. Because the scope is narrow, the review is fast, focused, and meaningful.
*   **Merging & Coding:** Once approved, the spec is merged, and coding on that specific subset begins.

### Why Monolithic Specs Fail in Spec-kit
If you try to push *all* initial requirements through a single Spec-kit PR, you will encounter several problems that the framework was built to avoid:
*   **Reviewer Fatigue:** A PR containing a 30-page markdown file detailing every facet of a massive project will not get a thorough review. Reviewers will skim it, missing critical technical flaws.
*   **Blocked Development:** If you wait to get approval on the *entire* project's spec before writing code, your engineers will be blocked for weeks. By breaking it into subsets, Team A can start coding Feature A while Team B is still writing the spec for Feature B.
*   **Scope Creep:** Spec-kit relies heavily on the **Goals** and **Non-Goals** sections. If a spec covers too much, it becomes impossible to define what is strictly out of scope.

### Summary
Think of Spec-kit as a funnel that requires well-sized inputs. **You should digest and slice your initial requirements into logical, actionable technical milestones first.** Then, feed each of those milestones into the Spec-kit template as individual Pull Requests. This ensures fast reviews, clear technical direction, and an agile development process.

> **How does constitution fall into this philosophy? Is it allowed to be partial then too?**

In software engineering and the Spec-kit philosophy, a project’s **"Constitution"** typically refers to the core architectural principles, system invariants, shared data models, and non-negotiable constraints (like security or compliance rules) that govern the entire system. 

If you are breaking down requirements into smaller sub-specs, the question of whether the constitution can also be "partial" is a great one. 

The short answer is **yes, it can be partial—but it is better described as "iterative" or "minimally viable."** 

Here is how the concept of a constitution fits into the Spec-kit philosophy and how you should handle it:

### 1. The "Minimally Viable" Constitution (The V1 Umbrella Spec)
While you don't want to design the entire system upfront, you *do* need a set of ground rules so that when Team A writes Spec A and Team B writes Spec B, they don't build incompatible systems. 

Before the sub-specs are written, you should create an **Architecture or Foundation Spec** (your constitution). This spec is "partial" in the sense that it doesn't contain granular details, but it must be "complete" regarding the global rules. It should define:
*   **System Invariants:** (e.g., "All services must communicate via asynchronous events," or "No service can write directly to another service's database.")
*   **Core Technology Stack:** (e.g., "We are using Postgres for relational data and Redis for caching.")
*   **Global Non-Goals:** (e.g., "We will not support on-premise deployments in this system.")

### 2. Amendments via Pull Requests
The beauty of managing specs in GitHub (Spec-kit) is that markdown files are living documents tracked by version control. 

Because your constitution is partial, you will inevitably hit a point where a new sub-feature requires a rule that doesn't exist yet, or breaks an existing rule. 
*   **How it works:** Instead of fighting the constitution, the engineer writes their sub-spec and simultaneously opens a **Pull Request to "amend" the constitution spec**. 
*   **Why this is powerful:** The entire engineering team gets to review the amendment. If it’s approved, the constitution evolves. If it’s rejected, the engineer must rewrite their sub-spec to comply with the existing rules.

### 3. Avoiding "Analysis Paralysis"
If you force your team to write a 100% complete constitution before any sub-specs can be written, you fall back into the trap of the "Waterfall" methodology. You will spend months arguing over edge cases that may never happen. 

By allowing the constitution to be partial:
*   You establish just enough rules to get started safely.
*   You learn what the real architectural constraints are by actually building the sub-components.
*   You defer decisions until the last responsible moment. (e.g., You don't need a constitutional rule on how to handle machine-learning pipelines until you actually reach the milestone of building the ML sub-system).

### Summary
In the Spec-kit workflow, **your constitution should be a living foundation.** It must be complete enough upfront to ensure different subsets of the project integrate successfully, but partial enough that you aren't guessing at future requirements. As the project subsets move through the Spec-kit process, the constitution grows and adapts alongside them via Pull Requests.

> **So the same applies to architecture design? Because big changes in architecture during the course of a project are expensive?**

You have hit on the exact tension that makes software engineering so difficult. You are absolutely right: **big changes in architecture mid-project are massively expensive, and sometimes fatal to a project.**

Because of this, you cannot treat core architecture *exactly* the same way you treat a feature requirement. While you still want to avoid writing a 500-page "Big Design Up Front" document, the approach to architecture in the Spec-kit philosophy requires a critical distinction: **One-Way Doors vs. Two-Way Doors.**

Here is how modern engineering teams use the Spec-kit process to handle architecture design without falling into the trap of expensive mid-course corrections.

### 1. One-Way vs. Two-Way Doors
This concept (popularized by Amazon) is the lens through which you should view your architecture specs.

*   **One-Way Doors:** Decisions that are nearly impossible or extremely expensive to reverse later. *(Examples: Choosing a fundamentally wrong database paradigm like NoSQL vs. Relational; building a monolith vs. microservices; core security and identity models).*
*   **Two-Way Doors:** Decisions that are relatively cheap to change or refactor later. *(Examples: The specific payload of an API endpoint; the internal folder structure of a module; the choice of an internal caching library).*

**The Spec-kit Rule:** All *One-Way Door* architectural decisions must be solidified in an initial, highly-scrutinized "Architecture Spec" before *any* sub-specs are written. *Two-Way Door* decisions can be deferred to the partial sub-specs.

### 2. Macro-Architecture vs. Micro-Architecture
To prevent expensive changes, your initial Architecture Spec must define the **Macro-Architecture** (the skeleton). It is "partial" only in the sense that it lacks granular detail, but it must be structurally sound. 

Your initial Architecture Spec should rigorously define:
*   **System Boundaries:** What services exist and what are their exact responsibilities?
*   **Data Flow & Storage:** Where does the data live? Is it synchronous or asynchronous? 
*   **Protocols:** Are we using REST, GraphQL, or gRPC?
*   **Failure Modes:** What happens if the database goes down?

What it leaves out (to be handled iteratively by the feature Spec-kits):
*   Exact database table schemas.
*   The JSON structures of the API responses.
*   The specific algorithms used inside a given service.

### 3. The Review Process is Different
Because architecture changes are so expensive, an Architecture Spec goes through a different, much heavier review process than a standard feature Spec-kit.

*   **Feature Spec Reviewers:** Usually a Tech Lead and a few peers on the immediate team.
*   **Architecture Spec Reviewers:** Staff/Principal engineers, Security/Infra teams, and Cross-team Tech Leads. 

The goal of the Architecture Spec PR is to have your most experienced engineers actively try to poke holes in the system design *before* a single line of code is written. They are specifically looking for those expensive, hidden traps.

### 4. Prototyping (Spikes) to Reduce Risk
If the team isn't sure about a One-Way Door decision (e.g., "Will this new graph database actually handle our load?"), the Spec-kit process encourages a **Spike**. 

Before approving the Architecture Spec, engineers will write quick, throwaway code (a prototype) to test the risky assumption. Once the prototype proves the architecture is viable, the results are documented in the Spec-kit, the PR is approved, and the throwaway code is deleted.

### Summary
The architecture spec is indeed "partial," but **it is partial in detail, not in structural integrity.** Think of it like blueprints for a house: 

You absolutely must decide where the load-bearing walls and plumbing stacks go before you start building (One-Way Doors). But you don't need to decide what color to paint the kitchen cabinets or what kind of doorknobs to install until much later (Two-Way Doors). 

The Spec-kit process ensures that the load-bearing decisions are documented, reviewed, and locked in first, while deferring the cheaper, granular decisions to the iterative feature specs.

### Links for One Way vs Two Way Doors:
* https://www.inc.com/jeff-haden/amazon-founder-jeff-bezos-this-is-how-successful-people-make-such-smart-decisions.html
* https://medium.com/one-to-n/one-way-two-way-door-decisions-a0e29029e200

### Links for umbrella activities:
* https://www.geeksforgeeks.org/software-engineering/umbrella-activities-in-software-engineering/