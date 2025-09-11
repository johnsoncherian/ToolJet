# Architecture Decision Records (ADRs)

This directory contains Architecture Decision Records for the ToolJet project. ADRs document significant architectural decisions, their context, and consequences.

## What are ADRs?

Architecture Decision Records are documents that capture important architectural decisions made in the project. They provide:

- **Context**: Why the decision was needed
- **Decision**: What was decided
- **Consequences**: The positive and negative outcomes

## ADR Index

| ADR | Title | Status | Date |
|-----|-------|--------|------|
| [0001](0001-record-architecture-decisions.md) | Record Architecture Decisions | Accepted | 2024-01-XX |

## ADR Status Definitions

- **Proposed**: Under discussion, not yet decided
- **Accepted**: Decision has been made and approved
- **Rejected**: Decision was considered but not adopted
- **Superseded**: Decision has been replaced by a newer ADR

## Creating a New ADR

1. Create a new file with the next sequential number: `XXXX-brief-title.md`
2. Use the ADR template structure
3. Set initial status to "Proposed"
4. Add the ADR to this index
5. Submit for team review

## ADR Template

```markdown
# ADR-XXXX: Brief Descriptive Title

## Status

**Proposed** | **Accepted** | **Rejected** | **Superseded**

## Context

Describe the situation and problem that led to this decision.

## Decision

Describe the architectural decision made.

## Consequences

### Positive
- List positive outcomes

### Negative  
- List negative outcomes

### Risks
- List potential risks
```

## Guidelines

- Keep ADRs concise but complete
- Focus on the "why" not just the "what"
- Update status as decisions evolve
- Reference related ADRs when applicable
- Include relevant technical details and constraints
