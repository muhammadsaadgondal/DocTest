# Code Examples

This document provides practical examples for using DocTest in various scenarios.

## Table of Contents

1. [Basic Examples](#basic-examples)
2. [Advanced Examples](#advanced-examples)
3. [Integration Examples](#integration-examples)
4. [Custom Scenarios](#custom-scenarios)

## Basic Examples

### Example 1: Simple Test Execution

```javascript
const DocTest = require('doctest');

// Create a new DocTest instance
const doctest = new DocTest({
  debug: false,
  verbose: true
});

// Run tests on all markdown files
async function runTests() {
  try {
    const results = await doctest.run('docs/**/*.md');
    
    console.log(`Total tests: ${results.total}`);
    console.log(`Passed: ${results.passed}`);
    console.log(`Failed: ${results.failed}`);
    
    if (results.failed > 0) {
      process.exit(1);
    }
  } catch (error) {
    console.error('Error running tests:', error);
    process.exit(1);
  }
}

runTests();
```

### Example 2: Testing Specific Files

```javascript
const DocTest = require('doctest');

const doctest = new DocTest();

async function testSpecificFiles() {
  // Test multiple specific files
  const files = [
    'docs/getting-started.md',
    'docs/api/api-reference.md',
    'docs/guides/user-guide.md'
  ];
  
  const results = await doctest.run(files);
  
  // Process results
  results.tests.forEach(test => {
    if (test.status === 'failed') {
      console.error(`❌ ${test.name}`);
      console.error(`   ${test.error.message}`);
    } else {
      console.log(`✓ ${test.name}`);
    }
  });
}

testSpecificFiles();
```

### Example 3: Validation

```javascript
const DocTest = require('doctest');

const doctest = new DocTest();

async function validateDocs() {
  const validation = await doctest.validate('docs/');
  
  if (validation.isValid) {
    console.log('✓ All documentation is valid!');
  } else {
    console.error('Validation errors found:');
    
    validation.errors.forEach(error => {
      console.error(`  ${error.file}:${error.line}`);
      console.error(`  ${error.message}`);
    });
    
    if (validation.warnings.length > 0) {
      console.warn('\nWarnings:');
      validation.warnings.forEach(warning => {
        console.warn(`  ${warning.file}:${warning.line}`);
        console.warn(`  ${warning.message}`);
      });
    }
  }
}

validateDocs();
```

## Advanced Examples

### Example 4: Custom Configuration

```javascript
const DocTest = require('doctest');
const { Config } = require('doctest');

// Load custom configuration
const config = Config.load('./doctest.config.js');

// Override specific settings
config.set('timeout', 10000);
config.set('retries', 3);

// Create DocTest with custom config
const doctest = new DocTest(config.toObject());

async function runWithCustomConfig() {
  const results = await doctest.run('docs/', {
    filter: '*.integration.md',
    parallel: true,
    maxWorkers: 4
  });
  
  console.log(results);
}

runWithCustomConfig();
```

### Example 5: Watch Mode

```javascript
const DocTest = require('doctest');

const doctest = new DocTest({ verbose: true });

// Start watching files
const watcher = doctest.watch('docs/**/*.md', {
  ignoreInitial: true,
  interval: 500
});

// Handle events
watcher.on('change', async (file) => {
  console.log(`\n📝 File changed: ${file}`);
  console.log('Running tests...\n');
  
  try {
    const results = await doctest.run(file);
    
    if (results.failed === 0) {
      console.log('✓ All tests passed!');
    } else {
      console.error(`✗ ${results.failed} test(s) failed`);
    }
  } catch (error) {
    console.error('Error:', error.message);
  }
});

watcher.on('error', (error) => {
  console.error('Watcher error:', error);
});

console.log('👀 Watching for changes...');
console.log('Press Ctrl+C to stop');
```

### Example 6: Event Handling

```javascript
const DocTest = require('doctest');

const doctest = new DocTest();

// Subscribe to events
doctest.on('test:start', (test) => {
  console.log(`Starting: ${test.name}`);
});

doctest.on('test:pass', (test, result) => {
  console.log(`✓ Passed: ${test.name} (${result.duration}ms)`);
});

doctest.on('test:fail', (test, error) => {
  console.error(`✗ Failed: ${test.name}`);
  console.error(`  ${error.message}`);
});

doctest.on('run:complete', (results) => {
  console.log('\n' + '='.repeat(50));
  console.log('Test Summary');
  console.log('='.repeat(50));
  console.log(`Total:   ${results.total}`);
  console.log(`Passed:  ${results.passed}`);
  console.log(`Failed:  ${results.failed}`);
  console.log(`Skipped: ${results.skipped}`);
  console.log(`Duration: ${results.duration}ms`);
});

// Run tests
doctest.run('docs/');
```

## Integration Examples

### Example 7: GitHub Actions Integration

```yaml
# .github/workflows/test-docs.yml
name: Documentation Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Install DocTest
      run: npm install -g doctest
    
    - name: Run documentation tests
      run: doctest run docs/
    
    - name: Generate report
      if: always()
      run: doctest report docs/ --format html > report.html
    
    - name: Upload report
      if: always()
      uses: actions/upload-artifact@v2
      with:
        name: test-report
        path: report.html
```

### Example 8: Express.js Integration

```javascript
const express = require('express');
const DocTest = require('doctest');

const app = express();
const doctest = new DocTest();

// Endpoint to run tests
app.post('/api/test-docs', async (req, res) => {
  try {
    const { files } = req.body;
    const results = await doctest.run(files || 'docs/');
    
    res.json({
      success: results.failed === 0,
      results: {
        total: results.total,
        passed: results.passed,
        failed: results.failed,
        duration: results.duration
      }
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
});

// Endpoint to validate docs
app.post('/api/validate-docs', async (req, res) => {
  try {
    const { files } = req.body;
    const validation = await doctest.validate(files || 'docs/');
    
    res.json({
      valid: validation.isValid,
      errors: validation.errors,
      warnings: validation.warnings
    });
  } catch (error) {
    res.status(500).json({
      valid: false,
      error: error.message
    });
  }
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### Example 9: Jest Integration

```javascript
// tests/documentation.test.js
const DocTest = require('doctest');

describe('Documentation Tests', () => {
  let doctest;
  
  beforeAll(() => {
    doctest = new DocTest({ debug: false });
  });
  
  test('all documentation files should be valid', async () => {
    const validation = await doctest.validate('docs/');
    expect(validation.isValid).toBe(true);
    expect(validation.errors).toHaveLength(0);
  });
  
  test('getting started guide code examples should pass', async () => {
    const results = await doctest.run('docs/getting-started.md');
    expect(results.failed).toBe(0);
  });
  
  test('API reference examples should be valid', async () => {
    const results = await doctest.run('docs/api/api-reference.md');
    expect(results.passed).toBeGreaterThan(0);
    expect(results.failed).toBe(0);
  });
});
```

## Custom Scenarios

### Example 10: Custom Test Runner

```javascript
const DocTest = require('doctest');
const { TestRunner, Test } = require('doctest');

class CustomTestRunner extends TestRunner {
  async execute(test, context) {
    // Add custom setup
    console.log(`Setting up test: ${test.name}`);
    
    // Call parent execute
    const result = await super.execute(test, context);
    
    // Add custom teardown
    console.log(`Tearing down test: ${test.name}`);
    
    return result;
  }
}

// Use custom runner
const doctest = new DocTest({
  runner: new CustomTestRunner()
});

doctest.run('docs/');
```

### Example 11: Custom Parser

```javascript
const { Parser } = require('doctest');

class CustomParser extends Parser {
  parse(content, options) {
    // Custom parsing logic
    const blocks = super.parse(content, options);
    
    // Add custom metadata
    blocks.forEach(block => {
      block.metadata.customField = 'custom value';
    });
    
    return blocks;
  }
}

const parser = new CustomParser();
const result = parser.parse(markdownContent);
```

### Example 12: Custom Reporter

```javascript
const { Reporter } = require('doctest');

class CustomReporter extends Reporter {
  generate(results, format) {
    if (format === 'custom') {
      // Generate custom format
      return this.generateCustomFormat(results);
    }
    
    return super.generate(results, format);
  }
  
  generateCustomFormat(results) {
    return JSON.stringify({
      summary: {
        total: results.total,
        passed: results.passed,
        failed: results.failed
      },
      details: results.tests.map(test => ({
        name: test.name,
        status: test.status,
        duration: test.duration
      }))
    }, null, 2);
  }
}

const reporter = new CustomReporter();
const report = reporter.generate(testResults, 'custom');
console.log(report);
```

## Utility Examples

### Example 13: Batch Testing

```javascript
const DocTest = require('doctest');
const glob = require('glob');

async function batchTest() {
  const doctest = new DocTest();
  
  // Find all markdown files
  const files = glob.sync('docs/**/*.md');
  
  // Test in batches
  const batchSize = 5;
  const results = [];
  
  for (let i = 0; i < files.length; i += batchSize) {
    const batch = files.slice(i, i + batchSize);
    console.log(`Testing batch ${i / batchSize + 1}...`);
    
    const batchResults = await Promise.all(
      batch.map(file => doctest.run(file))
    );
    
    results.push(...batchResults);
  }
  
  // Aggregate results
  const summary = results.reduce((acc, result) => {
    acc.total += result.total;
    acc.passed += result.passed;
    acc.failed += result.failed;
    return acc;
  }, { total: 0, passed: 0, failed: 0 });
  
  console.log('Summary:', summary);
}

batchTest();
```

### Example 14: Error Recovery

```javascript
const DocTest = require('doctest');

async function robustTesting() {
  const doctest = new DocTest({ retries: 3 });
  
  const files = ['docs/api.md', 'docs/guide.md', 'docs/examples.md'];
  
  for (const file of files) {
    let attempts = 0;
    let success = false;
    
    while (attempts < 3 && !success) {
      try {
        console.log(`Testing ${file} (attempt ${attempts + 1})...`);
        const results = await doctest.run(file);
        
        if (results.failed === 0) {
          console.log(`✓ ${file} passed`);
          success = true;
        } else {
          attempts++;
        }
      } catch (error) {
        attempts++;
        console.error(`Attempt ${attempts} failed:`, error.message);
        
        if (attempts < 3) {
          await new Promise(resolve => setTimeout(resolve, 1000));
        }
      }
    }
    
    if (!success) {
      console.error(`✗ ${file} failed after 3 attempts`);
    }
  }
}

robustTesting();
```

## See Also

- [API Reference](api-reference.md) - Complete API documentation
- [User Guide](../guides/user-guide.md) - User guide and best practices
- [Configuration](../configuration.md) - Configuration options

---

*Last updated: December 2025*
