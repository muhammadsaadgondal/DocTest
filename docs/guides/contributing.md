# Contributing Guide

Thank you for your interest in contributing to DocTest! This guide will help you get started.

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Development Workflow](#development-workflow)
4. [Coding Standards](#coding-standards)
5. [Testing](#testing)
6. [Documentation](#documentation)
7. [Submitting Changes](#submitting-changes)

## Code of Conduct

We are committed to providing a welcoming and inclusive environment for all contributors.

### Our Standards

- Be respectful and considerate
- Welcome newcomers and help them learn
- Focus on constructive feedback
- Respect differing viewpoints and experiences

### Unacceptable Behavior

- Harassment or discriminatory comments
- Trolling or insulting remarks
- Personal or political attacks
- Publishing others' private information

## Getting Started

### Prerequisites

Before you begin, ensure you have:

- Git installed (version 2.0+)
- Node.js installed (version 14.0+)
- A GitHub account
- Basic knowledge of JavaScript/TypeScript

### Fork and Clone

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/DocTest.git
   cd DocTest
   ```

3. Add upstream remote:
   ```bash
   git remote add upstream https://github.com/muhammadsaadgondal/DocTest.git
   ```

### Install Dependencies

```bash
npm install
```

### Verify Setup

Run tests to ensure everything is working:

```bash
npm test
```

## Development Workflow

### 1. Create a Branch

Create a new branch for your work:

```bash
git checkout -b feature/your-feature-name
```

Branch naming conventions:
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation changes
- `refactor/` - Code refactoring
- `test/` - Test improvements

### 2. Make Your Changes

- Write clear, concise code
- Follow existing code style
- Add tests for new functionality
- Update documentation as needed

### 3. Commit Your Changes

Write clear commit messages:

```bash
git add .
git commit -m "feat: add new feature description"
```

Commit message format:
```
<type>: <subject>

<body>

<footer>
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

### 4. Keep Your Branch Updated

Regularly sync with upstream:

```bash
git fetch upstream
git rebase upstream/main
```

### 5. Push Your Changes

```bash
git push origin feature/your-feature-name
```

## Coding Standards

### JavaScript/TypeScript

- Use ES6+ features
- Prefer `const` over `let`, avoid `var`
- Use arrow functions for callbacks
- Add JSDoc comments for functions

Example:

```javascript
/**
 * Calculates the sum of two numbers
 * @param {number} a - First number
 * @param {number} b - Second number
 * @returns {number} The sum of a and b
 */
const add = (a, b) => a + b;
```

### Code Style

- Use 2 spaces for indentation
- Maximum line length: 100 characters
- Add semicolons
- Use single quotes for strings

Run the linter:

```bash
npm run lint
```

Fix style issues automatically:

```bash
npm run lint:fix
```

### File Organization

```
src/
├── core/           # Core functionality
├── utils/          # Utility functions
├── services/       # Business logic
├── api/            # API endpoints
└── tests/          # Test files
```

## Testing

### Writing Tests

- Write tests for all new features
- Maintain or improve code coverage
- Use descriptive test names

Example test:

```javascript
describe('add function', () => {
  it('should correctly add two positive numbers', () => {
    expect(add(2, 3)).toBe(5);
  });

  it('should correctly add negative numbers', () => {
    expect(add(-2, -3)).toBe(-5);
  });

  it('should handle zero', () => {
    expect(add(0, 5)).toBe(5);
  });
});
```

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm test -- --watch

# Run tests with coverage
npm test -- --coverage

# Run specific test file
npm test path/to/test.spec.js
```

### Test Coverage

Maintain at least 80% code coverage:

```bash
npm run test:coverage
```

## Documentation

### Code Documentation

- Add JSDoc comments for public APIs
- Include examples in documentation
- Keep comments up-to-date

### User Documentation

- Update relevant markdown files in `docs/`
- Add examples for new features
- Update the changelog

### Documentation Style

- Use clear, concise language
- Include code examples
- Add links to related topics
- Use proper markdown formatting

## Submitting Changes

### Pull Request Process

1. **Create a Pull Request**
   - Go to your fork on GitHub
   - Click "New Pull Request"
   - Select your branch

2. **PR Title and Description**
   - Use clear, descriptive title
   - Explain what changes you made
   - Reference related issues

Example PR description:
```markdown
## Changes

- Added new feature X *(replace with actual feature description)*
- Fixed bug Y *(replace with actual bug description)*
- Updated documentation

## Related Issues

Closes #123 *(replace with actual issue number)*

## Testing

- Added unit tests *(describe what you tested)*
- Tested manually on Chrome, Firefox *(or your testing environment)*
- All existing tests pass

## Screenshots

(If applicable)
```

3. **Code Review**
   - Address reviewer feedback
   - Update your branch as needed
   - Be responsive to comments

4. **Merge**
   - Once approved, your PR will be merged
   - Delete your branch after merge

### PR Checklist

Before submitting, ensure:

- [ ] Code follows style guidelines
- [ ] All tests pass
- [ ] New tests added for new features
- [ ] Documentation updated
- [ ] Commit messages are clear
- [ ] Branch is up-to-date with main
- [ ] No merge conflicts

## Community

### Getting Help

- **Questions**: Open a discussion on GitHub
- **Bugs**: Open an issue with reproduction steps
- **Features**: Open an issue to discuss first

### Communication Channels

- GitHub Issues: Bug reports and feature requests
- GitHub Discussions: Questions and general discussion
- Pull Requests: Code contributions

## Recognition

Contributors are recognized in:
- README.md contributors section
- Release notes
- GitHub contributors page

## License

By contributing to DocTest, you agree that your contributions will be licensed under the project's license.

---

Thank you for contributing to DocTest! 🎉

## Additional Resources

- [User Guide](user-guide.md)
- [API Reference](../api/api-reference.md)
- [Architecture Overview](../architecture.md)
- [Getting Started](../getting-started.md)

*Last updated: December 2025*
