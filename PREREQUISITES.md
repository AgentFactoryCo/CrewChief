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

## Azure AI Foundry Local (Optional)

Azure AI Foundry Local is only required for AI-powered features (summary, suggest-maint, track-prep). All core garage management features work without it.

**Foundry Local** is Microsoft's local AI runtime that runs models on your machine with full privacy - no cloud required.

### System Requirements

- **OS**: Windows 10/11 (x64/ARM), Windows Server 2025, or macOS
- **RAM**: Minimum 8 GB (16 GB recommended)
- **Disk**: Minimum 3 GB free (15 GB recommended)

### Check if Foundry Local is installed

```bash
foundry --version
```

If installed, you'll see version information. If not, you'll get a "command not found" error.

### Install Foundry Local

**Windows:**
```bash
winget install Microsoft.FoundryLocal
```

**macOS:**
```bash
brew install microsoft/foundrylocal/foundrylocal
```

**Official Documentation:**
[Microsoft Learn - Get started with Foundry Local](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-local/get-started)

### Download and Run a Model

1. List available models:
   ```bash
   foundry model list
   ```

2. Download a model (recommended: phi-3.5-mini or similar 3-7B parameter model):
   ```bash
   foundry model download phi-3.5-mini
   ```

3. Start the Foundry Local service:
   ```bash
   foundry service start
   ```

4. Check the service status and find the API port:
   ```bash
   foundry service status
   ```

   **Important**: Foundry Local uses a dynamic port assignment. The output will show you the actual port (e.g., `http://localhost:52734/v1`).

### Configure CrewChief for Your Port

Since Foundry Local assigns ports dynamically, you need to configure CrewChief with your actual port:

1. Find your Foundry Local endpoint:
   ```bash
   foundry service status
   ```

2. Set the environment variable:
   ```bash
   export CREWCHIEF_LLM_BASE_URL=http://localhost:YOUR_PORT/v1
   ```

   Or create a `.env` file in your project directory:
   ```bash
   CREWCHIEF_LLM_BASE_URL=http://localhost:52734/v1
   CREWCHIEF_LLM_MODEL=phi-3.5-mini
   ```

### Verify Foundry is running

Replace `PORT` with your actual port from `foundry service status`:

```bash
curl http://localhost:PORT/v1/models
```

You should see JSON output listing available models.

### Recommended Models

For CrewChief, we recommend smaller, efficient models:

- **phi-3.5-mini** (3.8B) - Fast, efficient, good for most garage tasks
- **phi-3-medium** (14B) - Better reasoning, requires more resources
- **GPT-OSS-20b** - Optimized for local/edge, requires 16GB+ RAM

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
- [ ] (Optional) Azure AI Foundry Local running (`foundry service status`)

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

1. Check if Foundry service is running:
   ```bash
   foundry service status
   ```

2. If not running, start it:
   ```bash
   foundry service start
   ```

3. Verify you have the correct port configured in your `.env` file

4. Check Foundry logs for errors:
   ```bash
   foundry service logs
   ```

5. Restart the service if needed:
   ```bash
   foundry service restart
   ```

### Windows-specific issues

- If `python3` doesn't work, try `python` (without the 3)
- If scripts aren't found, activate the virtual environment: `.venv\Scripts\activate`
- If activation fails, try: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
