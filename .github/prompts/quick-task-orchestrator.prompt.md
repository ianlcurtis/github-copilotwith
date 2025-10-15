````prompt
# ⚡ Quick Task Orchestrator

## Purpose
Streamlined orchestration for single-day tasks like refactoring, small features, or performance analysis.

## Overview
This guides you through a simplified 4-phase process:
1. **Objective Gathering** - Clarify what needs to be done
2. **Approach Options** - Present 2-3 ways to accomplish it
3. **Plan Creation** - Break into actionable steps
4. **Execution** - Implement with progress tracking

## Instructions for Copilot

### Phase Coordination
You orchestrate a focused, single-objective task. Keep everything concise and action-oriented.

### Workspace Structure
All task artifacts go in: `0.Delivery/[task-name]/`
- `objective.md` - Clear task definition
- `approach-options.md` - 2-3 implementation approaches
- `plan.md` - Step-by-step execution plan
- `progress.md` - Real-time progress tracking

### Getting Started
1. Ask user to describe the task objective in 1-2 sentences
2. Create workspace folder: `0.Delivery/[descriptive-task-name]/`
3. Begin Phase 1

### Phase Execution

**Phase 1: Objective Gathering** (#file:quick-task/1.task-objective.prompt.md)
- Clarify exactly what needs to be done
- Identify success criteria
- Note constraints or requirements
- Ask 2-3 clarifying questions max
- Create `objective.md`

**Phase 2: Approach Options** (#file:quick-task/2.task-approaches.prompt.md)
- Analyze the objective
- Present 2-3 viable approaches
- Include pros/cons and time estimates
- User selects preferred approach
- Create `approach-options.md` with selection noted

**Phase 3: Plan Creation** (#file:quick-task/3.task-plan.prompt.md)
- Break work into 3-7 concrete steps
- Identify files/code to modify
- Note potential risks
- Create `plan.md` with checklist
- Create `progress.md` for tracking

**Phase 4: Execution** (#file:quick-task/4.task-execute.prompt.md)
- Work through plan step-by-step
- Update `progress.md` after each step
- Mark items complete in `plan.md`
- Validate against success criteria
- Summary of what was accomplished

### Quality Gates
Before next phase:
- [ ] User confirms understanding
- [ ] Expected file created/updated
- [ ] No blocking questions remain

### Progress Tracking
Throughout execution, maintain `progress.md`:
```markdown
# Progress Log

## [Step Name] - ⏳ In Progress / ✅ Complete
- What was done
- Any issues encountered
- Time taken

[Next step...]
```

### Completion
Task is complete when:
- [ ] All plan steps executed
- [ ] Success criteria met
- [ ] Changes tested/validated
- [ ] User confirms satisfaction

## Usage
```
#file:quick-task-orchestrator "refactor the authentication module to use async/await"
```

## Expected Timeline
Entire process should complete within one working day (max 8 hours).

## Next Steps
When ready, begin:
```
#file:quick-task/1.task-objective.prompt.md
```
````
