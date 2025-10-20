# Project Plan: **cpclip** - Professional CLI Clipboard Tool

## Tool Name: **cpclip**
**Rationale:** Short (6 chars), clear purpose, no namespace conflicts, memorable

---

## Architecture Overview

**Type:** Pure shell script (bash/zsh compatible)
- Zero compilation needed
- Transparent and auditable
- Universal compatibility
- Instant installation

**Core Components:**
- Main script: `cpclip` (~400-500 lines)
- Installation: `install.sh` + `uninstall.sh`
- Distribution: Homebrew tap, APT package, curl installer, manual git

---

## Feature Specification

### V1.0 MVP Features

**Basic Usage:**
```bash
cpclip file.txt              # Copy file content
cpclip image.png             # Copy with MIME type
cpclip folder/               # Copy folder (native refs)
cpclip .                     # Copy current directory
```

**Flags:**
```bash
-h, --help                   # Usage help
-v, --version                # Show version
-t, --text                   # Folder as text tree
-a, --archive                # Folder as tar.gz
-l, --list                   # File paths only
-m, --mime                   # Show MIME type (dry-run)
```

**Platform Support:**
- macOS: `pbcopy` + `osascript` for rich MIME
- Linux: `xclip`/`xsel`/`wl-copy` with MIME types
- Debian/Ubuntu: APT package with automatic dependency installation
- WSL: Basic support via `clip.exe`

**MIME Handling:**
- Auto-detect via `file --mime-type`
- Preserve MIME on clipboard where supported
- Smart fallback for limited platforms

---

## Installation Methods

### 1. APT (Debian/Ubuntu)
```bash
# Add repository (if using PPA)
sudo add-apt-repository ppa:username/cpclip
sudo apt update
sudo apt install cpclip

# Or direct .deb installation
wget https://github.com/username/cpclip/releases/download/v1.0.0/cpclip_1.0.0_all.deb
sudo dpkg -i cpclip_1.0.0_all.deb
sudo apt-get install -f  # Install dependencies
```

### 2. Homebrew (macOS/Linux)
```bash
brew tap username/cpclip
brew install cpclip
```

### 3. Curl Installer
```bash
curl -sSL https://raw.githubusercontent.com/username/cpclip/main/install.sh | bash
```

### 4. Manual
```bash
git clone https://github.com/username/cpclip.git
cd cpclip
./install.sh
```

---

## Repository Structure

```
cpclip/
├── cpclip                    # Main executable
├── install.sh                # Auto-installer
├── uninstall.sh              # Clean removal
├── README.md                 # Documentation
├── LICENSE                   # MIT License
├── CHANGELOG.md              # Version history
├── .github/
│   └── workflows/
│       ├── release.yml       # Auto-release on tag
│       └── test.yml          # CI testing
├── debian/
│   ├── control               # Package metadata
│   ├── changelog             # Debian changelog
│   ├── compat                # Debhelper compatibility
│   ├── copyright             # License info
│   ├── install               # Installation rules
│   ├── postinst              # Post-install script
│   └── rules                 # Build rules
├── homebrew/
│   └── cpclip.rb             # Homebrew formula
├── completions/
│   ├── cpclip.bash           # Bash completion
│   └── cpclip.zsh            # Zsh completion
├── man/
│   └── cpclip.1              # Man page
└── tests/
    ├── test.sh               # Test suite
    └── fixtures/             # Test files
```

---

## Technical Implementation Plan

### Phase 1: Core Script (Days 1-2)
1. OS detection (`uname`)
2. Clipboard tool finder (pbcopy/xclip/xsel/wl-copy)
3. MIME type detection (`file --mime-type`)
4. File copying logic
5. Folder handling (native file refs on macOS, list on Linux)
6. Error handling with helpful messages
7. Help/version output

### Phase 2: Distribution (Days 3-5)
1. `install.sh` - copies to `/usr/local/bin`, sets permissions
2. `uninstall.sh` - clean removal
3. Debian package creation:
   - `debian/control` with dependencies (xclip, file)
   - `debian/rules` for build process
   - `debian/postinst` for post-installation setup
   - Test .deb package on Ubuntu 20.04, 22.04, 24.04
4. Homebrew formula creation
5. GitHub Actions for automated releases (.deb + source)
6. Version tagging system

### Phase 3: Polish (Days 6-7)
1. Advanced folder modes (--text tree, --archive)
2. Enhanced MIME preservation (osascript/xclip -t)
3. Progress indicators for large files
4. Shell completions (bash/zsh)
5. Man page generation
6. Comprehensive testing

### Phase 4: Testing & Documentation (Days 8-9)
1. Test suite covering edge cases
2. Multi-platform CI testing (Ubuntu, Debian, macOS)
3. README with GIF demos
4. Installation verification across all methods
5. Public release

---

## Debian/Ubuntu APT Packaging Details

### Package Metadata (debian/control)
```
Source: cpclip
Section: utils
Priority: optional
Maintainer: Your Name <email@example.com>
Build-Depends: debhelper (>= 10)
Standards-Version: 4.5.0
Homepage: https://github.com/username/cpclip

Package: cpclip
Architecture: all
Depends: ${misc:Depends}, xclip | xsel | wl-clipboard, file
Recommends: xclip
Description: Professional CLI tool to copy files/folders to clipboard
 cpclip is a simple yet powerful command-line tool that copies file
 contents to the system clipboard while preserving MIME types.
 It supports text files, images, binary files, and entire folders.
 Works seamlessly on Linux with xclip/xsel/wl-copy.
```

### Build Process
```bash
# Build .deb package
dpkg-buildpackage -us -uc -b

# Test installation
sudo dpkg -i ../cpclip_1.0.0_all.deb

# Check dependencies
sudo apt-get install -f
```

### GitHub Actions Integration
- Auto-build .deb on release tag push
- Upload .deb as release asset
- Generate SHA256 checksums
- Test installation on multiple Ubuntu versions

### Distribution Options

**Option 1: Direct .deb Downloads (Simpler)**
- Host .deb files in GitHub Releases
- Users download and install with dpkg
- Update via new downloads

**Option 2: PPA (More Professional)**
- Create Launchpad PPA
- Users add repository once
- Automatic updates via apt
- Requires Launchpad account and GPG signing

**Recommended:** Start with Option 1, migrate to Option 2 post-launch

---

## Key Technical Decisions

### Clipboard Strategy by OS

**macOS:**
- Text: `pbcopy < file`
- Images: `osascript` for UTI preservation
- Folders: File URLs for Finder paste

**Linux (Debian/Ubuntu):**
- Primary: `xclip -selection clipboard -t <mime> -i file`
- Fallback: `xsel --clipboard --input < file`
- Wayland: `wl-copy --type <mime> < file`
- Folders: `file://` URIs list
- Auto-install prompt if tools missing

**Error Handling:**
- Pre-flight checks for clipboard tools
- Installation instructions specific to OS/distro
- Large file warnings (>100MB)
- Permission validation
- Helpful suggestions for common issues

---

## Dependency Management

### Runtime Dependencies

**macOS:**
- `pbcopy` (built-in)
- `osascript` (built-in)
- `file` (usually pre-installed)

**Debian/Ubuntu:**
- `xclip` (primary, auto-installed via APT)
- `xsel` (fallback)
- `wl-clipboard` (Wayland support)
- `file` (MIME detection)

**Dependency Declaration in debian/control:**
```
Depends: xclip | xsel | wl-clipboard, file
Recommends: xclip
Suggests: tree
```

---

## Success Criteria

✅ **Installation:** < 30 seconds, zero config
✅ **Compatibility:** macOS + Linux (bash/zsh)
✅ **APT Support:** Full Debian/Ubuntu integration with dependencies
✅ **UX:** Intuitive, no flags needed for basic use
✅ **Reliability:** Proper MIME type handling
✅ **Distribution:** APT + Homebrew + curl installer working
✅ **Documentation:** Clear README + man page
✅ **Testing:** 90%+ coverage of core paths

---

## Future Enhancements (V1.1+)

- Multiple file support: `cpclip file1 file2 file3`
- Clipboard history stack
- RTF/HTML rich formats
- Better progress indicators
- Fedora/RHEL RPM packaging
- Arch Linux AUR package
- Windows native support (beyond WSL)
- Shell integration (right-click context menu)

---

## Release Checklist

### Pre-Release
- [ ] Version bump in script header
- [ ] Update CHANGELOG.md
- [ ] Update debian/changelog
- [ ] Test on Ubuntu 20.04, 22.04, 24.04
- [ ] Test on Debian 11, 12
- [ ] Test on macOS (Intel + ARM)
- [ ] Verify all installation methods
- [ ] Update documentation

### Release Process
- [ ] Create git tag (v1.0.0)
- [ ] GitHub Actions builds .deb package
- [ ] Verify .deb installation
- [ ] Update Homebrew formula
- [ ] Create GitHub Release with notes
- [ ] Announce on social media

### Post-Release
- [ ] Monitor issue tracker
- [ ] Update documentation based on feedback
- [ ] Plan next version features

---

**Ready to implement:** This plan delivers a professional, production-ready CLI tool that's genuinely useful and easy to install across all major platforms.
