# cpclip

> A professional CLI tool to copy files and folders to clipboard with MIME type preservation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)]()

## Features

- 📋 Copy file contents to clipboard
- 📁 Copy entire folders to clipboard
- 🎨 Preserves MIME types (images, text, binary)
- 🖥️ Cross-platform (macOS, Linux)
- ⚡ Ultra-simple installation and usage
- 🔧 Zero configuration needed

## Installation

### Homebrew (macOS/Linux)
```bash
brew tap username/cpclip
brew install cpclip
```

### APT (Debian/Ubuntu)
```bash
# Direct installation
wget https://github.com/username/cpclip/releases/download/v1.0.0/cpclip_1.0.0_all.deb
sudo dpkg -i cpclip_1.0.0_all.deb
sudo apt-get install -f
```

### Curl Installer
```bash
curl -sSL https://raw.githubusercontent.com/username/cpclip/main/install.sh | bash
```

### Manual Installation
```bash
git clone https://github.com/username/cpclip.git
cd cpclip
./install.sh
```

## Quick Start

```bash
# Copy a file
cpclip document.txt

# Copy an image
cpclip screenshot.png

# Copy a folder
cpclip ~/projects/myapp

# Copy current directory
cpclip .
```

## Usage

### Basic Commands
```bash
cpclip <file>              # Copy file to clipboard
cpclip <folder>            # Copy folder to clipboard
cpclip --help              # Show help
cpclip --version           # Show version
```

### Advanced Options
```bash
cpclip --text <folder>     # Copy folder as text tree
cpclip --archive <folder>  # Copy folder as tar.gz
cpclip --list <folder>     # Copy file paths only
cpclip --mime <file>       # Show MIME type (dry-run)
```

## Platform Support

| Platform | Status | Clipboard Tool |
|----------|--------|----------------|
| macOS    | ✅ Full | pbcopy, osascript |
| Linux (X11) | ✅ Full | xclip, xsel |
| Linux (Wayland) | ✅ Full | wl-copy |
| WSL | ⚠️ Basic | clip.exe |

## Requirements

### macOS
- Built-in tools (no additional requirements)

### Linux (Debian/Ubuntu)
- `xclip` or `xsel` or `wl-clipboard` (auto-installed via APT)
- `file` command (usually pre-installed)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

Nabeel Javed<nabeel.javed@gmail.com>

<!-- ## Support

- 🐛 Report bugs: [GitHub Issues](https://github.com/username/cpclip/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/username/cpclip/discussions) -->
