---
description: Create a technical implementation plan for a new feature.
argument-hint: <path/to/requirements.md or "feature description">
---
Generate a **review-ready, executable** implementation plan based on the user's request.

The user has provided the context in `$ARGUMENTS`.
- If it's a file path, read the file for requirements.
- If it's a string, use it as the feature description.

Before generating the plan, scan the project for existing architectural patterns, naming conventions, and dependencies to ensure the plan is consistent with the current codebase. If the requirements are ambiguous, ask for clarification on key decisions.

The plan must be written in the same language as the source requirements (e.g., Chinese requirements -> Chinese plan). Do not translate technical terms.

## Output File

- **Path**: `docs/YYYY-MM-DD-implementation-plan-<slug>.md`
- **Slug**: Derived from the feature title, lowercased with hyphens.
- If `docs/` does not exist, ask the user for the desired output directory.

## Document Structure

### 1. Background & Goals
- **Problem**: What business problem or user need does this solve?
- **Source**: Link to the requirement document (if any).
- **Success Criteria**: What observable outcomes define success?

### 2. Scope
- **In Scope**: Bulleted list of capabilities to be delivered.
- **Out of Scope**: Bulleted list of what is explicitly excluded.
- **Assumptions**: Key technical or business assumptions.

### 3. Design Principles
- **Architectural Consistency**: How does this fit into the existing architecture (e.g., layers, services, data flow)?
- **Key Trade-offs**: What were the major design decisions and their justifications (e.g., sync vs. async, build vs. buy)?
- **Conventions**: Note adherence to existing naming, module placement, and coding styles.

### 4. Architecture Diagram
Use a **Mermaid** diagram to illustrate the components and data flow.
- Highlight new or modified components.
- Include upstream callers and downstream dependencies.
- Below the diagram, briefly describe each component's responsibility.

**For backend services:** show controllers, services, repositories, database, caches, and message queues.
**For frontend applications:** show pages/routes, components, state management, and API clients.

### 5. Detailed Design
Choose the subsection relevant to your project type.

#### 5A. Backend
- **API Contract**: HTTP method, path, request/response schemas, and error codes.
- **Data Model**: Database schema changes, new tables, or indexes.
- **Key Logic**: Describe critical classes, functions, or algorithms in pseudocode.
- **Resilience**: Note transaction boundaries, idempotency, retries, and timeouts.

#### 5B. Frontend
- **Component Hierarchy**: Describe the page/component tree and data flow.
- **State Management**: Detail the use of local state, global stores, and server-state caching.
- **Data Fetching**: Explain the strategy (e.g., SSR, CSR, hooks) and caching mechanisms.
- **User Experience**: Address loading, empty, and error states.

### 6. Migration & Rollout
- **Database**: Migration script order and rollback strategy.
- **API**: Versioning plan and backward compatibility.
- **Deployment**: Rollout strategy (e.g., feature flags, canary release).

### 7. Risks & Mitigations
| Risk | Impact (High/Med/Low) | Likelihood (High/Med/Low) | Mitigation Strategy |
| --- | --- | --- | --- |
| ... | | | |

### 8. Milestones
High-level phases only. Detailed task breakdown should be done in a separate step.

## Final Steps
After writing the file:
1. Print the absolute path to the generated document.
2. Provide a 3-5 sentence summary of the plan's key architectural decisions.
3. Suggest the user run `/create-task` to break the plan into actionable tasks.
