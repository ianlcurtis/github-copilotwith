# GitHub Copilot Use Case Solutions
A library of use case archetypes and GHCP solutions.

# Greenfield
TBD

# Brownfield
## Documentation of current code

Example prompt to explain a migration approach
```
You are an expert in [technology e.g. Springboot and Camel], please explain step by step how you would redesign and convert this from [ Source implementation e.g. Camel] to [target implementation e.g. Springboot native]?
```

Example prompt to document a route xml file in Java
```
Can you generate a mermaid diagram for this API project for our documentation? Additionally include a synopsis of each route formatted like this:
 
api/route:
Purpose:
Special parameters:
Error handling:
Headers:
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
