---
applyTo:
  - '**/*.xml'
  - '**/*.xsd'
  - '**/*.xsl'
  - '**/*.xslt'
---

# XML and XSD Best Practices

This file provides guidelines for working with XML documents, XSD schemas, and related technologies.

## XML Document Best Practices

### Structure and Formatting
- **Use Consistent Indentation**: Use 2 or 4 spaces for indentation (avoid tabs)
- **One Element Per Line**: Keep elements on separate lines for readability
- **Meaningful Element Names**: Use descriptive, self-documenting element names
- **Use CamelCase or kebab-case**: Be consistent with naming convention throughout
- **Avoid Deep Nesting**: Keep XML structure as flat as practical (max 5-7 levels)
- **Close Empty Elements**: Use self-closing tags `<element/>` for empty elements

### Namespaces
- **Declare at Root**: Declare all namespaces at the root element when possible
- **Use Prefixes Consistently**: Maintain consistent namespace prefixes across related documents
- **Default Namespace**: Use default namespace for primary content, prefixes for others
- **Avoid Namespace Proliferation**: Minimize the number of different namespaces

### Attributes vs Elements
- **Metadata as Attributes**: Use attributes for metadata (IDs, types, flags)
- **Content as Elements**: Use elements for actual data content
- **No Complex Data in Attributes**: Attributes should contain simple scalar values only
- **Consider Extensibility**: Elements are more extensible than attributes

### Data Types and Values
- **Use ISO Standards**: ISO 8601 for dates/times, ISO 4217 for currencies
- **Validate Data**: Ensure data conforms to expected types and formats
- **Handle Special Characters**: Properly escape XML special characters (&lt;, &gt;, &amp;, &quot;, &apos;)
- **CDATA for Complex Content**: Use CDATA sections for content with special characters or formatting
- **Whitespace Handling**: Be explicit about whitespace significance

### Comments and Documentation
- **Document Purpose**: Include comments explaining the purpose of complex structures
- **Version Information**: Include schema version and document version where applicable
- **Processing Instructions**: Document any required processing instructions
- **Avoid Over-Commenting**: Don't state the obvious; comment intent, not syntax

## XSD Schema Best Practices

### Schema Design
- **Schema-First Development**: Define XSD schema before generating code
- **Modular Design**: Split large schemas into multiple files using `xs:include` or `xs:import`
- **Reusable Types**: Define common types once and reuse via `xs:complexType` and `xs:simpleType`
- **Clear Type Names**: Use descriptive names ending in "Type" (e.g., `AddressType`, `PersonType`)
- **Documentation**: Use `xs:annotation` and `xs:documentation` extensively

### Type Definitions
- **Simple Types for Constraints**: Use `xs:simpleType` with restrictions for constrained values
- **Pattern Validation**: Use `xs:pattern` with regex for format validation
- **Enumeration**: Use `xs:enumeration` for fixed value lists
- **Min/Max Constraints**: Apply `minOccurs` and `maxOccurs` appropriately. If these are missing then the default is that the element must only appear once. 
- **Default Values**: Provide sensible defaults where appropriate using `default` attribute

### Complex Types
- **Choice vs Sequence**: Use `xs:sequence` for ordered elements, `xs:choice` for alternatives
- **Extension vs Restriction**: Prefer extension for adding to base types
- **Abstract Types**: Use abstract types for polymorphism
- **Mixed Content**: Avoid mixed content unless absolutely necessary
- **Attribute Groups**: Group commonly-used attributes with `xs:attributeGroup`

### Versioning and Compatibility
- **Version Attribute**: Include version information in schema
- **Backward Compatibility**: Consider existing documents when updating schemas
- **Deprecation**: Mark deprecated elements with documentation before removal
- **Namespace Versioning**: Use namespace versioning for major breaking changes
- **Optional Elements**: Make new elements optional to maintain compatibility

### Performance Considerations
- **Avoid Deep Recursion**: Limit recursive element definitions
- **Optimize Wildcards**: Use specific elements rather than `xs:any` when possible
- **Limit Pattern Complexity**: Keep regex patterns simple and efficient
- **Key References**: Use `xs:key` and `xs:keyref` for referential integrity

## XML Validation

### Schema Validation
- **Always Validate**: Validate XML against schema before processing
- **Handle Validation Errors**: Provide clear error messages for validation failures
- **Validate Early**: Validate at the earliest possible point in processing
- **Schema Caching**: Cache compiled schemas for performance
- **Partial Validation**: Consider validating incrementally for large documents

### Custom Validation
- **Business Rules**: Implement business logic validation separate from schema validation
- **Cross-Field Validation**: Validate relationships between fields not expressible in XSD
- **Schematron**: Consider Schematron for complex validation rules beyond XSD capabilities
- **Custom Validators**: Create reusable validators for common validation patterns

## Security Best Practices

### XML Security
- **Disable External Entities**: Prevent XXE (XML External Entity) attacks
- **Limit Entity Expansion**: Protect against billion laughs attack
- **Disable DTD Processing**: Disable DTD processing if not needed
- **Schema Validation**: Always validate against known schemas
- **Sanitize Input**: Sanitize XML content from untrusted sources

## Performance Optimization

### Parsing Strategies
- **Streaming for Large Files**: Use streaming approaches for large XML files
- **Random Access for Small Files**: Use tree-based parsing for small files requiring random access
- **Selective Parsing**: Use selective parsing for large documents
- **Memory Management**: Stream large XML to avoid loading entire document in memory

### Generation Strategies
- **Streaming Generation**: Use streaming approaches for generating large XML documents
- **Batching**: Generate XML in chunks rather than one massive document
- **Buffer Size**: Configure appropriate buffer sizes for I/O operations
- **Character Encoding**: Use efficient character encoding (UTF-8)

### Optimization Tips
- **Avoid Pretty Printing in Production**: Save space and processing time
- **Lazy Loading**: Parse XML lazily when possible
- **Index Key Elements**: Build indexes for frequently accessed elements
- **Pool Parsers**: Reuse parser instances where appropriate
- **Compression**: Consider compressing XML for storage/transmission

## Testing XML/XSD

### Schema Testing
- **Valid Documents**: Test with valid XML documents
- **Invalid Documents**: Test with intentionally invalid documents
- **Edge Cases**: Test boundary conditions and edge cases
- **Optional Elements**: Test all combinations of optional elements
- **Backward Compatibility**: Test new schemas with old documents
- **multiple namespaces**: XML files might contain multiple namespaces. Ensure that the test of the output files are able to handle for this.
### Validation Testing
- **Positive Tests**: Confirm valid documents pass validation
- **Negative Tests**: Confirm invalid documents fail with correct errors
- **Performance Tests**: Test validation performance with large documents
- **Concurrent Validation**: Test thread safety of validation code
- **Error Message Quality**: Verify error messages are clear and actionable

## Tools and Libraries

### Development Tools
- **Oxygen XML Editor**: Professional XML/XSD editor
- **XMLSpy**: Comprehensive XML development environment
- **IntelliJ IDEA**: Excellent XML/XSD support built-in
- **xmllint**: Command-line XML validation tool
- **xsdgen**: Command-line XSD to Java generator

## Common Pitfalls to Avoid

### Schema Design Pitfalls
- ❌ **Russian Doll Pattern**: Nesting all type definitions (hard to reuse)
- ❌ **Venetian Blind Pattern**: Using only global elements (loses type context)
- ❌ **Overly Permissive**: Using `xs:any` with `processContents="lax"` everywhere
- ❌ **Underspecified Types**: Not constraining string types with patterns or enumerations
- ❌ **Ignoring Namespaces**: Not properly managing namespace declarations

### Processing Pitfalls
- ❌ **Loading Entire Document**: Loading massive XML files completely into memory
- ❌ **Ignoring Validation Errors**: Proceeding with invalid XML
- ❌ **Parsing Untrusted XML**: Not securing parsers against attacks
- ❌ **Hardcoding Paths**: Using hardcoded XPath expressions instead of type-safe navigation
- ❌ **Ignoring Character Encoding**: Not specifying or handling encoding properly

## References and Resources

### Official Specifications
- W3C XML Specification: https://www.w3.org/TR/xml/
- W3C XML Schema Specification: https://www.w3.org/TR/xmlschema11-1/
- JAXB Specification: https://jakarta.ee/specifications/xml-binding/

### Best Practice Guides
- XML Design Patterns
- XSD Best Practices (various industry standards)
- OWASP XML Security Cheat Sheet
- Java XML Processing Best Practices

### Online Tools
- XSD Validation Tools
- XML Formatting and Validation
- XPath Testing Tools
- XSLT Transformation Testing
