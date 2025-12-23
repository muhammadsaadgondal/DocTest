# Configuration

This document describes how to configure DocTest for your needs.

## Configuration File

DocTest uses a configuration file located at the root of your project. You can use either:

- `doctest.config.js` (JavaScript)
- `doctest.config.json` (JSON)
- `.doctestrc` (JSON or YAML)

## Basic Configuration

Here's a basic configuration example:

```javascript
module.exports = {
  // Project name
  name: 'DocTest',
  
  // Version
  version: '1.0.0',
  
  // Environment settings
  env: {
    development: {
      debug: true,
      port: 3000
    },
    production: {
      debug: false,
      port: 8080
    }
  }
};
```

## Configuration Options

### Core Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `name` | string | `'DocTest'` | Project name |
| `version` | string | `'1.0.0'` | Project version |
| `debug` | boolean | `false` | Enable debug mode |
| `port` | number | `3000` | Server port |

### Advanced Options

#### Logging

```javascript
{
  logging: {
    level: 'info', // 'error', 'warn', 'info', 'debug'
    format: 'json', // 'json' or 'text'
    output: 'console' // 'console' or 'file'
  }
}
```

#### Performance

```javascript
{
  performance: {
    cache: true,
    maxConcurrency: 10,
    timeout: 30000 // milliseconds
  }
}
```

## Environment Variables

You can also configure DocTest using environment variables:

```bash
# Development
export DOCTEST_ENV=development
export DOCTEST_DEBUG=true
export DOCTEST_PORT=3000

# Production
export DOCTEST_ENV=production
export DOCTEST_DEBUG=false
export DOCTEST_PORT=8080
```

## Best Practices

1. **Use environment-specific configs**: Keep separate configurations for development, staging, and production
2. **Never commit secrets**: Use environment variables for sensitive data
3. **Document custom options**: If you add custom configuration options, document them
4. **Validate configuration**: Always validate your configuration before deployment

## Examples

### Development Configuration

```javascript
module.exports = {
  name: 'DocTest',
  env: 'development',
  debug: true,
  port: 3000,
  logging: {
    level: 'debug'
  }
};
```

### Production Configuration

```javascript
module.exports = {
  name: 'DocTest',
  env: 'production',
  debug: false,
  port: 8080,
  logging: {
    level: 'error'
  },
  performance: {
    cache: true,
    maxConcurrency: 20
  }
};
```

## See Also

- [Getting Started](getting-started.md)
- [API Reference](api/api-reference.md)
