# Scholar Setup Guide — From Zero to Writing

This guide takes you from a fresh computer with no developer tools to a fully working Scholar plugin in VS Code. Every step includes the exact command or click. Works on **macOS**, **Windows**, and **Linux**.

**Time estimate:** 20-30 minutes (mostly waiting for downloads)

**What you'll have when done:**
- VS Code with Claude Code built in
- 9 academic writing skills you can invoke with `/scholar:skill-name`
- Your Zotero library connected for citation search and verification

---

## Step 1: Install Node.js

Node.js is needed by Claude Code and by the Word document generator.

| Platform | How to install |
|----------|---------------|
| **macOS** | Option A (recommended): Install Homebrew first, then `brew install node` — see below. Option B: Download from https://nodejs.org/ (LTS version) |
| **Windows** | Open PowerShell and run: `winget install OpenJS.NodeJS.LTS` — or download from https://nodejs.org/ (LTS version). **After installing, close and reopen PowerShell.** |
| **Linux (Ubuntu/Debian)** | `sudo apt update && sudo apt install -y nodejs npm` |
| **Linux (Fedora)** | `sudo dnf install nodejs npm` |

### macOS: Install Homebrew first (recommended)

Homebrew is a package manager that makes installing dev tools easy. Open **Terminal** (`Cmd + Space`, type "Terminal", hit Enter) and paste:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

It will ask for your Mac password (you won't see characters as you type — that's normal). When it finishes, it may show "Next steps" commands — **run those commands**. They look like:

```bash
echo >> ~/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

Then install Node.js:

```bash
brew install node
```

### Verify (all platforms)

Open a terminal and type:

```bash
node --version
```

You should see `v20` or higher.

---

## Step 2: Install Git (+ Git Bash on Windows)

Git is needed to download the Scholar plugin. **On Windows, Claude Code specifically requires Git Bash** (a Unix-like shell that comes bundled with Git for Windows).

| Platform | How to install |
|----------|---------------|
| **macOS** | Already included with Xcode Command Line Tools. If not: `xcode-select --install` or `brew install git` |
| **Windows** | Download from https://git-scm.com/download/win — run installer, **use all default options** (Git Bash is included). On the "Adjusting your PATH" screen, keep the default: "Git from the command line and also from 3rd-party software". **After installing, close and reopen PowerShell.** |
| **Linux** | `sudo apt install git` (Ubuntu/Debian) or `sudo dnf install git` (Fedora) |

### Windows: Allow PowerShell scripts

Windows blocks scripts by default. Run this once in PowerShell:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

If it asks you to confirm, type `Y` and press Enter.

### Windows: If Claude Code can't find Git Bash later

Set this environment variable in PowerShell:

```powershell
[System.Environment]::SetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "C:\Program Files\Git\bin\bash.exe", "User")
```

Close and reopen PowerShell after running this.

### Verify (all platforms)

```bash
git --version
```

---

## Step 3: Install VS Code

| Platform | How to install |
|----------|---------------|
| **macOS** | `brew install --cask visual-studio-code` or download from https://code.visualstudio.com/ |
| **Windows** | Download from https://code.visualstudio.com/ — run installer, check "Add to PATH" |
| **Linux** | Download `.deb` or `.rpm` from https://code.visualstudio.com/ or use snap: `sudo snap install code --classic` |

**Open VS Code** after installation.

---

## Step 4: Install Claude Code Extension in VS Code

This step is identical on all platforms:

1. Open VS Code
2. Click the **Extensions** icon in the left sidebar (looks like 4 squares, or press `Ctrl+Shift+X` / `Cmd+Shift+X`)
3. Search for **"Claude Code"**
4. Click **Install** on the one by **Anthropic**
5. After install, you'll see a Claude icon in the left sidebar

### Sign in to Claude

1. Click the Claude icon in the VS Code sidebar
2. Click **Sign in**
3. Sign in with your Anthropic account (or create one at https://claude.ai)
4. You need a Claude Pro, Team, or Max subscription — or Anthropic API credits

---

## Step 5: Install the Scholar Plugin

Open the **terminal inside VS Code**:
- macOS: `Ctrl + `` ` `` or menu: Terminal > New Terminal
- Windows/Linux: `Ctrl + `` ` `` or menu: Terminal > New Terminal

Run:

```bash
claude plugin install --from github:mcmespinaa/scholar
```

If that command doesn't work (plugin install syntax may vary by Claude Code version), use the manual method:

```bash
git clone https://github.com/mcmespinaa/scholar.git ~/.claude/scholar
```

On **Windows**, the path is different:

```powershell
git clone https://github.com/mcmespinaa/scholar.git "$env:USERPROFILE\.claude\scholar"
```

**Verify:** In the Claude Code panel in VS Code, type `/scholar:` and you should see 9 skills listed.

---

## Step 6: Install the Word Document Generator

The writing skills produce `.docx` files. Install the library they need:

```bash
npm install -g docx
```

On **Linux**, you may need `sudo`:

```bash
sudo npm install -g docx
```

---

## Step 7: Set Up Zotero (for Citation Skills)

The citation skills search your Zotero reference library. Skip to 7c if you already use Zotero.

### 7a. Install Zotero

Download and install from: https://www.zotero.org/download/

Available for macOS, Windows, and Linux. Create a free account at: https://www.zotero.org/user/register

Sign in: Zotero > Settings > Sync > enter your credentials.

### 7b. Install Better BibTeX Plugin

Better BibTeX adds the auto-export feature that Scholar needs.

1. Download the latest `.xpi` file from: https://github.com/retorquere/zotero-better-bibtex/releases/latest
2. In Zotero: Tools > Add-ons > gear icon > Install Add-on From File > select the `.xpi`
3. Restart Zotero

### 7c. Export Your Library as CSL-JSON

This creates the file that Scholar's citation skills read:

1. In Zotero, right-click the collection you want to use (e.g., your thesis collection)
2. Select **Export Collection...**
3. Set format to: **Better CSL JSON**
4. Check **Keep updated** (auto-exports when you add new references)
5. Save to a location you'll remember:
   - macOS/Linux: `~/Documents/references/my-library.json`
   - Windows: `C:\Users\YourName\Documents\references\my-library.json`
6. Click OK

### 7d. Configure Scholar to Find Your Library

In the VS Code terminal:

**macOS/Linux:**
```bash
cd ~/.claude/scholar
cp templates/local.md .local.md
```

**Windows (PowerShell):**
```powershell
cd "$env:USERPROFILE\.claude\scholar"
copy templates\local.md .local.md
```

Open `.local.md` in VS Code and update the `ZOTERO_LIBRARY` path to where you saved your CSL-JSON file:

```
ZOTERO_LIBRARY: ~/Documents/references/my-library.json
```

On Windows, use the full path:
```
ZOTERO_LIBRARY: C:\Users\YourName\Documents\references\my-library.json
```

---

## Step 8: Test It

Open any folder in VS Code (File > Open Folder), then open the Claude Code panel and try:

```
/scholar:citation-helper "sustainability leadership"
```

If you have references about sustainability leadership in your Zotero library, Scholar will find and return them with citation keys.

Try a writing skill:

```
/scholar:research-proposal "SDG 12 and circular economy in Swedish SMEs"
```

---

## Optional: Install Pandoc

Pandoc renders `[@citekey]` citations into formatted Word documents with an automatic reference list. Optional — the writing skills can produce .docx without it.

| Platform | How to install |
|----------|---------------|
| **macOS** | `brew install pandoc` |
| **Windows** | Download from https://pandoc.org/installing.html |
| **Linux** | `sudo apt install pandoc` (Ubuntu/Debian) or `sudo dnf install pandoc` (Fedora) |

Download the APA 7 citation style:

**macOS/Linux:**
```bash
mkdir -p ~/Documents/references
curl -L -o ~/Documents/references/apa7.csl \
  https://raw.githubusercontent.com/citation-style-language/styles/master/apa.csl
```

**Windows (PowerShell):**
```powershell
mkdir $env:USERPROFILE\Documents\references -Force
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/citation-style-language/styles/master/apa.csl" -OutFile "$env:USERPROFILE\Documents\references\apa7.csl"
```

Update your `.local.md`:
```
CSL_FILE: ~/Documents/references/apa7.csl
```

---

## Quick Reference

Once everything is installed, here's what you can do:

| Command | What it does |
|---------|-------------|
| `/scholar:research-proposal "topic"` | Write a research proposal |
| `/scholar:academic-essay "topic"` | Write an academic essay or literature review |
| `/scholar:systematic-review "topic"` | Run a 7-phase systematic review pipeline |
| `/scholar:project-report "topic"` | Write a project report with BMC |
| `/scholar:academic-editor "file.md"` | 5-pass editorial review of a draft |
| `/scholar:proposal-grader "file.md"` | Grade a proposal against rubric (A-F) |
| `/scholar:citation-helper "query"` | Search your Zotero library |
| `/scholar:citation-checker "file.md"` | Audit all citations in a document |
| `/scholar:citation-fixer "file.md"` | Auto-fix citation issues |

---

## Troubleshooting

### "command not found: node" (macOS/Linux)
Close and reopen your terminal. On macOS, make sure you ran the Homebrew "Next steps" commands from Step 1.

### "node is not recognized" (Windows)
Close and reopen PowerShell. If still not working, restart your computer — Windows needs a restart to pick up new PATH entries. If you used `winget`, try opening a fresh PowerShell window.

### "cannot be loaded because running scripts is disabled" (Windows)
Run this once in PowerShell:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### "Claude Code on Windows requires git-bash" (Windows)
Git Bash is not installed or Claude Code can't find it. Install Git for Windows from https://git-scm.com/download/win (Git Bash is included). If already installed, set the path manually:
```powershell
[System.Environment]::SetEnvironmentVariable("CLAUDE_CODE_GIT_BASH_PATH", "C:\Program Files\Git\bin\bash.exe", "User")
```
Close and reopen PowerShell after.

### "command not found: claude"
The Claude Code CLI may not be on your PATH. If you're using Claude Code only through the VS Code extension, you don't need the CLI — the `/scholar:` commands work directly in the extension's chat panel.

### Paths with spaces cause errors (Windows)
If your Windows username or folder path contains spaces (e.g., `C:\Users\John Smith\`), commands may break. When entering paths in prompts, **never wrap them in quotes** — enter the bare path. If a path has spaces, use the short form or rename the folder to remove spaces.

### Skills don't show up when typing /scholar:
Make sure the plugin is installed. In VS Code's Claude panel, type `/` and look for skills starting with `scholar:`. If nothing shows, try reinstalling:
```bash
claude plugin install --from github:mcmespinaa/scholar
```

### Citation skills say "library not found"
Check that your `.local.md` file has the correct path to your CSL-JSON export. The file must exist at the path you specified.

### Zotero export isn't updating
In Zotero: Edit > Settings > Better BibTeX > Automatic export > make sure scheduling is set to "On change" (not "Paused").

### Permission errors with npm install -g (Linux)
Use `sudo npm install -g docx` or configure npm to install globally without sudo:
```bash
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## Getting Help

- Plugin issues: https://github.com/mcmespinaa/scholar/issues
- Claude Code docs: https://docs.anthropic.com/en/docs/claude-code
- Zotero forums: https://forums.zotero.org/
