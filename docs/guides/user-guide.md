# User Guide

Welcome to the DocTest User Guide! This comprehensive guide will help you understand and use all features of DocTest.

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Usage](#basic-usage)
3. [Advanced Features](#advanced-features)
4. [Best Practices](#best-practices)
5. [FAQ](#faq)
6. [Getting Help](#getting-help)
7. [Next Steps](#next-steps)

## Introduction

DocTest is a powerful documentation testing and management tool designed to help developers maintain high-quality project documentation.

### Key Features

- **Documentation Testing**: Automatically test code examples in documentation
- **Version Control**: Track documentation changes over time
- **Multi-format Support**: Markdown, HTML, and plain text
- **Integration**: Works with popular CI/CD pipelines

## Basic Usage

### Creating Your First Document

1. Create a new markdown file:
   ```bash
   touch docs/my-first-doc.md
   ```

2. Add content with code examples:
   ````markdown
   # My First Document
   
   Here's a code example:
   ```javascript
   console.log('Hello, DocTest!');
   ```
   ````

3. Test your documentation:
   ```bash
   doctest run docs/my-first-doc.md
   ```

### Running Tests

DocTest can automatically extract and test code examples from your documentation:

```bash
# Test all documentation
doctest run docs/

# Test specific file
doctest run docs/getting-started.md

# Test with verbose output
doctest run --verbose docs/
```

### Organizing Documentation

Organize your documentation in a logical structure:

```
docs/
├── getting-started.md
├── configuration.md
├── guides/
│   ├── user-guide.md
│   └── contributing.md
└── api/
    ├── api-reference.md
    └── examples.md
```

## Advanced Features

### Code Block Testing

DocTest supports testing various programming languages:

#### JavaScript/Node.js

````markdown
```javascript
const result = add(2, 3);
assert(result === 5);
```
````

#### Python

````markdown
```python
def add(a, b):
    return a + b

assert add(2, 3) == 5
```
````

#### Shell Commands

````markdown
```bash
echo "Hello, World!"
# Expected output: Hello, World!
```
````

### Configuration Testing

Test configuration files and validate their structure:

```bash
doctest validate config/app.config.js
```

### Continuous Integration

Integrate DocTest into your CI/CD pipeline:

**GitHub Actions Example:**

```yaml
name: Documentation Tests
on: [push, pull_request]

jobs:
  test-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install DocTest
        run: npm install -g doctest
      - name: Run Documentation Tests
        run: doctest run docs/
```

### Custom Test Scenarios

Create custom test scenarios for complex examples:

```javascript
// doctest.config.js
module.exports = {
  scenarios: {
    'api-test': {
      setup: async () => {
        // Setup test server
      },
      teardown: async () => {
        // Cleanup
      }
    }
  }
};
```

## Best Practices

### 1. Write Testable Examples

Always write code examples that can be executed and tested:

```javascript
// Good: Complete, runnable example
const sum = (a, b) => a + b;
console.log(sum(2, 3)); // Output: 5

// Avoid: Incomplete snippets
const sum = (a, b) => ...
```

### 2. Keep Documentation Updated

- Update documentation when code changes
- Run documentation tests before committing
- Review documentation in code reviews

### 3. Use Clear Structure

- Use consistent heading levels
- Include a table of contents for long documents
- Link related documents

### 4. Add Context

Provide context for code examples:

```markdown
The following example demonstrates how to configure the database connection:

```javascript
const config = {
  host: 'localhost',
  port: 5432,
  database: 'mydb'
};
```
```

### 5. Version Your Documentation

- Tag documentation versions with releases
- Maintain documentation for multiple versions
- Archive old documentation

## FAQ

### How do I skip a code block from testing?

Use the `skip` annotation:

````markdown
```javascript skip
// This code won't be tested
broken.code.here();
```
````

### Can I test asynchronous code?

Yes! DocTest supports async/await:

````markdown
```javascript
async function fetchData() {
  const response = await fetch('/api/data');
  return response.json();
}
```
````

### How do I handle environment-specific code?

Use conditional testing:

````markdown
```javascript
if (process.env.NODE_ENV === 'production') {
  // Production-only code
}
```
````

### What file formats are supported?

DocTest supports:
- Markdown (.md)
- HTML (.html)
- Plain text (.txt)
- AsciiDoc (.adoc)

### How do I contribute to documentation?

See our [Contributing Guide](contributing.md) for detailed instructions.

## Getting Help

- **Documentation**: Check our [API Reference](../api/api-reference.md)
- **Issues**: Report bugs on [GitHub Issues](https://github.com/muhammadsaadgondal/DocTest/issues)
- **Community**: Join our community discussions

## Next Steps

- Explore [API Reference](../api/api-reference.md) for detailed API documentation
- Check out [Examples](../api/examples.md) for more code samples
- Read [Contributing Guide](contributing.md) to contribute to the project

---

*Last updated: December 2025*
