# README

Project demonstrates speckit setup flow on a development project (both greenfield and brownfield) including extensions.

Each commit is trying to represent one of the steps in the setup process outlined below.

## Project related folder structure

```text
speckit-vote/
├── role-files/      # Folder with role files with constitution checklists
├── role-templates/  # Folder with role templates with constitution questions
├── documentation/   # Folder for project documentation
├── .gitignore       # Standard gitignore file with .specify/ folder ignored
└── README.md        # This file
```


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

### 4. Obtain initial project requirements
Obtain initial project requirements and add them to the documentation folder. Make sure that they are in agent readable markdown format.

### 5. Add role files
Add role template files.
Add role files that will be used by speckit constitution process.

### 6. Generate constitution
Use `/speckit.constitution` command with instructions to process role folder as parameter to generate constitution based on the role files and review it. Forbid usage of documentation folder in the constitution instructions to avoid mixing generated constitution with project documentation.
```
/speckit.constitution Use role files in the role-files/ folder. Do not use files in /documentation/ folder.
```

### 7. Validate constitution
Ensure generated constitution is written to `.specify/memory/constitution.md` and
	synced with `.specify/templates/plan-template.md`,
	`.specify/templates/spec-template.md`, and
	`.specify/templates/tasks-template.md`.

### 8. Generate specification
Use `/speckit.specify` command with instructions to process documentation folder as parameter to generate specification based on the project documentation and review it.
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

## Errors
* While running `/speckit.constitution` bug https://github.com/github/spec-kit/issues/908 was encountered. Constitution was generated and templates updated, so it seems to be only an inconvenience.
