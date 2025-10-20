# Contributing to cpclip

Thank you for considering contributing to cpclip! We welcome contributions from everyone.

## How to Contribute

### Reporting Bugs

If you find a bug, please create an issue with:
- Clear description of the problem
- Steps to reproduce
- Expected vs actual behavior
- Your OS and shell version
- cpclip version (`cpclip --version`)

### Suggesting Features

Feature suggestions are welcome! Please create an issue with:
- Clear description of the feature
- Use case and benefits
- Possible implementation approach (optional)

### Code Contributions

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
   - Follow the code style (see below)
   - Add tests if applicable
   - Update documentation

4. **Test your changes**
   ```bash
   cd tests
   ./test.sh
   ```

5. **Commit your changes**
   ```bash
   git commit -m "Add feature: description"
   ```

6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request**
   - Describe what changed and why
   - Reference any related issues
   - Ensure all tests pass

## Code Style

### Shell Script Guidelines
- Use `bash` compatible syntax
- Test with both `bash` and `zsh`
- Use 2 spaces for indentation
- Add comments for complex logic
- Use descriptive variable names
- Quote all variables: `"${variable}"`
- Use `[[ ]]` for conditionals (not `[ ]`)

### Example
```bash
#!/usr/bin/env bash

# Good
if [[ -f "${file_path}" ]]; then
  echo "File exists: ${file_path}"
fi

# Avoid
if [ -f $file_path ]
then
  echo "File exists: $file_path"
fi
```

## Testing

- Add tests for new features
- Ensure existing tests pass
- Test on multiple platforms when possible
- Include edge cases

## Development Setup

```bash
# Clone your fork
git clone https://github.com/yourusername/cpclip.git
cd cpclip

# Make the script executable
chmod +x cpclip

# Test locally
./cpclip --help
```

## Questions?

Feel free to open an issue for any questions or clarifications.

## Code of Conduct

Please be respectful and constructive. We're all here to learn and build something useful together.
