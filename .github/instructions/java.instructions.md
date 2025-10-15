---
description: 'Guidelines for building Java base applications'
applyTo: '**/*.java'
---

# Java Development

## General Instructions

- First, prompt the user if they want to integrate static analysis tools (SonarQube, PMD, Checkstyle)
  into their project setup. If yes, provide guidance on tool selection and configuration.
- If the user declines static analysis tools or wants to proceed without them, continue with implementing the Best practices, bug patterns and code smell prevention guidelines outlined below.
- Address code smells proactively during development rather than accumulating technical debt.
- Focus on readability, maintainability, and performance when refactoring identified issues.
- Use IDE / Code editor reported warnings and suggestions to catch common patterns early in development.
- if using maven or Java, do check if the user has this installed - prompt the user to install if they do not have these. do check it could be that the environment variables are just not set correctly. 
  
## Best practices
- Follow Oracle Java Code Conventions and Google Java Style Guide
- Use meaningful variable, method, and class names that reflect business purpose
- Maximum method length: 50 lines (prefer smaller, focused methods)
- Maximum class length: 500 lines (consider composition over large classes)
- Use proper exception handling with specific exception types
- Document all public APIs with comprehensive Javadoc
- Use Java 8+ features appropriately (streams, lambdas, Optional)
- Prefer immutable objects where possible
- Use SLF4J with Logback for consistent logging- 
- **Records**: For classes primarily intended to store data (e.g., DTOs, immutable data structures), **Java Records should be used instead of traditional classes**.
- **Pattern Matching**: Utilize pattern matching for `instanceof` and `switch` expression to simplify conditional logic and type casting.
- **Type Inference**: Use `var` for local variable declarations to improve readability, but only when the type is explicitly clear from the right-hand side of the expression.
- **Immutability**: Favor immutable objects. Make classes and fields `final` where possible. Use collections from `List.of()`/`Map.of()` for fixed data. Use `Stream.toList()` to create immutable lists.
- **Streams and Lambdas**: Use the Streams API and lambda expressions for collection processing. Employ method references (e.g., `stream.map(Foo::toBar)`).
- **Null Handling**: Avoid returning or accepting `null`. Use `Optional<T>` for possibly-absent values and `Objects` utility methods like `equals()` and `requireNonNull()`.

### Naming Conventions

- Follow Google's Java style guide:
  - `UpperCamelCase` for class and interface names.
  - `lowerCamelCase` for method and variable names.
  - `UPPER_SNAKE_CASE` for constants.
  - `lowercase` for package names.
- Use nouns for classes (`UserService`) and verbs for methods (`getUserById`).
- Avoid abbreviations and Hungarian notation.

### Bug Patterns

| Rule ID | Description                                                 | Example / Notes                                                                                  |
| ------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `S2095` | Resources should be closed                                  | Use try-with-resources when working with streams, files, sockets, etc.                           |
| `S1698` | Objects should be compared with `.equals()` instead of `==` | Especially important for Strings and boxed primitives.                                           |
| `S1905` | Redundant casts should be removed                           | Clean up unnecessary or unsafe casts.                                                            |
| `S3518` | Conditions should not always evaluate to true or false      | Watch for infinite loops or if-conditions that never change.                                     |
| `S108`  | Unreachable code should be removed                          | Code after `return`, `throw`, etc., must be cleaned up.                                          |

## Code Smells

| Rule ID | Description                                            | Example / Notes                                                               |
| ------- | ------------------------------------------------------ | ----------------------------------------------------------------------------- |
| `S107`  | Methods should not have too many parameters            | Refactor into helper classes or use builder pattern.                          |
| `S121`  | Duplicated blocks of code should be removed            | Consolidate logic into shared methods.                                        |
| `S138`  | Methods should not be too long                         | Break complex logic into smaller, testable units.                             |
| `S3776` | Cognitive complexity should be reduced                 | Simplify nested logic, extract methods, avoid deep `if` trees.                |
| `S1192` | String literals should not be duplicated               | Replace with constants or enums.                                              |
| `S1854` | Unused assignments should be removed                   | Avoid dead variables—remove or refactor.                                      |
| `S109`  | Magic numbers should be replaced with constants        | Improves readability and maintainability.                                     |
| `S1188` | Catch blocks should not be empty                       | Always log or handle exceptions meaningfully.                                 |

## Build and Verification

- After adding or modifying code, verify the project continues to build successfully.
- If the project uses Maven, run `mvn clean install`.
- If the project uses Gradle, run `./gradlew build` (or `gradlew.bat build` on Windows).
- Ensure all tests pass as part of the build.

## Dependencies Management
- Use Maven for dependency management with explicit version declarations
- Pin dependency versions to avoid conflicts (no version ranges)
- Group dependencies logically in pom.xml with comments
- Use dependency management for consistent versions across modules
- Document dependency choices and alternatives in README
- Regularly update dependencies for security patches

## Java XML Integration

### JAXB Best Practices
- **Generate from XSD**: Always generate Java classes from XSD, not vice versa
- **Package Organization**: Use jaxb:bindings to control package structure
- **Custom Bindings**: Customize type mappings with external binding files
- **Validation on Unmarshal**: Enable validation during unmarshalling
- **Handle JAXB Exceptions**: Properly handle JAXBException and validation errors

### JAXB Annotations
- **Minimal Code Changes**: Avoid modifying generated classes; use adapters instead
- **XmlAdapter**: Use XmlAdapter for custom type conversions
- **XmlRootElement**: Ensure root elements have @XmlRootElement annotation
- **XmlAccessType**: Choose appropriate access type (FIELD, PROPERTY, etc.)
- **Date/Time Handling**: Use proper adapters for Java 8 Date/Time types

### Marshalling and Unmarshalling
- **Thread Safety**: JAXBContext is thread-safe; Marshaller/Unmarshaller are not
- **Reuse Context**: Create JAXBContext once and reuse (expensive operation)
- **Pretty Printing**: Enable formatted output for human-readable XML
- **Encoding**: Explicitly set encoding (UTF-8) for marshalling
- **Fragment Marshalling**: Use fragment mode when marshalling without XML declaration

### XML Parsing Strategies in Java

#### SAX (Simple API for XML)
- **Use Case**: Streaming large XML files (2M+ records)
- **Advantages**: Memory efficient, fast for sequential processing
- **Disadvantages**: Event-driven, can't navigate backwards
- **When to Use**: Large files, one-pass processing, memory constrained

#### DOM (Document Object Model)
- **Use Case**: Small files requiring random access
- **Advantages**: Full document in memory, easy navigation, can modify
- **Disadvantages**: High memory usage for large files
- **When to Use**: Small files, need to modify XML, random access required

#### StAX (Streaming API for XML)
- **Use Case**: Selective parsing of large documents
- **Advantages**: Pull parsing, more control than SAX, memory efficient
- **Disadvantages**: More complex than DOM
- **When to Use**: Large files, need selective parsing, want more control than SAX

### XML Generation in Java

#### XMLStreamWriter
- **Use Case**: Generating large XML documents efficiently
- **Best Practices**:
  - Use try-with-resources for proper cleanup
  - Set proper encoding (UTF-8)
  - Flush and close streams properly
  - Use namespace-aware methods

```java
try (XMLStreamWriter writer = XMLOutputFactory.newInstance()
    .createXMLStreamWriter(outputStream, "UTF-8")) {
    writer.writeStartDocument("UTF-8", "1.0");
    writer.writeStartElement("root");
    // ... write elements
    writer.writeEndElement();
    writer.writeEndDocument();
}
```

### XML Security in Java

#### Secure DocumentBuilderFactory
```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
dbf.setFeature("http://xml.org/sax/features/external-general-entities", false);
dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
dbf.setXIncludeAware(false);
dbf.setExpandEntityReferences(false);
```

#### Secure SAXParserFactory
```java
SAXParserFactory spf = SAXParserFactory.newInstance();
spf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
spf.setFeature("http://xml.org/sax/features/external-general-entities", false);
spf.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
```

#### Security Best Practices
- **Prevent XXE Attacks**: Always disable external entities
- **Limit Entity Expansion**: Protect against billion laughs attack
- **Disable DTD Processing**: If not needed, disable completely
- **Validate Input**: Always validate against known schemas
- **Use Security Libraries**: Consider OWASP Java XML security libraries

### Recommended Java XML Libraries

#### JAXB (Jakarta XML Binding)
- **Use Case**: Schema-first XML binding
- **Version**: Separate library for Java 11+ (was built into Java 8-10)
- **Advantages**: Type-safe, generated from XSD, standard
- **Best For**: Enterprise applications, schema-driven development

#### Jackson XML
- **Use Case**: High-performance XML processing
- **Advantages**: Fast, flexible, integrates with Jackson ecosystem
- **Best For**: REST APIs, high-throughput applications

#### Woodstox
- **Use Case**: Fast StAX implementation
- **Advantages**: High performance, fully compliant StAX
- **Best For**: Large file processing, streaming scenarios

#### XStream
- **Use Case**: Simple object-to-XML serialization
- **Advantages**: Easy to use, minimal configuration
- **Best For**: Quick prototypes, simple serialization needs

#### Saxon
- **Use Case**: Advanced XSLT and XQuery processing
- **Advantages**: Full XSLT 3.0 support, XQuery, schema-aware
- **Best For**: Complex transformations, XSLT processing

### Java XML Performance Optimization

#### Memory Management
- **Stream Large Files**: Use SAX/StAX for files > 100MB
- **Object Pooling**: Pool frequently created objects in tight loops
- **Batch Processing**: Generate XML in chunks (e.g., 500k records per file)
- **Buffer Sizing**: Configure appropriate buffer sizes (8KB-64KB typical)

#### Processing Optimization
- **Reuse Parsers**: Pool and reuse parser instances
- **Cache Schemas**: Compile and cache XSD schemas
- **Parallel Processing**: Use parallel streams for independent XML processing
- **Lazy Loading**: Parse XML lazily when possible
- **Disable Pretty Printing**: In production, avoid formatting overhead

#### JAXB Optimization
- **Reuse JAXBContext**: Create once, reuse across operations (expensive to create)
- **Pool Marshallers**: Use object pools for Marshaller/Unmarshaller instances
- **Disable Validation**: Skip validation if already validated upstream
- **Use Fragments**: Avoid XML declaration overhead with fragment mode

### Common Java XML Pitfalls

#### JAXB Pitfalls
- ❌ **Recreating JAXBContext**: Creating new context for each operation (very expensive)
- ❌ **Sharing Marshallers**: Sharing Marshaller/Unmarshaller across threads (not thread-safe)
- ❌ **Modifying Generated Code**: Changing JAXB-generated classes (use adapters instead)
- ❌ **Not Handling Exceptions**: Ignoring JAXBException and validation errors
- ❌ **Forgetting Validation**: Not enabling validation during unmarshalling

#### Performance Pitfalls
- ❌ **Loading Entire Document**: Loading massive XML files completely into memory
- ❌ **Using DOM for Large Files**: DOM loads entire document, use SAX/StAX instead
- ❌ **Not Closing Streams**: Forgetting to close XML streams (use try-with-resources)
- ❌ **Excessive Pretty Printing**: Formatting XML in high-throughput scenarios
- ❌ **Ignoring Buffer Sizes**: Using default buffer sizes for large operations

#### Security Pitfalls
- ❌ **Not Disabling XXE**: Leaving external entity processing enabled
- ❌ **Parsing Untrusted XML**: Not securing parsers against attacks
- ❌ **Ignoring DTD Attacks**: Not protecting against entity expansion attacks
- ❌ **Missing Validation**: Processing XML without schema validation

### Java XML Testing

#### Unit Testing
- **Test Marshalling**: Verify Java objects marshal to expected XML
- **Test Unmarshalling**: Verify XML unmarshals to correct Java objects
- **Test Validation**: Ensure validation catches invalid XML
- **Test Edge Cases**: Empty elements, null values, special characters

#### Integration Testing
- **Schema Compliance**: Validate generated XML against XSD
- **Round-Trip Testing**: Marshal then unmarshal, verify object equality
- **Performance Testing**: Benchmark large file processing
- **Concurrent Testing**: Test thread safety of XML processing code

#### Testing Tools
- **XMLUnit**: Compare XML documents in tests
- **AssertJ XML**: Fluent assertions for XML
- **JUnit 5**: Modern testing framework
- **Mockito**: Mock XML dependencies