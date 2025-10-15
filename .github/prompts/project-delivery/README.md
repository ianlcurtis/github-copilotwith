# 🎯 GitHub Copilot Project Orchestration Prompts

## Overview
This collection of GitHub Copilot prompts provides a structured, orchestrated approach to project delivery. The system guides users through a comprehensive 5-phase process from requirements gathering to implementation completion.

## Getting Started
To begin a project:
1. Invoke `/project-delivery-orchestrator` in the agent chat window
2. Follow the guided prompts through each phase
3. Review and approve at each checkpoint
4. Execute the plan step-by-step
5. Validate completion against success criteria
   
### Starting a New Project
```
/project-delivery-orchestrator "I want to create a project that will [describe your high-level goal]"
```

### Individual Phase Usage
If you need to run specific phases independently:
```
#file:project-delivery/1.project-requirements-gatherer.prompt
#file:project-delivery/2.project-specifications-collector.prompt  
#file:project-delivery/3.project-approach-planner.prompt
#file:project-delivery/4.project-standards-setter.prompt
#file:project-delivery/5.project-implementation-executor.prompt
```

## System Architecture

### Main Orchestrator
- **`project-orchestrator.prompt.md`** (`#file:project-orchestrator`) - The main coordination prompt that manages the entire process
- **Location**: `.github/prompts/project-orchestrator.prompt.md` (at root prompts folder)

### Phase-Specific Prompts
**Location**: `.github/prompts/project-delivery/` (subfolder)
1. **`1.project-requirements-gatherer.prompt.md`** - Phase 1: Collect and organize requirements → `0.Delivery/1.Requirements/`
2. **`2.project-specifications-collector.prompt.md`** - Phase 2: Gather supporting assets and documentation → `0.Delivery/2.Specifications/`
3. **`3.project-approach-planner.prompt.md`** - Phase 3: Analyze options and create implementation plan → `0.Delivery/3.Implementation/`
4. **`4.project-standards-setter.prompt.md`** - Phase 4: Define technical standards and testing approach → `0.Delivery/4.Standards/`
5. **`5.project-implementation-executor.prompt.md`** - Phase 5: Execute development in workspace root or dedicated folder (user choice)

## Process Flow

```
┌─────────────────────┐
│ @0.project-         │
│  orchestrator       │
│  (Main Coordinator) │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│ @1.requirements-    │───▶│ @2.specifications-  │───▶│ @3.approach-        │
│  gatherer           │    │  collector          │    │  planner            │
│ (Phase 1)           │    │ (Phase 2)           │    │ (Phase 3)           │
└─────────────────────┘    └─────────────────────┘    └─────────────────────┘
                                                                      │
                                                                      ▼
┌─────────────────────┐    ┌─────────────────────┐                  
│ @5.implementation-  │◀───│ @4.standards-       │                  
│  executor           │    │  setter             │                  
│ (Phase 5)           │    │ (Phase 4)           │                  
└─────────────────────┘    └─────────────────────┘                  
```

## Expected Project Structure

After running through all phases, your project will have this structure:

```
your-project/
├── .github/
│   └── instructions/
│       └── project-<projectname>-instructions.md  # Project-specific instructions with applyTo: '**'
├── 0.Delivery/
│   └── 1.Requirements/
│   ├── source-materials/           # Meeting minutes, emails, etc.
│   ├── structured-requirements/    # User stories, BDD specs
│   ├── objectives/
│   │   └── objectives.md          # High-level project goals
│   └── requirements.md            # Comprehensive requirements summary
├── 1.Specifications/
│   ├── schemas/                   # Data schemas, XSD files
│   ├── examples/
│   │   ├── inputs/               # Sample input files
│   │   ├── outputs/              # Expected outputs
│   │   └── configurations/       # Config examples
│   ├── documentation/            # Technical specs
│   ├── diagrams/                 # System diagrams
│   ├── standards/                # Industry standards
│   ├── reference/                # Reference materials
│   └── analysis.md               # Asset analysis summary
├── 2.Implementation/
│   ├── implementation-plan.md     # Detailed execution plan
│   ├── progress-checklist.md      # Progress tracking
│   ├── session-log.md            # Development session log
│   └── standards-checklist.md    # Quality standards checklist
└── [your implementation files]   # Actual project code
```

## Phase Details

### Phase 1: Requirements Gathering
**Purpose**: Collect all project requirements and establish clear objectives
**Duration**: 1-2 hours
**Deliverables**:
- Organized requirements folder structure
- Comprehensive requirements.md file
- Clear project objectives
- Created/updated project-specific instructions file

### Phase 2: Specifications Collection
**Purpose**: Gather supporting technical assets and documentation
**Duration**: 1-2 hours  
**Deliverables**:
- Well-organized specifications folder
- Asset analysis document
- File processing guidelines
- Updated project-specific instructions file

### Phase 3: Approach Planning
**Purpose**: Analyze requirements and create detailed implementation plan
**Duration**: 2-3 hours
**Deliverables**:
- Multiple implementation options analysis
- Selected approach with rationale
- Detailed implementation plan with 3-hour goals
- Progress tracking checklist

### Phase 4: Standards Setting
**Purpose**: Define technical standards and testing approaches
**Duration**: 1 hour
**Deliverables**:
- Technology-specific coding guidelines
- Testing strategy and standards
- Quality assurance requirements
- Standards compliance checklist

### Phase 5: Implementation Execution
**Purpose**: Execute the development plan with quality tracking
**Duration**: Multiple 3-hour sessions
**Deliverables**:
- Working implementation
- Comprehensive tests
- Complete documentation
- Session logs and progress tracking

## Key Features

### Quality Gates
Each phase includes validation checkpoints to ensure completeness before proceeding.

### 3-Hour Goal Structure
Implementation goals are sized to be achievable within focused 3-hour development sessions.

### Standards-Driven Development
Technical standards are established early and applied consistently throughout development.

### Progress Tracking
Comprehensive tracking of progress, blockers, and lessons learned.

### Flexibility
While designed as a sequential process, individual prompts can be used independently as needed.

## Best Practices

### For Users
- Be thorough in gathering initial materials
- Convert documents to markdown for better processing
- Include edge cases and error scenarios in requirements
- Document business context for technical specifications
- Plan for iterative refinement of requirements

### For Implementation
- Work in small, incremental steps
- Test functionality as you build
- Maintain documentation throughout development
- Track time against estimates
- Celebrate milestone completions

## Customization

The prompt system is designed to be adaptable:
- Technology-specific guidelines can be customized
- Standards can be adjusted for team experience
- Goal sizing can be modified based on complexity
- Quality requirements can be tailored to project needs

## Success Metrics

Projects using this system typically achieve:
- **95%+ Requirements Coverage** - Comprehensive requirement capture
- **Clear Implementation Path** - Well-defined technical approach  
- **Consistent Code Quality** - Standards-driven development
- **Predictable Delivery** - 3-hour goal structure enables reliable planning
- **Complete Documentation** - Documentation maintained throughout process

## Support and Troubleshooting

### Common Issues
- **Incomplete Requirements**: Return to requirements-gatherer for additional material
- **Technical Blockers**: Use approach-planner to explore alternative solutions
- **Quality Issues**: Review standards-setter output and apply quality gates
- **Scope Creep**: Reference original requirements.md and objectives

### Getting Help
- Each prompt includes detailed instructions and troubleshooting tips
- Use the session-log.md to track and communicate issues
- Progress-checklist.md helps identify where problems occur
- Standards-checklist.md ensures quality standards are maintained

This orchestrated approach transforms ad-hoc development into a structured, predictable process that consistently delivers high-quality results.