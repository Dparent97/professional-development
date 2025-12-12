# MacBook Pro M4 Development Setup Checklist
**Profile:** Work (dp)  
**Goal:** Transform new MacBook into a development powerhouse  
**Strategy:** Install tools systematically, migrate files selectively later

---

## Phase 1: Core Development Tools ✓
**Status:** COMPLETED
- [x] Homebrew (package manager)
- [x] Node.js 25.1.0 + npm
- [x] Claude Code 2.0.29
- [x] Warp (terminal)
- [x] Cursor (AI code editor)
- [x] Git configured with SSH keys
- [x] Rectangle (window management)
- [x] Raycast (productivity launcher)

---

## Phase 2: Essential Command-Line Tools
**Why:** These are the Unix tools developers use daily

### Package Managers & Essentials
```bash
brew install wget curl git-lfs tree jq yq fzf ripgrep fd bat exa
```
- `wget/curl`: Download files from terminal
- `git-lfs`: Handle large files in Git
- `tree`: Visualize directory structures
- `jq/yq`: Parse JSON/YAML files
- `fzf`: Fuzzy finder for files/commands
- `ripgrep (rg)`: Fast code search
- `fd`: Better alternative to `find`
- `bat`: Better `cat` with syntax highlighting
- `exa`: Modern `ls` replacement

### Development Utilities
```bash
brew install gh tldr httpie
```
- `gh`: GitHub CLI (issues, PRs from terminal)
- `tldr`: Quick command examples
- `httpie`: Human-friendly HTTP client for APIs


---

## Phase 3: Programming Languages & Runtimes
**Why:** Foundation for any development work

### Python Ecosystem
```bash
brew install python@3.12 pyenv poetry
```
- `python@3.12`: Latest stable Python
- `pyenv`: Manage multiple Python versions
- `poetry`: Modern Python dependency management

**Post-install:** 
```bash
pip3 install --upgrade pip
pip3 install ipython jupyter notebook pandas numpy requests
```

### Node.js Enhancements
```bash
npm install -g pnpm yarn typescript ts-node nodemon
```
- `pnpm`: Faster npm alternative
- `yarn`: Alternative package manager
- `typescript`: TypeScript compiler
- `ts-node`: Run TypeScript directly
- `nodemon`: Auto-restart on file changes

---

## Phase 4: Version Control & Git Enhancements
**Why:** Better Git workflow and collaboration

```bash
brew install git-delta lazygit tig
```
- `git-delta`: Beautiful git diffs
- `lazygit`: Terminal UI for Git
- `tig`: Text-mode interface for Git

**Git Global Config Enhancements:**
```bash
git config --global core.pager "delta"
git config --global interactive.diffFilter "delta --color-only"
git config --global delta.navigate true
git config --global merge.conflictstyle diff3
git config --global diff.colorMoved default
```

---

## Phase 5: Databases & Data Tools
**Why:** Local database development

```bash
brew install postgresql@16 redis sqlite
brew services start postgresql@16
brew services start redis
```
- `postgresql`: Industry-standard SQL database
- `redis`: In-memory data store
- `sqlite`: Lightweight embedded database

**Optional GUI:**
```bash
brew install --cask dbeaver-community
```

---

## Phase 6: Containers & Infrastructure
**Why:** Modern app deployment and development

```bash
brew install --cask docker orbstack
```
- `Docker`: Container platform (or use OrbStack)
- `OrbStack`: Faster Docker alternative for Mac

**Alternative:** OrbStack is lighter and faster than Docker Desktop for Mac

---

## Phase 7: API Development & Testing
**Why:** Test and debug APIs efficiently

```bash
brew install --cask postman insomnia
```
- `Postman`: Full-featured API platform
- `Insomnia`: Lightweight API client

**Or terminal-based:**
```bash
brew install httpie
```

---

## Phase 8: Modern Code Editors & IDEs
**Why:** Different tools for different tasks

```bash
brew install --cask visual-studio-code
```

**VS Code Extensions (install via code command):**
```bash
code --install-extension github.copilot
code --install-extension eamodio.gitlens
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
code --install-extension ms-python.python
code --install-extension bradlc.vscode-tailwindcss
```

---

## Phase 9: Design & Media Tools
**Why:** For "Model Behavior" content creation and UI work

```bash
brew install --cask figma
brew install ffmpeg imagemagick
```
- `Figma`: UI/UX design
- `ffmpeg`: Video/audio processing from terminal
- `imagemagick`: Image manipulation from terminal

**Optional:**
```bash
brew install --cask davinci-resolve # Professional video editing
```

---

## Phase 10: Browsers & Developer Tools
**Why:** Testing across browsers, extensions for dev

```bash
brew install --cask google-chrome firefox arc
```
- `Chrome`: Dev tools, React DevTools
- `Firefox`: Developer Edition
- `Arc`: Modern browser with workspaces

---

## Phase 11: Productivity & Organization
**Why:** Manage tasks, notes, and workflows

```bash
brew install --cask notion obsidian
```
- `Notion`: All-in-one workspace
- `Obsidian`: Local markdown notes
- **Things3**: Already installed, needs workflow setup

---

## Phase 12: Communication & Collaboration
**Why:** Work with teams

```bash
brew install --cask slack discord zoom
```

---

## Phase 13: Shell & Terminal Enhancements
**Why:** Make terminal more powerful and pleasant

### Oh My Zsh (shell framework)
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Starship Prompt (beautiful, fast prompt)
```bash
brew install starship
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
```

### Zsh Plugins
Add to `~/.zshrc`:
```bash
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

Install plugins:
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

---

## Phase 14: AI & Machine Learning Tools
**Why:** For AI development and content creation

```bash
brew install ollama
pip3 install openai anthropic langchain
```
- `ollama`: Run LLMs locally (Llama, Mistral, etc.)
- Python AI libraries for development

---

## Phase 15: File Management & Backup
**Why:** Organize and protect your work

```bash
brew install --cask the-unarchiver transmission
brew install rclone rsync
```
- `The Unarchiver`: Extract any archive format
- `rclone`: Sync files to cloud storage
- `rsync`: Efficient file transfer/backup

---

## Phase 16: System Utilities
**Why:** Enhance macOS functionality

```bash
brew install --cask stats istat-menus alfred
brew install htop btop dust duf
```
- `Stats/iStat Menus`: System monitoring in menu bar
- `Alfred`: Advanced Spotlight alternative (or keep Raycast)
- `htop/btop`: System resource monitoring
- `dust`: Better `du` for disk usage
- `duf`: Better `df` for disk free space

---

## Phase 17: Security & Privacy
**Why:** Secure your development environment

```bash
brew install --cask 1password
brew install gnupg pass
```
- `1Password`: Password manager
- `gnupg`: GPG encryption
- `pass`: Unix password manager

---

## Phase 18: Documentation & Writing
**Why:** Write READMEs, docs, and notes efficiently

```bash
brew install pandoc grip
npm install -g markdownlint-cli
```
- `pandoc`: Convert between document formats
- `grip`: Preview GitHub markdown locally
- `markdownlint`: Lint markdown files

---

## Phase 19: Performance & Monitoring
**Why:** Profile and optimize your code

```bash
brew install hyperfine wrk
```
- `hyperfine`: Benchmark command-line programs
- `wrk`: HTTP benchmarking tool


---

## Phase 20: macOS Configuration & Dotfiles
**Why:** Consistent environment setup

### Create dotfiles repository
```bash
mkdir -p ~/Developer/dotfiles
cd ~/Developer/dotfiles
git init
```

### Essential dotfiles to create:
- `.zshrc` - Zsh configuration
- `.gitconfig` - Git settings
- `.gitignore_global` - Global Git ignores
- `Brewfile` - Homebrew dependencies list

### Generate current Brewfile:
```bash
brew bundle dump --file=~/Developer/dotfiles/Brewfile
```

---

## Post-Installation Tasks

### 1. Configure Git Identity (if not done)
```bash
git config --global user.name "Derek Parent"
git config --global user.email "dparent97@gmail.com"
```

### 2. Set up SSH keys for other services
- GitLab
- Bitbucket
- Any other Git hosts

### 3. System Preferences Optimization
```bash
# Show hidden files in Finder
defaults write com.apple.finder AppleShowAllFiles YES
killall Finder

# Faster key repeat
defaults write NSGlobalDomain KeyRepeat -int 1
defaults write NSGlobalDomain InitialKeyRepeat -int 10

# Show file extensions
defaults write NSGlobalDomain AppleShowAllExtensions -bool true

# Disable "Are you sure you want to open" dialog
defaults write com.apple.LaunchServices LSQuarantine -bool false
```

### 4. Create Standard Directory Structure
```bash
mkdir -p ~/Developer/{projects,experiments,learning,model-behavior}
mkdir -p ~/Documents/{maritime,personal,work}
mkdir -p ~/Downloads/archive
```

### 5. Set up Things3 Workflows
- Create Areas: Maritime, Development, Model Behavior, Personal
- Create Projects for current priorities
- Set up recurring tasks for maintenance

---

## Optional Advanced Tools

### Code Quality & Linting
```bash
brew install shellcheck hadolint yamllint
npm install -g eslint prettier stylelint
```

### Cloud CLIs
```bash
brew install awscli azure-cli google-cloud-sdk
```

### Database CLIs
```bash
brew install postgresql mysql
```

### Monitoring & Debugging
```bash
brew install --cask little-snitch proxyman charles
```

---

## Checklist Summary

**Essential (Do First):**
- Phases 2-4: CLI tools, languages, Git enhancements
- Phase 6: Docker/OrbStack
- Phase 13: Terminal enhancements

**High Priority:**
- Phases 5, 7-9: Databases, API tools, design tools
- Phase 10: Browsers
- Phase 20: Dotfiles and system config

**Medium Priority:**
- Phases 11-12: Productivity and communication
- Phases 14-16: AI tools, file management, utilities

**Nice to Have:**
- Phases 17-19: Security, documentation, monitoring
- Optional Advanced Tools section

---

## Notes for Claude Code

When executing this checklist:
1. **Confirm before installing** large applications (Docker, IDEs, etc.)
2. **Check existing installations** before reinstalling
3. **Document any errors** for troubleshooting
4. **Group related installations** for efficiency
5. **Test each tool** after installation
6. **Create backup points** before major changes
7. **Update this checklist** as things are completed

---

## Migration Strategy (Separate Phase)

**After dev tools are set up:**
1. Analyze old MacBook's important files
2. Create staging folders on new Mac
3. Selectively copy (not migrate) essential files:
   - Active maritime PDFs → `~/Documents/maritime/`
   - Model Behavior assets → `~/Developer/model-behavior/`
   - Any active code projects → `~/Developer/projects/`
4. Leave the chaos behind - fresh start!

---

**Created:** October 30, 2025  
**Last Updated:** December 12, 2025  
**Maintained by:** Claude Code + Derek Parent
