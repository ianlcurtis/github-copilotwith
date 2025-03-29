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

## Documentation of current code

Example prompt to give a high level description of what a project does.
```
Can you explain what this project does?
```

Example prompt to give a description of a specific file e.g. an API controller.
```
Explain the [file name], what is the input, its output and what is it dependent upon. Note that I do not know this codebase, do not make any assumptions about my knowledge.
```

Example prompt to document a file e.g. route xml in Java
```
Can you generate a mermaid diagram for this API project for our documentation? Additionally include a synopsis of each route formatted like this:
 
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

Prompt to write test cases:
```
Can you help me write the test cases, first for the existing code. The tests should compliment those that exist already, and create coverage for happy and unhappy path plus edge cases.
```

Prompt to identify test coverage:
```
Are there any additional tests I should include, or are all test angles covered?
```

## Migration
Migration from one code base to another.

Prompt example to start examining the process to convert from one codebase to another:
```
You are an expert in [technology e.g. Springboot and Camel], please explain step by step how you would redesign and convert this from [ Source implementation e.g. Camel] to [target implementation e.g. Springboot native]?
```
```
You are an expert in [technology e.g. Springboot and Apache Camel], please explain step by step how you would redesign and convert the [file] to a [target implementation e.g. native Spring] implementation. Please ensure that you follow best practice and ensure that you generate any unit tests for the converted implementation.
```


## Type
The type of solution being migrated.
- API
