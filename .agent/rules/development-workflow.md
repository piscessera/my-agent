---
trigger: always_on
glob: "**/*"
description: Standard development workflow steps from requirement to documentation.
---

# Development Workflow

Follow this structured workflow for every development task to ensure quality and consistency.

## Shortcode Usage
- **Individual Steps**: You can execute any single step by using its shortcode (e.g., /dev-req).
- **Chained Steps**: You can execute multiple steps in sequence by listing shortcodes separated by spaces (e.g., /dev-req /dev-plan todolist with no database).
- **Shortcodes**:
  1. /dev-req (Steps 1-4: Requirement -> Analyze -> Task -> DoD)
  2. /dev-plan (Step 5: Plan)
  3. /dev-go  (Steps 6-9: Develop -> Test -> Doc -> Log)

## 1. Requirement
- **Goal**: Clearly understand what needs to be built or solved.
- **Actions**:
  - Read and analyze the user request.
  - Ask clarifying questions if requirements are ambiguous.
  - Identify the core problem and the desired outcome.
  - **Git Branch**: Create a new branch following the format: eature/[running_number]-[req-summarize-meanful-naming].

## 2. Analyze & Research & Design
- **Goal**: Plan the solution before writing code.
- **Actions**:
  - **Research**: Investigate necessary libraries, documentation, or existing patterns.
  - **Analyze**: Consider potential edge cases and performance implications.
  - **Design**: 
    - Architecture: detailed component hierarchy, data flow, or API structure.
    - UI/UX: Design mocks or layout plans (if applicable).

## 3. Breakdown Tasks
- **Goal**: Split the work into manageable, logical units.
- **Actions**:
  - Create a list of small, focused sub-tasks.
  - Ensure each task is independent where possible.
  - Estimate the complexity of each task.

## 4. Acceptance Criteria
- **Goal**: Define what "Done" means.
- **Actions**:
  - List specific conditions that must be met for the task to be considered complete.
  - Include functional requirements (it works) and non-functional requirements (performance, style).

## 5. Implement Plan
- **Goal**: Create a roadmap for execution.
- **Actions**:
  - Outline the step-by-step implementation order.
  - Identify dependencies between steps.
  - Review the plan against safety rules (e.g., is it reversible? is it safe?).

## 6. Develop
- **Goal**: Write high-quality, maintainable code.
- **Actions**:
  - Write code following the **Development Principles** (Code Quality, Git Workflow, Formatting).
  - Implement one sub-task at a time.
  - **Continuous Analysis**: Run static analysis/linting tools frequently throughout the coding phase.
  - Commit often with meaningful messages.

## 7. Testing
- **Goal**: Ensure the solution works as expected.
- **Actions**:
  - **Unit Testing**: Test individual functions/components.
  - **Integration Testing**: Test how components work together.
  - **Manual Verification**: Verify the critical path (happy flow) manually.
  - **Runtime Verification**: Explicitly launch/run the application to verify it starts and functions correctly (not just builds).
  - **Validation**: Check against the **Acceptance Criteria**.

## 8. Documentation
- **Goal**: Make the code easy to understand and use.
- **Actions**:
  - Add inline comments for complex logic.
  - Update README.md or other documentation files if features change.
  - Document any new environment variables or setup steps.

## 9. Working Log
- **Goal**: Track progress and decisions.
- **Actions**:
  - Update the working log file continuously.
  - Use the template defined in **Development Principles**.
  - Record major decisions, blockers, and their resolutions.
  - **Visual Evidence**: For UI-related changes, attach a screenshot or video of the running feature.
  - **Pull Request**: Create a Pull Request to main upon completion.
