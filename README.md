# GitHub Copilot Use Case Solutions
A library of use case archetypes and GHCP solutions.

# Greenfield
TBD

# Brownfield

## Process
The steps where GHCP can help.
- Document
- Test as-is
- Migrate
- Review
- Test migrated

## Set overall project aim
```
You are an expert production code developer specializing in Java development and legacy system modernization. Your code is concise, readable, production-ready, and thoroughly commented for clarity.

**Core Principles:**
- Write production-quality Java code that follows enterprise standards
- Provide clear, meaningful comments explaining business logic and complex operations
- Apply separation of concerns and maintain clean code organization
- Use appropriate design patterns and SOLID principles
- Follow Java naming conventions and coding standards

**Legacy Migration Focus:**
- Modernize BASH scripts and COBOL systems to Java equivalently
- Preserve business logic while improving maintainability
- Document migration decisions and assumptions
- Ensure backward compatibility where required

**Code Organization:**
- Structure packages logically by feature/domain
- Separate business logic from infrastructure concerns
- Use proper layering (controller/service/repository patterns)
- Include comprehensive error handling and logging
- Write testable, modular code with clear interfaces

Always prioritize code clarity, maintainability, and production readiness over brevity.

When appropriate, you should use the #microsoft.docs.mcp tool to access the latest documentation and examples directly from the Microsoft Docs Model Context Protocol (MCP) server.
```

## Documentation of current code

### Prompt to give a high level description of what a project does:
```
We are new starters and have never touched this code before.
[Can you create a ReadMe file explaining] / [Can you explain] what this project does?
Start with a high-level summary of the codebase, then break the project down into logical areas and provide a thorough explanation of each, and how they interact.
The explanation should be written in a way that it can be understood by a person joining the team that is unfamiliar with the code.
```

### Prompt to give a description of a specific file e.g. an API controller:
```
Explain the [file name], what is the input, its output and what is it dependent upon. Note that I do not know this codebase, do not make any assumptions about my knowledge.
```

### Prompt to document a file e.g. route xml in Java:
```
Generate a mermaid diagram for this API project for our documentation? Additionally include a synopsis of each route formatted like this:
 
api/route:
Purpose:
Special parameters:
Error handling:
Headers:
```

## Testing

### Prompt to suggest testing approach:
```
I want to test the project before and after a code conversion. What would be the best way to do this?
```

### Prompts to assess existing unit test coverage:
Github Copilot can create additional high value unit test coverage, which can drive code quality improvement - for example, by discovering unhandled edge cases. 
```
Review the existing unit test coverage of the MODULE_NAME module consider existing tests with a view to adding additional tests in the future.
Generate a new readme markdown file named after the module name that provides an overview of the module and the test scenarios that currently exist.
```
```
Are there any additional tests I should include, or are all test angles covered?
Consider that I am using the [test framework e.g. Mockito] test framework.
Pay attention to edge cases and happy / unhappy paths. Provide a test coverage matrix.
```
```
The company standard framework for writing unit tests is the JUnit test framework.
Review the existing unit test coverage of the #folder:MODULE_NAME.
Create a new readme markdown file, in the same folder, named after the module name, that provides an overview of the module and the test scenarios that currently exist. 
If you do not find any unit tests, state in the readme file that there are no existing tests. 
**Ensure a comprehensive overview of all tests within this module**
```

### Prompt to create a plan to implement missing unit test coverage:
```
Consider the #MODULE-README.md and add a section at the end that includes a plan to enhance unit test coverage. 
You must include unit tests to enhance resilience, happy and unhappy paths and any edge cases. The plan to enhance unit test coverage must be ordered so we can work through it together.
Please do not include integration, performance or stress tests at this point. We will consider these at a later date.
```

### Prompt to implement the plan created in the previous prompt: 
(The generated plan should be selected with the cursor)
```
Using this #selection, detail a plan to enhance unit test coverage. I want to focus on enhancing the resilience and maintainability of this module. 
When implementing the tests you must comment your tests cases thoroughly so that what is being tested is clearly understandable. If there is an existing test for what is being tested, please amend the existing test file, where the tests are new, create a new file and ensure it is named appropriately.
Note all of the tests are in this #folder. Do not add tests to a new folder that is outside of this folder.
```

Alternatively, prompts to write test cases:
```
Can you help me write the test cases, first for the existing code.
The tests should compliment those that exist already, and create coverage for happy and unhappy path plus edge cases.
Use the [test framework e.g. Mockito] test framework.
You should comment your test cases thoroughly so that the objective is clearly understandable.
```
Variation on prompt to write test cases, can be run after the previous prompt which identifies missing angles. 
You should include existing test case code as context:
```
Can you write the test cases based on your advice.
Use the [test framework e.g. Mockito] test framework, and try to match our existing test cases.
You should comment your test cases thoroughly so that the objective is clearly understandable.
```

### Prompt to mock dependencies:
```
Can you help me mock dependencies of this service.
Ensure that any dependencies that are not within the workspace are mocked so that the code can be run in isolation.
```

### Prompt to create test data (small example test data required):
```
Can you create me 100 test data samples. The format should be as follows:

USERID,NAME,FAVOURITE_COLOUR
001,Jo,Orange
002,Anth,Black
003,John,Pink
004,Ian,Green
```

### Prompt to create performance tests:
```
?
```


## Migration
Migration from one code base to another.

Prompt example to start examining the process to convert from one codebase to another:
```
You are an expert in [technology e.g. Springboot and Camel], please explain step by step how you would redesign and convert this from [ Source implementation e.g. Camel] to [target implementation e.g. Springboot native]?
```

## Type
The type of solution being migrated.
- API
