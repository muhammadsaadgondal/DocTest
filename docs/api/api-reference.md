# API Reference

Complete API reference for DocTest.

## Table of Contents

1. [Core API](#core-api)
2. [Testing API](#testing-api)
3. [Configuration API](#configuration-api)
4. [Utilities API](#utilities-api)
5. [Events API](#events-api)

## Core API

### DocTest Class

The main class for interacting with DocTest.

#### Constructor

```javascript
new DocTest(options)
```

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| options | Object | No | Configuration options |
| options.debug | boolean | No | Enable debug mode (default: false) |
| options.verbose | boolean | No | Enable verbose output (default: false) |
| options.path | string | No | Path to documentation files |

**Example:**

```javascript
const DocTest = require('doctest');

const doctest = new DocTest({
  debug: true,
  path: './docs'
});
```

#### Methods

##### run()

Runs documentation tests.

```javascript
doctest.run(files, options)
```

**Parameters:**

- `files` (string|Array): File path(s) to test
- `options` (Object): Test options
  - `filter` (string): Filter tests by pattern
  - `timeout` (number): Test timeout in ms (default: 5000)

**Returns:** Promise<TestResults>

**Example:**

```javascript
const results = await doctest.run('docs/**/*.md', {
  timeout: 10000
});

console.log(`Tests passed: ${results.passed}`);
console.log(`Tests failed: ${results.failed}`);
```

##### validate()

Validates documentation structure and syntax.

```javascript
doctest.validate(files)
```

**Parameters:**

- `files` (string|Array): File path(s) to validate

**Returns:** Promise<ValidationResults>

**Example:**

```javascript
const validation = await doctest.validate('docs/');

if (validation.isValid) {
  console.log('Documentation is valid!');
} else {
  console.error('Validation errors:', validation.errors);
}
```

##### watch()

Watches documentation files for changes and runs tests automatically.

```javascript
doctest.watch(files, options)
```

**Parameters:**

- `files` (string|Array): File path(s) to watch
- `options` (Object): Watch options
  - `interval` (number): Polling interval in ms
  - `ignoreInitial` (boolean): Ignore initial file scan

**Returns:** Watcher

**Example:**

```javascript
const watcher = doctest.watch('docs/**/*.md', {
  interval: 1000
});

watcher.on('change', (file) => {
  console.log(`File changed: ${file}`);
});
```

## Testing API

### TestRunner

Handles test execution.

#### Methods

##### execute()

Executes a single test.

```javascript
TestRunner.execute(test, context)
```

**Parameters:**

- `test` (Test): Test object to execute
- `context` (Object): Execution context

**Returns:** Promise<TestResult>

**Example:**

```javascript
const { TestRunner } = require('doctest');

const result = await TestRunner.execute({
  code: 'console.log("Hello");',
  language: 'javascript'
}, {});
```

### Test Class

Represents a single test.

```javascript
new Test(code, language, metadata)
```

**Properties:**

- `code` (string): Test code
- `language` (string): Programming language
- `metadata` (Object): Additional metadata
- `status` (string): Test status (pending|running|passed|failed)

**Example:**

```javascript
const { Test } = require('doctest');

const test = new Test(
  'const x = 5; assert(x === 5);',
  'javascript',
  { line: 42, file: 'example.md' }
);
```

## Configuration API

### Config Class

Manages configuration.

#### Methods

##### load()

Loads configuration from file.

```javascript
Config.load(path)
```

**Parameters:**

- `path` (string): Path to config file

**Returns:** Config

**Example:**

```javascript
const { Config } = require('doctest');

const config = Config.load('./doctest.config.js');
console.log(config.get('debug'));
```

##### get()

Gets a configuration value.

```javascript
config.get(key, defaultValue)
```

**Parameters:**

- `key` (string): Configuration key
- `defaultValue` (any): Default value if key not found

**Returns:** any

**Example:**

```javascript
const timeout = config.get('timeout', 5000);
const debug = config.get('debug', false);
```

##### set()

Sets a configuration value.

```javascript
config.set(key, value)
```

**Parameters:**

- `key` (string): Configuration key
- `value` (any): Value to set

**Example:**

```javascript
config.set('debug', true);
config.set('timeout', 10000);
```

## Utilities API

### Parser

Parses documentation files.

#### Methods

##### parse()

Parses a documentation file.

```javascript
Parser.parse(content, options)
```

**Parameters:**

- `content` (string): File content
- `options` (Object): Parser options
  - `language` (string): Default language
  - `extractMetadata` (boolean): Extract metadata

**Returns:** ParseResult

**Example:**

```javascript
const { Parser } = require('doctest');

const result = Parser.parse(markdownContent, {
  language: 'javascript',
  extractMetadata: true
});

console.log(`Found ${result.codeBlocks.length} code blocks`);
```

### Reporter

Generates test reports.

#### Methods

##### generate()

Generates a test report.

```javascript
Reporter.generate(results, format)
```

**Parameters:**

- `results` (TestResults): Test results
- `format` (string): Output format (json|html|text)

**Returns:** string

**Example:**

```javascript
const { Reporter } = require('doctest');

const report = Reporter.generate(testResults, 'html');
fs.writeFileSync('report.html', report);
```

## Events API

### Event Emitter

DocTest extends EventEmitter and emits various events.

#### Events

##### test:start

Emitted when a test starts.

```javascript
doctest.on('test:start', (test) => {
  console.log(`Starting test: ${test.name}`);
});
```

##### test:pass

Emitted when a test passes.

```javascript
doctest.on('test:pass', (test, result) => {
  console.log(`Test passed: ${test.name}`);
});
```

##### test:fail

Emitted when a test fails.

```javascript
doctest.on('test:fail', (test, error) => {
  console.error(`Test failed: ${test.name}`, error);
});
```

##### run:complete

Emitted when all tests complete.

```javascript
doctest.on('run:complete', (results) => {
  console.log(`Completed: ${results.total} tests`);
  console.log(`Passed: ${results.passed}`);
  console.log(`Failed: ${results.failed}`);
});
```

## Type Definitions

### TestResults

```typescript
interface TestResults {
  total: number;
  passed: number;
  failed: number;
  skipped: number;
  duration: number;
  tests: TestResult[];
}
```

### TestResult

```typescript
interface TestResult {
  name: string;
  status: 'passed' | 'failed' | 'skipped';
  duration: number;
  error?: Error;
  output?: string;
}
```

### ValidationResults

```typescript
interface ValidationResults {
  isValid: boolean;
  errors: ValidationError[];
  warnings: ValidationWarning[];
}
```

## Error Handling

DocTest throws specific error types:

### TestError

Thrown when a test fails.

```javascript
try {
  await doctest.run('docs/');
} catch (error) {
  if (error instanceof TestError) {
    console.error('Test failed:', error.message);
    console.error('At line:', error.line);
  }
}
```

### ConfigError

Thrown for configuration errors.

```javascript
try {
  Config.load('invalid-config.js');
} catch (error) {
  if (error instanceof ConfigError) {
    console.error('Config error:', error.message);
  }
}
```

### ParserError

Thrown for parsing errors.

```javascript
try {
  Parser.parse(invalidMarkdown);
} catch (error) {
  if (error instanceof ParserError) {
    console.error('Parser error:', error.message);
  }
}
```

## CLI API

DocTest also provides a command-line interface:

```bash
# Run tests
doctest run [files]

# Validate documentation
doctest validate [files]

# Watch mode
doctest watch [files]

# Generate report
doctest report [files] --format html
```

### CLI Options

```
Options:
  -v, --version        Output version number
  -d, --debug          Enable debug mode
  -c, --config <path>  Config file path
  -f, --format <type>  Output format (json|html|text)
  -w, --watch          Watch mode
  -h, --help           Display help
```

## See Also

- [Examples](examples.md) - Practical code examples
- [User Guide](../guides/user-guide.md) - User guide
- [Getting Started](../getting-started.md) - Quick start guide

---

*Last updated: December 2025*
