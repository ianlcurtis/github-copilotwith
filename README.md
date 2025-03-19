# GitHub Copilot Use Case Solutions
A library of use case archetypes and GHCP solutions.

# Greenfield
TBD

# Brownfield
## Documentation of current code

Example prompt to document a route xml file in Java
```
Can you read this file and generate a mermaid diagram of API for our documentation?

If you can include a synopsis of each route's use as well like this:

api/route:

Does this thing
Special parameters
Error handling
Headers
```

## Migration
Migration from one code base to another.

Prompt example to start examining the process to convert a Java Camel app to SpringBoot:

```
"You are an expert in Springboot and Camel, please explain step by step how you would redesign and convert this from Camel to Springboot native?"
```

### Process
The steps where GHCP can help.
- Document
- Test as-is
- Migrate
- Review
- Test migrated

## Type
The type of solution being migrated.
- API
