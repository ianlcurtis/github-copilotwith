# Quick Tasks - Streamlined Single-Day Task Execution

## Overview
A simplified prompt system for focused, single-day tasks like refactoring, small features, performance analysis, or bug fixes. Based on the comprehensive project delivery system but streamlined for speed and simplicity.

## Getting Started

To begin a quick task:
1. Invoke `/quick-task-orchestrator.prompt` in the agent chat window
2. Follow the guided prompts through each phase
3. Review and approve at each checkpoint
4. Execute the plan step-by-step
5. Validate completion against success criteria

### Usage Example
```
/quick-task-orchestrator "refactor authentication module to use async/await"
```

The orchestrator will guide you through:
1. Clarifying the objective and success criteria
2. Presenting implementation approaches
3. Creating a detailed execution plan
4. Executing the plan with progress tracking

## Purpose
Execute well-defined, scoped tasks that can be completed within a single working day (max 8 hours), with structured planning and progress tracking.

## Task Types
- Code refactoring
- Small feature additions
- Performance optimization
- Bug fixes with clear scope
- Code quality improvements
- Documentation updates
- Test coverage improvements

## Process Flow

### 1. Task Orchestrator (`#file:quick-task-orchestrator` - at root)
Main entry point that coordinates the 4-phase process.
**Location**: `.github/prompts/quick-task-orchestrator.prompt.md`

### 2. Objective Gathering (`#file:1.task-objective`)
- Clarify what needs to be done
- Define success criteria (2-4 items)
- Identify constraints and scope
- List involved files/components
- Output: `objective.md`

### 3. Approach Options (`#file:2.task-approaches`)
- Present 2-3 viable approaches
- Show pros/cons and time estimates
- User selects preferred approach
- Output: `approach-options.md`

### 4. Plan Creation (`#file:3.task-plan`)
- Break into 3-7 concrete steps
- Define validation for each step
- Identify risks and mitigation
- Output: `plan.md` and `progress.md`

### 5. Execution (`#file:4.task-execute`)
- Execute steps sequentially
- Track progress in real-time
- Validate after each step
- Handle issues as they arise
- Output: Updates to `progress.md`

## Workspace Structure
All task artifacts are stored in a single folder:

```
0.Delivery/
└── [task-name]/
    ├── objective.md          # Task definition and success criteria
    ├── approach-options.md   # Evaluated approaches and selection
    ├── plan.md              # Detailed execution plan with steps
    └── progress.md          # Real-time progress tracking
```

## Key Principles

### Simplicity
- Single folder structure
- Self-descriptive filenames
- No unnecessary complexity
- Focus on action over documentation

### Speed
- Minimal planning overhead
- Quick decision-making
- 2-3 approach options max
- 3-7 execution steps

### Quality
- Clear success criteria
- Step-by-step validation
- Progress tracking
- Issue documentation

### Flexibility
- Adapt plan as needed
- Handle unexpected issues
- Document deviations
- Maintain rollback options

## Completion Criteria
A task is complete when:
- All execution steps are done
- Success criteria are met
- Tests are passing
- Changes are validated
- User confirms satisfaction

## Time Expectations
- **Objective Gathering**: 15-30 minutes
- **Approach Options**: 15-30 minutes
- **Plan Creation**: 30-60 minutes
- **Execution**: 3-6 hours
- **Total**: Maximum 8 hours (one working day)

## Comparison to Full Project Delivery

| Aspect | Full Project | Quick Task |
|--------|-------------|------------|
| Duration | Weeks/months | Hours/1 day |
| Folder Structure | Multiple organized folders | Single folder |
| Planning Depth | Comprehensive | Focused |
| Documentation | Extensive | Essential only |
| Approach Options | 3-5 detailed | 2-3 concise |
| Plan Steps | 10-20+ | 3-7 |
| Progress Tracking | Multiple logs | Single progress file |

## When to Use

### Use Quick Tasks For:
- Refactoring a module or component
- Adding a small, well-defined feature
- Performance optimization of specific code
- Bug fixes with clear reproduction
- Code quality improvements
- Test coverage additions

### Use Full Project Delivery For:
- New application development
- Major feature additions
- System architecture changes
- Multiple integrated components
- Long-term initiatives
- Complex requirements gathering needed

## Best Practices

1. **Start Clear**: Spend time in Phase 1 getting crystal clear on the objective
2. **Choose Wisely**: Select the approach that balances speed and quality for your context
3. **Plan Granular**: Break work into steps small enough to validate incrementally
4. **Track Diligently**: Update progress after each step - it helps maintain momentum
5. **Validate Early**: Test as you go rather than waiting until the end
6. **Document Issues**: Capture problems and solutions for future reference
7. **Stay Focused**: Resist scope creep - if new requirements emerge, start a new task

The orchestrator will ensure you move through phases methodically and don't skip important steps.
