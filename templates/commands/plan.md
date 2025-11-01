---
description: Execute the implementation planning workflow using the plan template to generate design artifacts.
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. **Setup**: Run `{SCRIPT}` from repo root and parse JSON for FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

2. **Load context**: Read FEATURE_SPEC and `/memory/constitution.md`. Load IMPL_PLAN template (already copied).

3. **Load Persona Configuration** (if available):
   - Check if `.specify/config.json` exists
   - If exists, read the `personas.enabled` list
   - Load persona definitions from `memory/personas/` for enabled personas
   - Identify which personas contribute to the `plan` phase:
     - **Solution Architect (SA)**: Architecture design, technology stack, data models, API contracts
     - **Tech Lead (TL)**: Implementation strategy, code organization, development workflow
     - **DevOps Engineer**: Infrastructure design, deployment strategy, monitoring and logging
     - **Security Engineer**: Security architecture, authentication/authorization, data protection
     - **UX Designer**: Interaction design, UI patterns, responsive design
     - **Frontend Developer (FE)**: Frontend architecture, component design, state management
     - **Backend Developer (BE)**: Backend architecture, API design, database schema
   - If no persona configuration exists, proceed with standard single-agent workflow

4. **Execute plan workflow**: Follow the structure in IMPL_PLAN template to:
   - Fill Technical Context (mark unknowns as "NEEDS CLARIFICATION")
   - Fill Constitution Check section from constitution
   - Evaluate gates (ERROR if violations unjustified)
   - Phase 0: Generate research.md (resolve all NEEDS CLARIFICATION)
   - Phase 1: Generate data-model.md, contracts/, quickstart.md
   - Phase 1: Update agent context by running the agent script
   - Re-evaluate Constitution Check post-design

5. **Orchestrate Persona Contributions** (if personas enabled):
   
   If personas are enabled, coordinate their contributions in parallel where possible:
   
   a. **Solution Architect (SA)** - Lead architect:
      - Design overall system architecture (monolith, microservices, serverless, etc.)
      - Select technology stack with clear rationale
      - Design data models and entity relationships
      - Create API contracts (REST, GraphQL, gRPC)
      - Define integration patterns and external dependencies
      - Establish architectural patterns and principles
      - Document technical trade-offs and alternatives
   
   b. **Tech Lead (TL)** - Implementation strategy:
      - Define implementation approach (TDD, BDD, iterative)
      - Establish coding standards and conventions
      - Design code organization and module structure
      - Plan for code reusability and modularity
      - Define development workflow and branching strategy
      - Establish error handling and logging strategies
      - Plan for testing strategy (unit, integration, e2e)
   
   c. **DevOps Engineer** (if enabled):
      - Design infrastructure and hosting requirements
      - Select cloud providers and services
      - Plan CI/CD pipeline architecture
      - Define deployment environments (dev, staging, production)
      - Establish monitoring and logging strategy
      - Plan for disaster recovery and backup
      - Define infrastructure as code approach
   
   d. **Security Engineer** (if enabled):
      - Design security architecture and controls
      - Plan authentication and authorization mechanisms
      - Define encryption requirements (at rest, in transit)
      - Establish key management strategy
      - Design for compliance requirements
      - Plan security testing and vulnerability scanning
      - Define audit logging requirements
   
   e. **UX Designer** (if enabled):
      - Define interaction design patterns
      - Establish UI component library and design system
      - Plan responsive and adaptive layouts
      - Define accessibility implementation approach
      - Establish visual design guidelines
      - Plan for user feedback and micro-interactions
   
   f. **Frontend Developer (FE)** (if enabled):
      - Design frontend architecture and framework choice
      - Plan component hierarchy and composition
      - Define state management approach
      - Establish styling strategy (CSS Modules, Tailwind, etc.)
      - Plan for code splitting and lazy loading
      - Define build and bundling strategy
   
   g. **Backend Developer (BE)** (if enabled):
      - Design backend architecture and framework choice
      - Plan API endpoint structure and operations
      - Design database schema and relationships
      - Define business logic layer architecture
      - Plan for caching and performance optimization
      - Establish data validation and error handling
   
   **Orchestration Pattern**:
   - SA creates the initial architecture and technology stack
   - TL, DevOps, Security, UX, FE, and BE contribute their specialized sections in parallel
   - SA reviews and integrates all contributions for consistency
   - TL validates that implementation strategy aligns with architecture
   - Resolve conflicts with SA having final say on architecture, TL on implementation approach
   - Ensure all personas' contributions align with project constitution
   
   **Attribution**: Add subtle attribution markers for persona contributions:
   ```markdown
   <!-- Architecture designed by: SA -->
   <!-- Implementation strategy by: TL -->
   <!-- Infrastructure plan by: DevOps -->
   ```

6. **Stop and report**: Command ends after Phase 2 planning. Report branch, IMPL_PLAN path, generated artifacts, and persona contributions (if applicable).

## Phases

### Phase 0: Outline & Research

1. **Extract unknowns from Technical Context** above:
   - For each NEEDS CLARIFICATION → research task
   - For each dependency → best practices task
   - For each integration → patterns task

2. **Generate and dispatch research agents**:

   ```text
   For each unknown in Technical Context:
     Task: "Research {unknown} for {feature context}"
   For each technology choice:
     Task: "Find best practices for {tech} in {domain}"
   ```

3. **Consolidate findings** in `research.md` using format:
   - Decision: [what was chosen]
   - Rationale: [why chosen]
   - Alternatives considered: [what else evaluated]

**Output**: research.md with all NEEDS CLARIFICATION resolved

### Phase 1: Design & Contracts

**Prerequisites:** `research.md` complete

1. **Extract entities from feature spec** → `data-model.md`:
   - Entity name, fields, relationships
   - Validation rules from requirements
   - State transitions if applicable

2. **Generate API contracts** from functional requirements:
   - For each user action → endpoint
   - Use standard REST/GraphQL patterns
   - Output OpenAPI/GraphQL schema to `/contracts/`

3. **Agent context update**:
   - Run `{AGENT_SCRIPT}`
   - These scripts detect which AI agent is in use
   - Update the appropriate agent-specific context file
   - Add only new technology from current plan
   - Preserve manual additions between markers

**Output**: data-model.md, /contracts/*, quickstart.md, agent-specific file

## Key rules

- Use absolute paths
- ERROR on gate failures or unresolved clarifications
