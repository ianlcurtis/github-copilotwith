<p align="left">
	<img src="img/githubcopilot-colour.png" alt="GitHub Copilot Logo" width="120" height="100"/>
</p>

# GitHub Copilot Use Case Solutions

A library of use case archetypes, GHCP solutions, and GHCP advice and best practice.

## Index
- [Initial project setup](#initial-project-setup)
	- [Copilot-instructions.md](#copilot-instructionsmd)
	- [VSCode Modes for task specific context](#vscode-modes-for-task-specific-context)
	- [MCP Servers for external information sources](#mcp-servers-for-external-information-sources)
- [Greenfield](#greenfield)
- [Brownfield](#brownfield)
	- [Process](#process)
	- [Documentation](#documentation)
	- [Testing](#testing)
		- [System Testing](#system-testing)
		- [Unit testing](#unit-testing)
		- [Mocking](#mocking)
		- [Test data](#test-data)
		- [Performance testing](#performance-testing)
		- [QA](#qa)
	- [Migration](#migration)
		- [Migrate by example](#migrate-by-example)
		- [Migrate by flow](#migrate-by-flow)
		- [Migrate by steps](#migrate-by-steps)
		- [Migrate by pseudo-code](#migrate-by-pseudo-code)
		- [Migrate UI code](#migrate-ui-code)
	- [Review and Remediation](#review-and-remediation)

## Initial project setup
### Copilot-instructions.md
Use the ./.github/copilot-instructions.md prompt to specify the aim of the project, coding standards, and framework versions to use.
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

**Frameworks:**
- Use the Spring Framework 6
- Use JUnit 5

Always prioritize code clarity, maintainability, and production readiness over brevity.

When appropriate, you should use the #microsoft.docs.mcp tool to access the latest documentation and examples directly from the Microsoft Docs Model Context Protocol (MCP) server.
```

### VSCode Modes for task specific context
VSCode Modes allow you to create custom task specific context prompts (like the Agent, Edit, and Ask modes available out-of-the-box). These are easily switchable when you are performing different activities in the context of your project. Use these to layer additional prompt context. For example, you might use Agent mode to perform a COBOL to Java code migration, then switch to a custom "Reviewer", or "Optimisation Expert", mode that allows you to review the migration.
```
You are an expert Java code reviewer. Analyze the provided Java code and provide feedback on:

Code Quality: Syntax, readability, naming conventions, and best practices
Performance: Identify potential bottlenecks and optimization opportunities
Security: Look for vulnerabilities, input validation issues, and security anti-patterns
Design Patterns: Assess proper use of OOP principles and design patterns
Testing: Evaluate test coverage and quality of unit tests
Documentation: Check for adequate comments and JavaDoc

Output Format:
List critical issues first, then suggestions for improvement
Provide specific line references when possible
Suggest concrete improvements with code examples
Rate overall code quality (1-10) with brief justification

Focus Areas:
Exception handling
Resource management (try-with-resources)
Thread safety (if applicable)
Memory efficiency
Adherence to Java coding standards

Be constructive and educational in your feedback.
```

### MCP Servers for external information sources
Add appropriate MCP servers to VSCode, for example *microsoft.docs.mcp* to allow Copilot to pull in Microsoft documentation when responding.

Also see [VSCode Config guide](https://code.visualstudio.com/docs/copilot/customization/overview)

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

## Documentation

Prompt to give a high level description of what a project does:
```
We are new starters and have never touched this code before.
[Can you create a ReadMe file explaining] / [Can you explain] what this project does?
Start with a high-level summary of the codebase, then break the project down into logical areas and provide a thorough explanation of each, and how they interact.
The explanation should be written in a way that it can be understood by a person joining the team that is unfamiliar with the code.
```

Prompt to give a description of a specific file e.g. an API controller:
```
Explain the [file name], what is the input, its output and what is it dependent upon. Note that I do not know this codebase, do not make any assumptions about my knowledge.
```

Prompt to document a file e.g. route xml in Java:
```
Generate a mermaid diagram for this API project for our documentation? Additionally include a synopsis of each route formatted like this:
 
api/route:
Purpose:
Special parameters:
Error handling:
Headers:
```

## Testing

Prompt to suggest testing approach:
```
I want to test the project before and after a code conversion. What would be the best way to do this?
```

### System Testing
Prompt to suggest system testing approach:
```
Consider the legacy code <LEGACY FILE OR FOLDER> and the migrated code <MIGRATED FILE OR FOLDER>
I want to thoroughly system/black-box test the code before and after a code conversion using the same data set containing happy, unhappy, and edge cases. 
What would be the best way to do this?
```

Prompt to implement a proposed system testing approach:
```
I am happy with the suggested test approach. Create the necessary files in line with your suggestion in the folder <FOLDER NAME>.
The folder should contain the source code and a subfolder for the test data files.
```

Prompt to review system test coverage:
```
Consider the legacy code <LEGACY FILE OR FOLDER> and the migrated code <MIGRATED FILE OR FOLDER> and the test framework <TEST FRAMEWORK FOLDER>
Review the test data being used by the test framework for completeness, providing a test coverage matrix which identifies any code paths that lack adequate testing so that we can create test data to cover these paths.
```

### Unit testing
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

Prompt to create a plan to implement missing unit test coverage:
```
Consider the #MODULE-README.md and add a section at the end that includes a plan to enhance unit test coverage. 
You must include unit tests to enhance resilience, happy and unhappy paths and any edge cases. The plan to enhance unit test coverage must be ordered so we can work through it together.
Please do not include integration, performance or stress tests at this point. We will consider these at a later date.
```

Prompt to implement the plan created in the previous prompt: 
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

Variation on prompt to write test cases, can be run after the previous prompt which identifies missing angles:
You should include existing test case code as context:
```
Can you write the test cases based on your advice.
Use the [test framework e.g. Mockito] test framework, and try to match our existing test cases.
You should comment your test cases thoroughly so that the objective is clearly understandable.
```

### Mocking
Prompt to mock dependencies:
```
Can you help me mock dependencies of this service.
Ensure that any dependencies that are not within the workspace are mocked so that the code can be run in isolation.
```

### Test data
Prompt to create test data (small example test data required):
```
Can you create me 100 test data samples. The format should be as follows:

USERID,NAME,FAVOURITE_COLOUR
001,Jo,Orange
002,Anth,Black
003,John,Pink
004,Ian,Green
```

### Performance testing
```
?
```

### QA
QA teams can use copilot to create test cases, using an MCP server for either GitHub or ADO. Copilot can pull details of the user story including acceptance criteria, and create and link a test case with test steps.
See [GitHub MCP Server](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/use-the-github-mcp-server) and [ADO MCP Server](https://devblogs.microsoft.com/devops/azure-devops-mcp-server-public-preview/).
```
You are processing user story <USER STORY NUMBER>
 
Consider the user story description, and the acceptance criteria, and also the document <DOCUMENT PATH> which describes the logic of the new feature.
 
You  should do the following: 
1. Create a test case for testing this new feature
2. Link the test case to the user story
3. Add test steps, each with an action and a result, that thoroughly test all aspects of the new functionality.
```

## Migration
Migration from one code base to another.

Prompt example to start examining the process to convert from one codebase to another:
```
You are an expert in [technology e.g. Springboot and Camel], please explain step by step how you would redesign and convert this from [ Source implementation e.g. Camel] to [target implementation e.g. Springboot native]?
```

### Migrate by example
Using an example migration for copilot to follow is a great way of getting really good results with minimal prompting.

```
Consider the Pyspark file <INSERT BLUEPRINT FILE PATH HERE> which was migrated from the C++ file "<BLUEPRINT SOURCE C++ FILE>"
Create a Pyspark implementation of the "<C++ FILE TO BE MIGRATED>" using a similar approach.
```

### Migrate by flow
This is a good option for complicated or multi-step migrations. Use a tool such as draw.io to create a flow diagram that describes the process required. Each box in the flow diagram can be a prompt. Export the diagram as XML and provide it as context for a migration prompt.

### Migrate by steps
This is good to break the process into steps that copilot can undertake to achieve a goal.
```
Step 1: Get DB2 Columns
Please list all the column names for the DB2 tables for the <ENTITY> entity using this process.

Step 2: Generate SQL
Create a SQL query to fetch all <ENTITY> fields from the database using the DDL to create the necessary JOINS on each table in the query.

Step 3: Generate MyBatis XML mapper
Create me a MyBatis xml mapper file for the <ENTITY> object, using the sql output from Step 2. 

Step 4: Add WHERE clause
Add where clause to MyBatis XML output from Step 3. 
```

### Migrate by pseudo-code
This is a good option for when there are many similar migrations. Create a good prompt using one of the methods mentioned. Then, parameterise the prompt to make it reusable across the different but similar migrations.

A simple example:
```
$FILEPATH="myapp/src/module1"
$DTONAME="module1_dto"

Convert the file $FILEPATH from COBOL to C#. Use the DTO $DTONAME
```

A more complicated example that brings together some of these ideas:
```
You should use the following variables:
$Drawio="<SQL GENERATION.drawio>"
$EntityName="<ENTITY>"
$DDLFile="<REFERENCE\DDL.sql>"
$DAOInterface="<SERVICES\SRC\MYDAO.java>"

Step 1: Get DB2 Columns
There is a process to map a field in COBOL to a DB2 database column which is in the file $Drawio. 
List all the column names for the DB2 tables for the $EntityName entity using this process.

Step 2: Generate SQL
The DDL file $DDLFile  defines the database schema.
Create a SQL query to fetch all $EntityName fields from the database using the DDL to create the necessary JOINS on each table in the query.

Step 3: Generate MyBatis XML mapper
Create a MyBatis xml mapper file for the $EntityName object, using the sql output from Step 2. 
The object has an specified interface which is represented as a DAO with file $DAOInterface. 

Step 4: Add WHERE clause
Add where clause to MyBatis XML output from Step 3. 
The DDL file $DDLFile specifies the database schema. Supplement selection criteria from DAO and the database using the DDL.
```
### Migrate UI code
Use copilot to migrate complicated legacy code screens to modern web frameworks.
```
I have many complex Java Swing UIs that I want to convert to React. The top level screen UIs have many tabs and sub windows, and example of this is file <FILE PATH>. Copilot is not having much success doing this due to the complexity of the UI.
 
Create a detailed generic plan in a README.md that consists of incremental steps that copilot can follow. The steps should break the process of migration to React into more manageable steps that would give copilot greater accuracy.
 
For example:
Analyse the top level Java screen, noting the properties of all UI components on the screen.
Consider how these components would be recreated in React and create a plan to build this in this empty React app <REACT APP FOLDER>
Check that the screen matches exactly the source screen.
Consider all sub-screens of the top level screen etc

This is just an example, you should use your own understanding to create the best plan. You should be able to do this migration by using just the Java source code.
```

## Review and Remediation
### SonarQube
If you use SonarQube for code scanning, you can use copilot to address flagged issues. Use an MCP Server such as [here](https://github.com/SonarSource/sonarqube-mcp-server) to allow copilot to query issues and resolve. 
[see also](https://community.sonarsource.com/t/introducing-the-sonarqube-mcp-server-alpha/144204)

# Workarounds, Common Problems, Troubleshooting
Fixes and workarounds for common issues

## Context limit exceeded
When working with large codebases it is not uncommon to hit the context window for the underlying LLM. For the most part VSCode (or other IDEs) will do their best to restrict context to only that necessary but that is not always enough. If possible create summarisation artifacts for your codebase, for example multiple ReadMe.mds, and include these as context explicitly rather than allowing Copilot to pull in many files from the workspace (use the "re-run without workspace" link when needed). You can troubleshoot context problems by reviewing context being packaged and sent to the back-end LLM - to do this view the *Output* terminal tab and switch the source to *GitHub Copilot Chat*. Here you review the .md files being prepared and sent with each request.

## Response matched public code
Sometimes you will receive a message saying that the reponse was blocked as it matched public code. This is due to organisation level policy on what can be returned from Copilot. There are a few things to work around this issue, such as rephrasing, switching to a different mode (e.g. to "Ask" mode), or switching model. You can find some more tips to overcome this [here](https://github.com/orgs/community/discussions/159805)
