---
description: Break down an implementation plan into test-first (TDD) tasks.
argument-hint: <path/to/implementation-plan.md>
---
Generate a task list that follows a **Test-Driven Development (TDD)** workflow, based on an implementation plan document. Each task should guide the developer to write tests before implementation.

**Prerequisite Check:**
1. The user must provide a path to an implementation plan document in `$ARGUMENTS`.
2. If `$ARGUMENTS` is empty or the file doesn't exist, stop and instruct the user to provide a valid plan file, suggesting they run `/create-implement-plan` if one doesn't exist.

**Task Generation Process:**
1. Read the implementation plan provided in `$ARGUMENTS`.
2. Focus on the "Detailed Design," "Migration & Rollout," and "Milestones" sections.
3. Decompose the work into a sequence of tasks structured for a **Red-Green-Refactor** workflow.

**Task Definition (Test-First):**
Each task must be structured into three parts: writing tests, implementing the feature, and refactoring.
- **Testable (Test-First):** The first action must be to write a failing test that defines the goal of the task.
- **Independent:** Can be worked on without being blocked by other tasks *in the same phase*.
- **Small:** Represents a single, focused piece of work (ideally less than 1 day).

**Output Format:**
- Create a new file for the tasks.
- **Path**: `tasks/YYYY-MM-DD-tasks-from-<plan-slug>.md`.
- **Slug**: Derive `<plan-slug>` from the implementation plan's filename.
- If the `tasks/` directory does not exist, ask the user for permission to create it.
- Use Markdown headings for each task, with numbered steps for the TDD workflow.

Example Output Structure:
```markdown
# TDD Tasks for [Feature Name from Plan]

Based on: [path/to/implementation-plan.md]

## Milestone 1: Backend API

### Task 1.1: Add `email` and `password_hash` to `users` table

- **Status**: To Do

**1. Define Test Case (Manual for Migrations):**
- [ ] Define the verification steps: after running the migration, the `users` table should have `email` and `password_hash` columns. After rollback, they should be gone.

**2. Implement & Verify:**
- [ ] Create a new database migration file.
- [ ] Add SQL `ALTER TABLE` statements for the new columns.
- [ ] Run the migration and verify its success.
- [ ] Run the migration rollback and verify the table is restored.

### Task 1.2: Create `POST /api/v1/register` endpoint

- **Status**: To Do

**1. Write Failing Tests (Test-First):**
- [ ] Write an integration test for a `POST` request to `/api/v1/register` with a valid payload, asserting a `201 Created` response. Confirm it fails with `404 Not Found`.
- [ ] Add a test case for an invalid email, asserting a `400 Bad Request` response.
- [ ] Add a test case for a missing password, asserting a `400 Bad Request` response.

**2. Implement to Pass Tests:**
- [ ] Add the route `POST /api/v1/register`. Re-run tests.
- [ ] Create the controller and request DTOs with validation. Re-run tests.
- [ ] Implement the minimal service logic to make the happy-path test pass.
- [ ] Add the specific validation logic to make the remaining tests pass.

**3. Refactor:**
- [ ] Review the controller and service code for clarity and adherence to standards.
- [ ] Ensure all tests still pass after refactoring.

...
```

**Final Steps:**
After writing the file:
1. Print the absolute path to the generated task list.
2. Suggest the user can now start working on the first task.
