# Complete Guide: Running py-xiaozhi in VSCode

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [System Requirements](#system-requirements)
3. [Installation and Setup](#installation-and-setup)
4. [Configuring VSCode](#configuring-vscode)
5. [Running the Project](#running-the-project)
6. [Debugging](#debugging)
7. [Common Tasks](#common-tasks)
8. [Troubleshooting](#troubleshooting)

---

## 🎯 Project Overview

### What is py-xiaozhi?

**py-xiaozhi** is a Python-based voice client that implements an AI assistant with advanced voice interaction capabilities. It's a port of the [xiaozhi-esp32](https://github.com/78/xiaozhi-esp32) project for desktop computers, allowing you to experience AI features without specific hardware requirements.

### Key Features

#### 🤖 AI Capabilities
- **Voice Interaction**: Complete voice recognition and synthesis system
- **Computer Vision**: Image recognition and analysis (multimodal)
- **Smart Wake-up**: Keyword detection to activate the assistant
- **Continuous Dialogue**: Natural and fluid conversations with the assistant

#### 🛠️ MCP Tools Ecosystem
- **System Control**: Monitoring, app management, volume control
- **Calendar**: Complete event management and reminders
- **Timers**: Countdown timers and scheduled task execution
- **Music**: Online music search and playback with local cache
- **Web Search**: Bing integration for smart searches
- **Recipes**: Recipe database with search and recommendations
- **Maps**: Geolocation, routes, and weather via Amap services
- **Camera**: Image capture and AI analysis

#### 🏠 IoT Integration
- **Device Control**: Unified architecture to manage IoT devices
- **Smart Home**: Control lights, temperature sensors, etc.
- **Real-time Sync**: Device state monitoring

#### 🎵 Advanced Audio Processing
- **Codecs**: Opus support and real-time resampling
- **Voice Activity Detection (VAD)**: Intelligent interruption
- **Echo Cancellation**: WebRTC module for high-quality audio
- **System Audio Recording**: Loopback audio capture

### Technical Architecture

The project uses an event-driven architecture based on `asyncio`:

```
┌─────────────────────────────────────────────────┐
│           Application (Main Layer)              │
├─────────────────────────────────────────────────┤
│  Protocol Layer    │  Audio Processing Layer   │
│  (WebSocket/MQTT)  │  (Opus, VAD, AEC)        │
├─────────────────────────────────────────────────┤
│  MCP Tools Layer   │  IoT Device Layer         │
│  (System, Music)   │  (Thing Manager)          │
├─────────────────────────────────────────────────┤
│  Display Layer (PyQt5 GUI / CLI)               │
└─────────────────────────────────────────────────┘
```

**Main Components**:
- `main.py`: Application entry point
- `src/application.py`: Core application logic
- `src/protocols/`: WebSocket/MQTT communication
- `src/audio_codecs/`: Audio processing
- `src/mcp/`: Modular tools system
- `src/views/`: PyQt5 graphical interface

---

## 💻 System Requirements

### Basic Requirements

- **Python**: 3.9 - 3.12 (recommended: 3.10)
- **Operating System**: 
  - Windows 10+
  - macOS 10.15+
  - Linux (Ubuntu 20.04+, Debian, etc.)
- **RAM**: Minimum 4GB (recommended: 8GB+)
- **Disk Space**: 2GB free
- **Audio Devices**: Working microphone and speakers
- **Internet Connection**: Required for AI services

### Optional Feature Requirements

- **Voice Wake-up**: Sherpa-ONNX models (~500MB)
- **Camera**: Camera device + OpenCV
- **API Keys**: Zhipu API key for computer vision (optional)

---

## 🚀 Installation and Setup

### Step 1: Install System Dependencies

#### Windows

```powershell
# Install Scoop (package manager)
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex

# Install dependencies
scoop install git python ffmpeg
```

#### macOS

```bash
# Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install dependencies
brew install python@3.10 portaudio opus ffmpeg git
xcode-select --install
```

#### Linux (Ubuntu/Debian)

```bash
# Update repositories
sudo apt-get update

# Install main dependencies
sudo apt-get install -y \
    python3.10 python3.10-venv python3-pip \
    portaudio19-dev libportaudio2 \
    ffmpeg libopus0 libopus-dev \
    build-essential libasound2-dev \
    libxcb-xinerama0 libxkbcommon-x11-0 \
    pulseaudio-utils

# For development
sudo apt-get install -y gcc g++ make cmake pkg-config
```

### Step 2: Clone the Repository

```bash
# Clone the project
git clone https://github.com/huangjunsen0406/py-xiaozhi.git
cd py-xiaozhi
```

### Step 3: Setup Python Environment

#### Option A: Using Miniconda (Recommended)

```bash
# Download and install Miniconda
# Windows: https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe
# macOS (Intel): wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
# macOS (Apple Silicon): wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
# Linux: wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# For Linux/macOS, run:
chmod +x Miniconda3-latest-*.sh
./Miniconda3-latest-*.sh

# Create virtual environment
conda create -n py-xiaozhi python=3.10 -y
conda activate py-xiaozhi

# Install PyQt5 via conda (recommended to avoid issues)
conda install -c conda-forge pyqt=5.15 -y
```

#### Option B: Using venv

```bash
# Create virtual environment
python3 -m venv .venv

# Activate virtual environment
# Windows:
.venv\Scripts\activate
# Linux/macOS:
source .venv/bin/activate
```

### Step 4: Install Python Dependencies

```bash
# Update pip
pip install --upgrade pip

# Install dependencies
# Linux/Windows:
pip install -r requirements.txt

# macOS:
pip install -r requirements_mac.txt
```

### Step 5: Verify Installation

```bash
# Test main imports
python -c "import sounddevice; print('✓ SoundDevice OK')"
python -c "import opuslib; print('✓ Opus OK')"
python -c "import PyQt5.QtCore as q; print('✓ PyQt5 OK, Qt version:', q.QT_VERSION_STR)"
python -c "import websockets; print('✓ WebSockets OK')"
python -c "import asyncio; print('✓ Asyncio OK')"
```

If all tests pass, installation is complete! ✅

---

## 🔧 Configuring VSCode

### Step 1: Install Visual Studio Code

Download and install VSCode from: https://code.visualstudio.com/

### Step 2: Install Recommended Extensions

Open VSCode and install the following extensions:

1. **Python** (ms-python.python) - Essential
2. **Pylance** (ms-python.vscode-pylance) - Enhanced IntelliSense
3. **Python Debugger** (ms-python.debugpy) - Debugging
4. **Black Formatter** (ms-python.black-formatter) - Code formatting
5. **autoDocstring** (njpwerner.autodocstring) - Docstring generation
6. **GitLens** (eamodio.gitlens) - Advanced Git tools (optional)
7. **Error Lens** (usernamehw.errorlens) - Error visualization (optional)

To install quickly, run in VSCode terminal:

```bash
code --install-extension ms-python.python
code --install-extension ms-python.vscode-pylance
code --install-extension ms-python.debugpy
code --install-extension ms-python.black-formatter
code --install-extension njpwerner.autodocstring
```

### Step 3: Open Project in VSCode

```bash
# In project folder
code .
```

### Step 4: Select Python Interpreter

1. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS)
2. Type "Python: Select Interpreter"
3. Select the environment you created:
   - If using conda: `conda: py-xiaozhi`
   - If using venv: `.venv` in project folder

### Step 5: Configure launch.json for Debugging

The project now includes configurations in `.vscode/launch.json`. You can customize them as needed. The file includes:

- **GUI Mode (WebSocket)**: Standard GUI mode with WebSocket protocol
- **CLI Mode**: Command-line interface mode
- **MQTT Protocol**: GUI mode with MQTT protocol
- **Skip Activation**: Debug mode that bypasses device activation
- **Camera Test**: Test camera functionality
- **Audio Test**: Test audio devices

### Step 6: Configure tasks.json for Common Tasks

The project includes `.vscode/tasks.json` with useful tasks:

- **Run tasks**: GUI/CLI modes, different protocols
- **Format code**: Black formatting
- **Lint code**: Flake8 checking
- **Test utilities**: Camera, audio, music cache
- **Install dependencies**: For different platforms
- **Verify installation**: Check all imports work

Access tasks with `Ctrl+Shift+P` > "Tasks: Run Task"

### Step 7: Editor Settings

Settings are already in `.vscode/settings.json`:

- **Auto formatting**: Code formatted with Black on save
- **Import organization**: Imports organized automatically
- **Linting**: Code checking with Flake8
- **Docstrings**: Google format for documentation

---

## ▶️ Running the Project

### Method 1: Via VSCode Integrated Terminal

1. Open terminal in VSCode: `` Ctrl+` `` (or View > Terminal)
2. Make sure virtual environment is activated
3. Run:

```bash
# GUI mode (default)
python main.py

# CLI mode
python main.py --mode cli

# With MQTT protocol
python main.py --protocol mqtt

# Skip activation (for debugging)
python main.py --skip-activation
```

### Method 2: Using VSCode Tasks

1. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS)
2. Type "Tasks: Run Task"
3. Select desired task:
   - "Run: GUI Mode (WebSocket)"
   - "Run: CLI Mode (WebSocket)"
   - "Run: GUI Mode (MQTT)"

### Method 3: Using the Play Button

1. Open `main.py` file
2. Click the ▶️ (Play) button in the top right corner
3. Or press `F5` to start with debugging

### First Run: Device Activation

On first run, you'll need to activate the device:

1. The program will open an activation window
2. A URL will be displayed (e.g., `https://xiaozhi.me/`)
3. Open the URL in your browser
4. Scan the QR code or enter the code manually
5. Complete activation on the website
6. The program will automatically detect activation

**Note**: Activation is only needed once. Credentials are saved in `config/efuse.json`.

---

## 🐛 Debugging

### Using VSCode Debugger

1. **Set Breakpoints**:
   - Click to the left of the line number in code
   - A red dot will appear

2. **Start Debugging**:
   - Press `F5`
   - Or go to "Run > Start Debugging"
   - Select desired configuration (GUI, CLI, etc.)

3. **Debug Controls**:
   - `F5`: Continue
   - `F10`: Step Over (next line)
   - `F11`: Step Into (enter function)
   - `Shift+F11`: Step Out (exit function)
   - `Ctrl+Shift+F5`: Restart
   - `Shift+F5`: Stop

4. **Inspect Variables**:
   - "Variables" panel shows all variables in scope
   - Hover over variables in code
   - Use "Debug Console" to evaluate expressions

### Debug Console

During debugging, you can evaluate Python expressions in Debug Console:

```python
# Check variable value
print(config.get_config("SYSTEM_OPTIONS.CLIENT_ID"))

# Call functions
await some_async_function()

# Inspect objects
dir(app)
```

### Logging

The project uses extensive logging. Logs are saved to:
- Console: Colored output
- File: `logs/xiaozhi.log` (if configured)

To adjust log level, edit `src/utils/logging_config.py`:

```python
# Levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
setup_logging(level="DEBUG")
```

---

## 📝 Common Tasks

### Format Code

```bash
# Via terminal
python -m black src/ main.py

# Via VSCode
# Ctrl+Shift+P > "Format Document"
# Or save file (auto-format on save)
```

### Check Code Style

```bash
# Via terminal
python -m flake8 src/ main.py

# Via VSCode Task
# Ctrl+Shift+P > "Tasks: Run Task" > "Lint: Flake8 (Check All)"
```

### Test Camera

```bash
# Via terminal
python scripts/camera_scanner.py

# Via VSCode Task
# Ctrl+Shift+P > "Tasks: Run Task" > "Test: Camera Scanner"
```

### Generate Directory Tree

```bash
python scripts/dir_tree.py
```

### Check Opus

```bash
bash checke_opus.sh
```

### Update Dependencies

```bash
# Update pip
pip install --upgrade pip

# Reinstall requirements
pip install -r requirements.txt --upgrade

# For macOS
pip install -r requirements_mac.txt --upgrade
```

---

## 🔍 Troubleshooting

### Problem 1: "ModuleNotFoundError"

**Error**: `ModuleNotFoundError: No module named 'xxx'`

**Solution**:
```bash
# Check if virtual environment is activated
# Conda:
conda activate py-xiaozhi

# venv:
source .venv/bin/activate  # Linux/macOS
.venv\Scripts\activate     # Windows

# Reinstall dependencies
pip install -r requirements.txt
```

### Problem 2: PyQt5 not working

**Error**: GUI issues

**Linux Solution**:
```bash
# Install system dependencies
sudo apt-get install -y libxcb-xinerama0 libxkbcommon-x11-0

# Reinstall PyQt5
pip uninstall PyQt5 PyQt5-sip PyQt5-Qt5 -y
conda install -c conda-forge pyqt=5.15 -y
```

**macOS Solution**:
```bash
# Use conda for PyQt5
conda install -c conda-forge pyqt=5.15
```

### Problem 3: Audio Error (SoundDevice)

**Error**: `PortAudioError` or microphone issues

**Windows Solution**:
```powershell
# Reinstall sounddevice
pip uninstall sounddevice -y
pip install sounddevice
```

**Linux Solution**:
```bash
# Check PulseAudio
pulseaudio --check
pulseaudio --start

# Add user to audio group
sudo usermod -a -G audio $USER
# Logout and login again

# Reinstall audio dependencies
sudo apt-get install -y portaudio19-dev libportaudio2 pulseaudio-utils
pip install sounddevice --force-reinstall
```

**macOS Solution**:
```bash
# Reinstall portaudio
brew reinstall portaudio
pip install sounddevice --force-reinstall
```

### Problem 4: Activation Fails

**Error**: Cannot activate device

**Solution**:
```bash
# Clear activation configuration
rm config/efuse.json

# Run again
python main.py
```

### Problem 5: VSCode Cannot Find Python Interpreter

**Solution**:
1. Press `Ctrl+Shift+P`
2. Type "Python: Select Interpreter"
3. If environment doesn't appear:
   - For conda: `which python` in activated conda terminal
   - For venv: Navigate to `.venv/bin/python` (Linux/macOS) or `.venv\Scripts\python.exe` (Windows)
4. Paste full path in "Enter interpreter path"

### Problem 6: Echo or Poor Audio

**Solution**:

Edit `config/config.json`:

```json
{
  "AEC_OPTIONS": {
    "ENABLED": true,
    "FILTER_LENGTH_RATIO": 0.6,
    "BUFFER_MAX_LENGTH": 300,
    "ENABLE_PREPROCESS": true
  }
}
```

Adjust `FILTER_LENGTH_RATIO`:
- Strong echo: increase to 0.6-0.8
- Choppy audio: decrease to 0.2-0.4

### Problem 7: Debugging Not Working

**Solution**:

1. Check that Python Debugger extension is installed
2. Check that correct interpreter is selected
3. Try creating new `launch.json`:
   - Delete `.vscode/launch.json`
   - Press `F5`
   - Select "Python Debugger" > "Python File"

### Problem 8: Opus Library Not Found

**Error**: `Could not find opus library`

**Windows Solution**:
```powershell
# DLL is already included in project at libs/windows/
# Copy to system directory if needed
copy libs\windows\opus.dll C:\Windows\System32\
```

**macOS Solution**:
```bash
# Install via conda (recommended)
conda install -c conda-forge opus

# Or via Homebrew
brew install opus

# Configure DYLD_LIBRARY_PATH if needed
echo 'export DYLD_LIBRARY_PATH=/opt/homebrew/lib:$DYLD_LIBRARY_PATH' >> ~/.zshrc
source ~/.zshrc
```

**Linux Solution**:
```bash
sudo apt-get install -y libopus0 libopus-dev
```

---

## 📚 Additional Resources

### Complete Documentation

- **Official Site**: https://huangjunsen0406.github.io/py-xiaozhi/
- **Configuration Guide**: `documents/docs/guide/配置说明.md`
- **Dependency Installation**: `documents/docs/guide/系统依赖安装.md`
- **Device Activation**: `documents/docs/guide/设备激活流程.md`

### Video Tutorials

- [Complete Video Tutorial](https://www.bilibili.com/video/BV1dWQhYEEmq/)
- [Project Demo](https://www.bilibili.com/video/BV1HmPjeSED2/)

### Community and Support

- **GitHub Issues**: https://github.com/huangjunsen0406/py-xiaozhi/issues
- **Gitee**: https://gitee.com/huang-jun-sen/py-xiaozhi

---

## 🎓 VSCode Development Tips

### Useful Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+P` | Quick file open |
| `Ctrl+Shift+P` | Command palette |
| `Ctrl+` ` | Open/close terminal |
| `F5` | Start debugging |
| `F12` | Go to definition |
| `Shift+F12` | Find all references |
| `Ctrl+Shift+F` | Search in all files |
| `Ctrl+/` | Comment/uncomment line |

### Multi-Cursor

- `Alt+Click`: Add cursor
- `Ctrl+Alt+Up/Down`: Add cursor above/below
- `Ctrl+D`: Select next occurrence

### Zen Mode

Press `Ctrl+K Z` to enter zen mode (fullscreen without distractions)

---

## ✅ Setup Checklist

- [ ] Python 3.9-3.12 installed
- [ ] VSCode installed
- [ ] Python extension installed in VSCode
- [ ] System dependencies installed
- [ ] Repository cloned
- [ ] Virtual environment created and activated
- [ ] Python dependencies installed
- [ ] Python interpreter selected in VSCode
- [ ] `.vscode/launch.json` configured
- [ ] `.vscode/tasks.json` configured
- [ ] Import tests passed
- [ ] Audio device working
- [ ] First run and activation complete
- [ ] Program runs without errors

---

## 🎉 Conclusion

Congratulations! You've successfully set up py-xiaozhi in VSCode. Now you can:

✅ Run the project in GUI or CLI mode
✅ Debug code with breakpoints
✅ Auto-format and check code
✅ Use custom tasks
✅ Develop new features

**Next Steps**:
1. Explore the assistant's features
2. Customize settings in `config/config.json`
3. Add your own wake words
4. Configure additional MCP tools
5. Contribute to the project!

**Need Help?**
- Check the complete documentation
- Open an issue on GitHub
- Watch the video tutorials

Happy developing! 🚀
