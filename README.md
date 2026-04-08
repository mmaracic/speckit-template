# README

Project demonstrates speckit setup flow on a development project (both greenfield and brownfield) including extensions.

Check the `notes.md` file for detailed notes on some the topics, problems, errors and open questions that were encountered during the project.

Project implements speckit process using Github Copilot.

> Large projects are usually divided into multiple features and each feature has its own specification. There is also usually a high level specification that defines overall architecture and cross-cutting concerns for the whole project. In the context of speckit, we can call the high level specification as **Umbrella** specification and the feature specifications as **Feature** specifications.

**Umbrella/Project specification** is a high-level document that captures the overall architecture, design principles, and key decisions for the entire project. It serves as a guiding document for the development team and stakeholders, providing a clear vision and direction for the project. It typically includes sections such as architectural overview, technology stack, security requirements, scalability requirements, and other cross-cutting concerns that apply to the entire project.

**Feature specification** is a more detailed document that focuses on a specific feature or set of features within the project. It provides a comprehensive description of the feature's functionality, user stories, acceptance criteria, and any specific design or implementation details related to that feature. Feature specifications are typically created for each major feature or component of the project and are used by the development team to guide the implementation of that specific feature.

> In an enterprise environment there are different roles involved in the project. One of the goals of this project is to explore how different roles can contribute to the speckit process, which commands do they run and how their input can be structured in a way that it can be effectively used by speckit agents to generate constitution, specifications and plans. For that purpose, role files with constitution checklists and role templates with constitution questions are created for different roles (product owner, architect, security expert, etc.) to capture their specific requirements and concerns in a structured way that can be easily processed by speckit agents.

## Project related folder structure

```text
speckit-vote/
├── .github/         # Folder with Github Copilot specific agents, skills and instructions
├── role-files/      # Folder with role files with constitution checklists
├── role-templates/  # Folder with role templates with constitution questions
├── documentation/   # Folder for project/feature documentation
├── .gitignore       # Standard gitignore file with .specify/ folder ignored
└── README.md        # This file
```
Copilot agent named `speckit-role-generator.agent.md` is created to help with generating role and role-template files.

## Steps to setup and use speckit in a project

### 1. Install speckit
Install speckit globally or locally in the project.
For global installation run:
```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### 2. Initialize speckit
Run `specify init .` in the project root to initialize speckit.

### 3. Ignore immutable speckit files
Add standard speckit files to .gitignore

### 4. Obtain project/feature requirements
Obtain project/feature requirements and add them to the documentation folder. Make sure that they are in agent readable markdown format.

### 5. Add role files
Add role template files.
Add role files that will be used by speckit constitution process.

### 6. Generate constitution
Use `/speckit.constitution` command with instructions to process role folder as parameter to generate constitution based on the role files and review it. Forbid usage of documentation folder in the constitution instructions to avoid mixing generated constitution with project/feature documentation.
```
/speckit.constitution Use role files in the role-files/ folder. Do not use files in /documentation/ folder.
```

### 7. Validate constitution
Ensure generated constitution is written to `.specify/memory/constitution.md` and
	synced with `.specify/templates/plan-template.md`,
	`.specify/templates/spec-template.md`, and
	`.specify/templates/tasks-template.md`.

### 8. Generate specification
Use `/speckit.specify` command with instructions to process documentation folder as parameter to generate specification based on the project/feature documentation and review it.
```
/speckit.specify Use documentation files in the documentation/ folder
```
If `/speckit.specify` is called multiple times, new branch will be created every time. Specification (`spec.md`) and checklist (`requirements.md`) for specification will be generated to in the appropriate feature branch folder under `specs/` (e.g., `specs/001-spec-from-docs/`).

### 9. Clarify specification open questions
Use `/speckit.clarify` command to clarify any open questions or unknowns that are blocking implementation. Do it even if the specifiy command claimed to have no open questions that need clarification, as there are often implicit assumptions that need to be surfaced and confirmed.
Run the command multiple times until all open questions are clarified and blockers for implementation are removed.
```
/speckit.clarify
```

### 10. Generate implementation plan
Use `/speckit.plan` command to generate implementation plan based on the specification and constitution. Review the plan and ensure that all mandatory points from the constitution are addressed in the plan.
```
/speckit.plan Use the role files in the role-files/ folder
```

## Problems
* Constitution template is optimized for summarization, not extraction fidelity. It has only 5 principle placeholders and 2 generic sections.
* If points from role file are not mandatory, they are often skipped or not fully addressed. This leads to incomplete constitution and architectural drift. Solution would be to make all points mandatory and require explicit confirmation in the plan checklist that they addressed. This would ensure that all important architectural decisions are made consciously and documented, reducing the risk of drift and misalignment.
* There is no required “source-to-constitution traceability table” placeholder in constitution-template.
* There is no dedicated placeholder for plan-gate checklist carryover from role files n constitution-template.
* There is no placeholder for unresolved mandatory bullets (for example, a FAIL/TODO list) in constitution-template, so omissions can pass silently.
* “Mandatory” at bullet level tells the generator not to skip the item. But inside the item, phrases like “recommend”, “can”, “good default” still signal soft choice, so the model may preserve intent but rewrite/compress details.
* Bullets in the constitution-architect do not have stable IDs (for example ARCH-001), so one-to-one mapping is hard to enforce.
* Section 2 checklist items in constitution-architect are not marked mandatory and are phrased as questions, so they are treated as prompts rather than constitutional obligations.
* It doesnt matter which file is changed - constitution-template or constitution-architect but it is always better to change role file over which we have full ownership to avoid future conflicts with speckit, LLM does not use any inherent precedence rules.

## Errors
* While running `/speckit.constitution` bug https://github.com/github/spec-kit/issues/908 was encountered. Constitution was generated and templates updated, so it seems to be only an inconvenience.

## Open questions
* Should all points in role files be mandatory to ensure they are not skipped in constitution and plan?
* How to include QA experts in the process? Using additional role file that focuses on quality gates? Is that enough or hands on completed application features of QA experts is needed to ensure quality gates are properly defined and implemented?
* At what point are designers included in the process that usually do the work using design tools like Figma? Should they have a separate role file for design related requirements in the constitution? Do we include Figma export at implementation stage? Do we combine both? Is design constitution input to Figma design? Is design done at once or per feature? If per feature, how do we ensure that design is not a bottleneck for development and that it is properly aligned with the overall architecture and requirements?
* How do we handle multi-repo projects? Do we generate one constitution per repo or one for the whole project? If one for the whole project, how do we ensure that it is properly referenced and followed in all repos? How do we handle cross-cutting requirements and concerns like architecture, logging, monitoring and security that may be relevant for the whole project? Monorepo is one solution but it is not always possible or desirable. Maybe we can have a main constitution for the whole project and then have sub-constitutions for each repo that reference the main constitution and add repo-specific requirements and decisions.
* Is the division of user requiremens supposed to be part of speckit process or is it expected that they are divided before the speckit process and then feeded into the speckti process piece by piece in the shape of separate RFCs. How do we handle initial requirements for the project and initial architecture as that should take initial requirements as input? This question is documented in the `notes.md` file. It includes differentiation between Umbrella specification and Feature specification.