# Role Personas

This directory contains role persona definitions that guide AI agents in contributing specialized expertise to the Spec-Driven Development workflow.

## What are Role Personas?

Role personas are specialized AI agent profiles that bring domain-specific expertise to different phases of the development process. Each persona focuses on specific aspects of software development, ensuring comprehensive coverage of requirements, architecture, implementation, testing, and operations.

## Available Personas

### Core Personas (Recommended for All Projects)

| Persona | Short Name | Primary Focus | Key Contributions |
|---------|------------|---------------|-------------------|
| **Business Analyst** | BA | Requirements & User Stories | User scenarios, functional requirements, acceptance criteria |
| **Solution Architect** | SA | Technical Architecture | System design, technology stack, integration patterns |
| **Tech Lead** | TL | Implementation Strategy | Code organization, task breakdown, best practices |

### Specialized Personas (Add as Needed)

| Persona | Short Name | Primary Focus | Key Contributions |
|---------|------------|---------------|-------------------|
| **Quality Assurance** | QA | Testing & Quality | Test strategies, quality gates, validation |
| **DevOps Engineer** | DevOps | Infrastructure & Deployment | CI/CD, monitoring, operational excellence |
| **Security Engineer** | Security | Security & Compliance | Threat modeling, security controls, compliance |
| **UX Designer** | UX | User Experience | Usability, accessibility, interaction design |
| **Frontend Developer** | FE | UI Implementation | Component design, state management, responsive design |
| **Backend Developer** | BE | API & Data | API design, database design, business logic |

## How Personas Work

### 1. Selection During Project Initialization

When running `specify init`, you'll be prompted to select which personas to enable for your project:

```bash
specify init my-project
```

You can select multiple personas using arrow keys and space bar. The selected personas will be:
- Copied to your project's `memory/personas/` directory
- Stored in `.specify/config.json` for reference by commands
- Activated for all subsequent spec commands

### 2. Persona Contributions by Command

Different personas contribute at different stages:

#### `/speckit.specify` (Specification Creation)
- **BA**: Creates user stories, functional requirements, acceptance criteria
- **SA**: Validates technical feasibility, identifies system constraints
- **Security**: Identifies security and compliance requirements
- **UX**: Defines user experience and accessibility requirements
- **QA**: Creates test scenarios and quality criteria

#### `/speckit.plan` (Implementation Planning)
- **SA**: Designs architecture, selects technology stack, creates data models
- **TL**: Defines implementation strategy, code organization, development workflow
- **DevOps**: Plans infrastructure, deployment strategy, monitoring
- **Security**: Designs security architecture, authentication/authorization
- **UX**: Defines interaction design, UI patterns, responsive design
- **FE**: Plans frontend architecture, component design, state management
- **BE**: Plans backend architecture, API design, database design

#### `/speckit.tasks` (Task Breakdown)
- **TL**: Breaks down implementation into tasks, manages dependencies
- **QA**: Creates testing tasks and validation checkpoints
- **FE**: Defines UI implementation tasks
- **BE**: Defines API and database tasks
- **DevOps**: Creates infrastructure and CI/CD tasks

#### `/speckit.implement` (Implementation)
- **TL**: Reviews code quality and best practices
- **QA**: Validates quality gates and runs tests
- **DevOps**: Validates deployment and monitoring
- **Security**: Performs security validation and vulnerability assessment
- **UX**: Validates user experience and accessibility
- **FE**: Implements and validates UI
- **BE**: Implements and validates backend logic

### 3. Sub-Agent Orchestration

When personas are enabled, the main AI agent acts as an orchestrator (similar to Cursor's Composer):

1. **Parallel Execution**: Independent personas work simultaneously
2. **Context Sharing**: Personas have access to relevant artifacts
3. **Contribution Merging**: Orchestrator integrates persona outputs
4. **Conflict Resolution**: Orchestrator resolves conflicting recommendations

## Persona Definition Format

Each persona is defined in a Markdown file with YAML frontmatter:

```markdown
---
id: persona-id
name: Full Persona Name
short_name: Abbreviation
role: Brief role description
contributes_to:
  - specify
  - plan
  - tasks
  - implement
phases:
  specify: ["section1", "section2"]
  plan: ["section1", "section2"]
---

# Persona Name

## Role Description
[Detailed description of the persona's role and responsibilities]

## Core Responsibilities
[What the persona does in each phase]

## Contribution Guidelines
[What to focus on and what to avoid]

## Quality Checklist
[Checklist items the persona should verify]

[Additional sections with domain-specific guidance]
```

## Configuration

Persona settings are stored in `.specify/config.json`:

```json
{
  "version": "1.0",
  "personas": {
    "enabled": ["business-analyst", "solution-architect", "tech-lead"],
    "available": [
      "business-analyst",
      "solution-architect",
      "tech-lead",
      "quality-assurance",
      "devops",
      "security",
      "ux",
      "frontend-developer",
      "backend-developer"
    ]
  },
  "orchestration": {
    "parallel_execution": true,
    "max_concurrent_personas": 3
  }
}
```

## Customizing Personas

You can customize personas for your project:

### 1. Modify Existing Personas

Edit persona files in `memory/personas/` to add project-specific guidelines:

```markdown
## Project-Specific Guidelines

### Our Technology Stack
- Frontend: React with TypeScript
- Backend: Python FastAPI
- Database: PostgreSQL

### Our Standards
- All APIs must use OpenAPI 3.0 specification
- All code must have 80%+ test coverage
- All UIs must meet WCAG 2.1 AA standards
```

### 2. Create Custom Personas

Create new persona files following the standard format:

```bash
# Create a new persona
touch memory/personas/data-scientist.md
```

Then add it to your `.specify/config.json`:

```json
{
  "personas": {
    "enabled": ["business-analyst", "solution-architect", "tech-lead", "data-scientist"]
  }
}
```

### 3. Disable Personas

To disable personas temporarily, update `.specify/config.json`:

```json
{
  "personas": {
    "enabled": []
  }
}
```

Or delete the personas from `memory/personas/` directory.

## Best Practices

### Choosing Personas

**For Small Projects** (1-2 developers):
- Start with core trio: BA, SA, TL
- Add QA for quality focus

**For Medium Projects** (3-5 developers):
- Core trio: BA, SA, TL
- Add: QA, DevOps, Security
- Add FE/BE if specialized frontend/backend

**For Large Projects** (6+ developers):
- All personas as needed
- Customize personas for your domain
- Consider creating custom personas

### Persona Collaboration

Personas work best when they collaborate:

1. **BA ↔ SA**: Ensure requirements are technically feasible
2. **SA ↔ TL**: Align architecture with implementation
3. **TL ↔ QA**: Coordinate on testing strategy
4. **DevOps ↔ Security**: Integrate security into infrastructure
5. **UX ↔ FE**: Ensure design is implementable
6. **SA ↔ BE**: Align backend architecture with system design

### Avoiding Persona Overload

Too many personas can slow down the process:

- Start with core personas
- Add specialized personas only when needed
- Disable personas that aren't contributing
- Merge similar personas for small projects

## Troubleshooting

### Personas Not Contributing

If personas aren't contributing:

1. Check `.specify/config.json` - are they enabled?
2. Verify persona files exist in `memory/personas/`
3. Check command templates - do they reference personas?
4. Review AI agent capabilities - does it support sub-agents?

### Conflicting Recommendations

If personas provide conflicting advice:

1. The orchestrator should resolve conflicts
2. Prioritize based on phase (BA in specify, SA in plan, TL in implement)
3. Document trade-offs and decisions
4. Update persona guidelines to prevent future conflicts

### Performance Issues

If persona orchestration is slow:

1. Reduce `max_concurrent_personas` in config
2. Disable parallel execution temporarily
3. Use fewer personas for simple features
4. Consider sequential execution for complex features

## Examples

### Example 1: E-commerce Feature

**Selected Personas**: BA, SA, TL, QA, Security, FE, BE

**Workflow**:
1. BA defines user stories for checkout flow
2. Security identifies PCI-DSS requirements
3. SA designs payment processing architecture
4. FE plans shopping cart UI components
5. BE designs payment API and order management
6. TL breaks down into implementation tasks
7. QA creates test scenarios for payment flows

### Example 2: Internal Dashboard

**Selected Personas**: BA, SA, TL, UX, FE

**Workflow**:
1. BA defines reporting requirements
2. UX designs dashboard layout and interactions
3. SA plans data aggregation architecture
4. FE designs chart components and state management
5. TL creates implementation tasks
6. All personas validate final implementation

### Example 3: API Service

**Selected Personas**: BA, SA, TL, QA, Security, DevOps, BE

**Workflow**:
1. BA defines API requirements
2. Security plans authentication and rate limiting
3. SA designs API architecture and contracts
4. BE plans endpoint implementation
5. DevOps plans deployment and monitoring
6. TL breaks down into tasks
7. QA creates API test scenarios

## Further Reading

- [Spec-Driven Development Methodology](../../spec-driven.md)
- [AGENTS.md](../../AGENTS.md) - Agent integration guide
- [README.md](../../README.md) - Main documentation

## Support

For questions or issues with personas:

1. Check this README first
2. Review persona definition files
3. Consult the main documentation
4. Open an issue on GitHub

