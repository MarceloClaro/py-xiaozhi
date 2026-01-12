# VSCode Setup Documentation - What's New

## 📝 Overview

This update adds comprehensive Visual Studio Code setup and usage documentation for the py-xiaozhi project, making it easier for developers to get started with local development.

## 🆕 New Files Added

### 1. Complete Setup Guides

#### Portuguese Guide
- **File**: `GUIA_VSCODE_PT.md`
- **Purpose**: Complete setup and usage guide in Portuguese
- **Content**:
  - Detailed project overview and architecture
  - Step-by-step installation instructions
  - VSCode configuration guide
  - Debugging tutorial
  - Common tasks and troubleshooting
  - 849 lines of comprehensive documentation

#### English Guide
- **File**: `VSCODE_GUIDE_EN.md`
- **Purpose**: Complete setup and usage guide in English
- **Content**:
  - Full project explanation
  - Installation procedures
  - VSCode setup instructions
  - Debugging guide
  - Troubleshooting section
  - 713 lines of detailed documentation

### 2. VSCode Configuration Files

#### Debug Configurations (`launch.json`)
- **Location**: `.vscode/launch.json`
- **Configurations**:
  1. **GUI Mode (WebSocket)** - Standard GUI with WebSocket protocol
  2. **CLI Mode** - Command-line interface mode
  3. **GUI Mode (MQTT)** - GUI with MQTT protocol
  4. **Skip Activation (Debug)** - Bypass device activation for debugging
  5. **Test Camera Scanner** - Test camera functionality
  6. **Test Audio Scanner** - Test audio devices

#### Task Definitions (`tasks.json`)
- **Location**: `.vscode/tasks.json`
- **Task Categories**:
  - **Run Tasks**: GUI/CLI modes, different protocols
  - **Development Tasks**: Format code (Black), Lint (Flake8)
  - **Testing Tasks**: Camera, audio, music cache tests
  - **Installation Tasks**: Install dependencies for different platforms
  - **Utility Tasks**: Directory tree generation, cache cleanup

#### Extension Recommendations (`extensions.json`)
- **Location**: `.vscode/extensions.json`
- **Recommended Extensions**:
  - Python (ms-python.python)
  - Pylance (ms-python.vscode-pylance)
  - Python Debugger (ms-python.debugpy)
  - Black Formatter (ms-python.black-formatter)
  - Flake8 (ms-python.flake8)
  - autoDocstring (njpwerner.autodocstring)
  - GitLens (eamodio.gitlens)
  - Error Lens (usernamehw.errorlens)
  - IntelliCode (visualstudioexptteam.vscodeintellicode)
  - TODO Tree (gruntfuggly.todo-tree)
  - Better Comments (aaron-bond.better-comments)

### 3. Quick Reference

- **File**: `documents/docs/guide/VSCODE_QUICK_REFERENCE.md`
- **Purpose**: Quick reference card for common operations
- **Content**:
  - Trilingual (English, Portuguese, Chinese)
  - Keyboard shortcuts
  - Quick commands
  - Troubleshooting tips
  - 239 lines of quick reference

### 4. Updated README Files

- **Modified**: `README.md` and `README.en.md`
- **Changes**: Added quick start sections linking to new guides

## 🎯 How to Use These Guides

### For First-Time Users

1. **Read the appropriate guide**:
   - Portuguese speakers: Read `GUIA_VSCODE_PT.md`
   - English speakers: Read `VSCODE_GUIDE_EN.md`

2. **Follow the setup steps**:
   - Install system dependencies
   - Clone the repository
   - Set up Python environment
   - Install Python dependencies
   - Configure VSCode

3. **Start developing**:
   - Open project in VSCode
   - Select Python interpreter
   - Press F5 to run/debug

### For Quick Reference

- Keep `documents/docs/guide/VSCODE_QUICK_REFERENCE.md` handy
- Access tasks with `Ctrl+Shift+P` → "Tasks: Run Task"
- Use debug configurations with `F5`

## ✨ Key Features

### 1. One-Click Debugging
Press `F5` and select from multiple pre-configured debug scenarios:
- GUI mode with WebSocket
- CLI mode
- MQTT protocol
- Skip activation for testing
- Camera and audio tests

### 2. Task Automation
Run common tasks without leaving VSCode:
- Format code automatically
- Lint with one command
- Test hardware components
- Install dependencies
- Clean cache

### 3. Extension Management
VSCode will automatically prompt to install recommended extensions when you open the project.

### 4. Comprehensive Documentation
- **Beginner-friendly**: Step-by-step guides for complete setup
- **Multilingual**: Supports Portuguese, English, and Chinese
- **Troubleshooting**: Common issues and solutions included
- **Best practices**: Tips for efficient development

## 📋 What Each Guide Covers

### Complete Guides (GUIA_VSCODE_PT.md / VSCODE_GUIDE_EN.md)

1. **Project Overview**
   - What is py-xiaozhi
   - Key features
   - Technical architecture
   - Component structure

2. **System Requirements**
   - Python version
   - Operating systems
   - Hardware requirements
   - Optional features

3. **Installation**
   - System dependencies (Windows, macOS, Linux)
   - Python environment setup (Conda, venv)
   - Dependency installation
   - Verification steps

4. **VSCode Configuration**
   - Extension installation
   - Interpreter selection
   - Launch configuration
   - Tasks setup
   - Editor settings

5. **Running the Project**
   - Terminal commands
   - Task execution
   - Debug button usage
   - First-run activation

6. **Debugging**
   - Breakpoint usage
   - Debug controls
   - Variable inspection
   - Debug console

7. **Common Tasks**
   - Code formatting
   - Style checking
   - Testing utilities
   - Dependency updates

8. **Troubleshooting**
   - Module not found
   - PyQt5 issues
   - Audio problems
   - Activation failures
   - Platform-specific issues

### Quick Reference

- Essential keyboard shortcuts
- Common commands
- Task list
- Quick troubleshooting
- Documentation links

## 🌍 Language Support

- **Portuguese** (`GUIA_VSCODE_PT.md`): Complete guide for Portuguese speakers
- **English** (`VSCODE_GUIDE_EN.md`): Full guide for English speakers
- **Trilingual Quick Reference**: English, Portuguese, and Chinese references

## 🔗 Integration with Existing Documentation

The new guides complement existing documentation:
- Link to official documentation site
- Reference existing configuration guides
- Point to video tutorials
- Include community links

## 💡 Tips for Maintainers

### Updating the Guides

When project structure or dependencies change, update:
1. Installation sections in both guides
2. Configuration examples
3. Troubleshooting sections
4. Quick reference commands

### Adding New Features

When adding new features:
1. Update the project overview section
2. Add new debug configurations if needed
3. Create new tasks for feature testing
4. Update troubleshooting if new issues arise

### Version Updates

Keep guides synchronized with:
- Python version requirements
- Dependency versions
- VSCode extension versions
- Configuration changes

## 📈 Benefits

### For Users
- ✅ Faster setup time
- ✅ Reduced errors
- ✅ Better development experience
- ✅ Easy troubleshooting
- ✅ Professional workflow

### For Project
- ✅ Lower barrier to entry
- ✅ More contributors
- ✅ Better code quality
- ✅ Standardized setup
- ✅ Improved documentation

## 🎓 Learning Resources

Each guide includes:
- Step-by-step tutorials
- Code examples
- Configuration samples
- Best practices
- Video tutorial links
- Community resources

## 🔄 Future Enhancements

Potential additions:
- Docker setup guide
- CI/CD integration guide
- Testing framework documentation
- Contribution guidelines
- Code style guide

## 📞 Support

If users encounter issues not covered in the guides:
1. Check the quick reference
2. Review troubleshooting sections
3. Search existing GitHub issues
4. Create a new issue with details
5. Reference the appropriate guide

## ✅ Checklist for Users

Before starting development, users should:
- [ ] Read the appropriate language guide
- [ ] Install system dependencies
- [ ] Set up Python environment
- [ ] Install Python dependencies
- [ ] Configure VSCode with recommended extensions
- [ ] Select correct Python interpreter
- [ ] Test with "Verify Installation" task
- [ ] Run first-time activation
- [ ] Try running with F5
- [ ] Review quick reference

## 🎉 Conclusion

These comprehensive guides make py-xiaozhi development accessible to developers worldwide, providing clear instructions for setup, configuration, and development in VSCode. The multilingual support ensures that developers can work in their preferred language while maintaining a consistent development experience.

---

**Author**: GitHub Copilot
**Date**: 2026-01-12
**Purpose**: Documentation for VSCode setup and usage
**Languages**: Portuguese, English
**Total Documentation**: ~2,000 lines
