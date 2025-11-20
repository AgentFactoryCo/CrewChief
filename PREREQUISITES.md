# Prerequisites

CrewChief requires Python 3.11+ and optionally Foundry Local for AI features. This guide helps you check if you have them installed and how to install them if needed.

## Python 3.11+

### Check if Python is installed

```bash
python3 --version
```

You should see output like:
```
Python 3.11.x
```

If the version is 3.11 or higher, you're good to go!

### Install Python (if needed)

#### macOS

**Using Homebrew (recommended):**
```bash
brew install python@3.11
```

**Using official installer:**
1. Download from [python.org](https://www.python.org/downloads/)
2. Run the installer
3. Verify installation: `python3 --version`

#### Linux

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install python3.11 python3.11-venv python3-pip
```

**Fedora:**
```bash
sudo dnf install python3.11 python3-pip
```

**Arch Linux:**
```bash
sudo pacman -S python python-pip
```

#### Windows

1. Download the installer from [python.org](https://www.python.org/downloads/)
2. Run the installer
3. **Important:** Check "Add Python to PATH" during installation
4. Verify in Command Prompt or PowerShell: `python --version`

### Verify pip is installed

```bash
python3 -m pip --version
```

If pip is missing:
```bash
# Linux/macOS
python3 -m ensurepip --upgrade

# Windows
python -m ensurepip --upgrade
```

## Foundry Local (Optional)

Foundry Local is only required for AI-powered features (summary, suggest-maint, track-prep). All core garage management features work without it.

### Check if Foundry Local is installed

```bash
foundry --version
```

If installed, you'll see version information. If not, you'll get a "command not found" error.

### Install Foundry Local

Foundry Local is an LLM runtime that runs entirely on your machine.

**Visit:** [foundry.black](https://foundry.black)

Follow the installation instructions for your operating system:

1. Install Foundry using their recommended method
2. Download a compatible model (e.g., `phi-3.5-mini`)
3. Start the server:
   ```bash
   foundry serve phi-3.5-mini
   ```

### Verify Foundry is running

```bash
curl http://localhost:1234/v1/models
```

You should see JSON output listing available models. If you get a connection error, make sure Foundry is running with `foundry serve`.

### Recommended Models

For CrewChief, we recommend:

- **phi-3.5-mini** - Fast, efficient, good for most garage management tasks
- **llama-3.1-8b** - More powerful, better reasoning, higher resource usage
- **phi-3-medium** - Balanced performance and quality

Start Foundry with your chosen model:
```bash
foundry serve phi-3.5-mini
```

### Running without Foundry Local

If you don't have Foundry installed or it's not running:

- ✅ All garage management commands work: `add-car`, `list-cars`, `show-car`, `log-service`, `history`
- ❌ AI commands will show a friendly error: `summary`, `suggest-maint`, `track-prep`

You can disable LLM features entirely in your config:
```bash
CREWCHIEF_LLM_ENABLED=false
```

## Additional Dependencies

### Git (for cloning repository)

**Check if installed:**
```bash
git --version
```

**Install if needed:**

- **macOS:** `brew install git` or install Xcode Command Line Tools
- **Linux:** `sudo apt install git` (Ubuntu/Debian) or equivalent
- **Windows:** Download from [git-scm.com](https://git-scm.com/)

### Virtual Environment Support

Python's `venv` module is usually included with Python 3.11+. Verify:

```bash
python3 -m venv --help
```

If missing on Linux:
```bash
# Ubuntu/Debian
sudo apt install python3.11-venv
```

## Quick Verification Checklist

Before installing CrewChief, ensure you have:

- [ ] Python 3.11+ installed (`python3 --version`)
- [ ] pip working (`python3 -m pip --version`)
- [ ] venv support (`python3 -m venv --help`)
- [ ] Git installed (`git --version`)
- [ ] (Optional) Foundry Local running (`curl http://localhost:1234/v1/models`)

Once these are ready, proceed to [Installation](README.md#installation) in the README.

## Troubleshooting

### Python version is too old

If you have Python 3.10 or earlier, CrewChief may not work correctly. Install Python 3.11+ using the instructions above.

### Multiple Python versions

If you have multiple Python versions, you may need to specify the exact version:

```bash
python3.11 -m venv .venv
```

### Permission errors on Linux/macOS

If you get permission errors during installation:

```bash
# Don't use sudo with pip!
# Instead, use --user flag or virtual environments
pip install --user crewchief
```

Better yet, always use a virtual environment (recommended method in README).

### Foundry Local not responding

1. Check if Foundry is running: `ps aux | grep foundry`
2. Check the port: `lsof -i :1234` (macOS/Linux) or `netstat -ano | findstr :1234` (Windows)
3. Try restarting Foundry: `foundry serve phi-3.5-mini`
4. Check Foundry's logs for errors

### Windows-specific issues

- If `python3` doesn't work, try `python` (without the 3)
- If scripts aren't found, activate the virtual environment: `.venv\Scripts\activate`
- If activation fails, try: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
