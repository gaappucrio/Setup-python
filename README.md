# Step-by-Step Programming Setup

This is my professional setup for programming in Python, focusing on adding the tools to best support the task I usually do. Since some libraries have continued support for Linux systems (like GPU use for tensorflow), this focuses the installation of Windows Subsystem for Linux, along tools to facilitate terminal use (Oh My Zsh) and integration to external tools (like GitHub).

---

## Version Verification

Some installation steps are different depending on the windows version you have. Although all screenshots in this material show the Windows 11 installation, any differences for Windows 10 installations are mentioned in the text. If nothing is specified, the steps are the same in both versions.

To verify your windows version:
* Use `Win + R` to open the run dialog box.
* Run the `winver` command.
* A window will pop up with your Windows version.

![Winver Executar](images/winver_executar.png) 
![Sobre o Windows](images/winver_sobre.png)

* If you have Windows 11, all good.
* If you have Windows 10 showing 2004 or above, all good.
* If you have Windows 10 below 2004, you need to update the system.
  * Use `Win + R` to open the run dialog box.
  * Run the `ms-settings:windowsupdate` command.
  * Check and execute updates until you have at least a Windows 10 showing 2004.

---

## Enable Virtualization

We need virtualization activated to enable multiple virtual machines on the computer. To check if it's already active:
* Use `Win + R` to open the run dialog box.
* Run the `taskmgr` command.
* Under the Performance tab, select the CPU option.
* Look for "Virtualization".

![Gerenciador de Tarefas](images/taskmgr_virtualization.png)

* If it's "Enabled", all good.
* If it's not listed or if it's "Disabled", follow a Windows tutorial to activate it.

---

## WSL and Ubuntu

WSL (Windows Subsystem for Linux) allows us to run a Linux environment within a windows machine, without needing to set dual-boot. We'll use Ubuntu (an open-source Linux-based OS) with WSL 2 for better performance and compatibility.

We'll start by installing wsl with administrator privileges in the command window (cmd):
* Use `Win + R` to open the run dialog box.
* Type the `cmd` command, but don't press "ok".
* Press `Ctrl + Shift + Enter` to run as administrator.
* Select the Yes option.
* A terminal executing as Windows System will pop up.
* In the terminal, run the command `wsl --install`.
* An Ubuntu installation will begin and restarting your computer might be required.

![CMD Administrator](images/cmd_admin.png)

Now we'll check the wsl version through the cmd:
* Use `Win + R` to open the run dialog box.
* Run the `cmd` command (no need to run with Admin Privileges).
* In the terminal, run the command `wsl -l -v`.
* If it shows version 2, all good.
* If it shows version 1:
  * In the terminal, run the command `wsl --set-version Ubuntu 2`.
  * After completion, run the command `wsl -l -v` once more to make sure all is good.

With the WSL installed and configured, type "Terminal" on your windows search bar and open the Terminal app. Powershell is the default terminal, but you can open a new tab with any type of terminal installed in the computer by clicking on the arrow pointing down. Select the Ubuntu terminal.

*(If you're using Windows 10, you can install the Terminal app through the Microsoft Store.)*

![Windows Terminal Ubuntu](images/terminal_ubuntu.png)

On the first time opening the Ubuntu terminal, it will prompt you to create a user account:
* Type a username only with lowercase, one word, and no special characters nor space.
* Type a password you won't forget (characters are invisible and the pointer won't move).
* Confirm the password.

Congratulations, you have access to Linux on your Windows machine.

Let's verify that the default locale is English:
* In the Ubuntu terminal, run the `locale` command.
* If the list doesn't include `LANG=en_US.UTF-8`, run the command `sudo locale-gen en_US.UTF-8`.
* Input your password for authorizing it.
* Close the terminal and open a new one.
* Rerun the `locale` command to verify the installation.

If `LANG=en_US.UTF-8` is still missing, update the locale by running these 3 lines of code:
```bash
sudo update-locale LANG=en_US.UTF8
sudo apt-get update
sudo apt-get install language-pack-en language-pack-en-base manpages
Then rerun the following commands:

Run sudo locale-gen en_US.UTF-8.

Input your password.

Close the terminal, open a new one, and rerun locale. LANG=en_US.UTF8 should be listed.

Before we continue, make sure to pin the Terminal app to your windows bar.

Visual Studio Code
Some of the configurations we'll set will require directly editing a specific line of code in a text file. We'll install Visual Studio Code (VSCode) to make this process easier. Go to the VSCode website and download the windows installer. When installing the app make sure to select all options tagged as "Other" on the setup step.

Once the installation is done, let's connect VSCode to WSL:

Run the command code --install-extension ms-vscode-remote.remote-wsl on your Ubuntu terminal.

Test if it works by typing the command code . on the terminal (the dot indicates you want to execute the command on the current folder).

This should open VSCode in your Ubuntu profile (Note the WSL:Ubuntu blue rectangle on the lower left corner).

Configuring the Windows Terminal App
Let's start by making Ubuntu the default terminal:

Right click the top of the terminal window and select Settings.

Select Ubuntu as the default profile and choose a color theme (Save before continuing).

On the bottom left corner, click on Open JSON file.

The code for the terminal settings will open on VSCode.

Above "profiles" add a new line with the following code:

JSON
"multiLinePasteWarning": false,
In "Profiles" and then in "list", look for the block with "name": "Ubuntu".

Add the following line of code to the block:

JSON
"commandline": "wsl.exe ~",
Save the JSON file before closing the VSCode window.

The "commandline": "wsl.exe ~" line makes sure Ubuntu always starts at the home directory. The "multiLinePasteWarning": false line suppresses warnings when pasting multiple lines of code.

Improving the VSCode Experience
To install extensions for better coding and collaboration, run the following lines of code on your terminal (you can copy and paste all lines at the same time):

Bash
code --install-extension ms-python.python
code --install-extension KevinRose.vsc-python-indent
code --install-extension ms-python.vscode-pylance
code --install-extension ms-toolsai.jupyter
code --install-extension ms-vscode.sublime-keybindings
code --install-extension emmanuelbeziat.vscode-great-icons
code --install-extension MS-vsliveshare.vsliveshare
code --install-extension alexcvzz.vscode-sqlite
Improving the Terminal Experience
Now we'll customize the terminal using zsh and oh my zsh. Run the following codes on your terminal:

Bash
sudo apt update
sudo apt install -y curl git imagemagick jq unzip vim zsh tree
sh -c "$(curl -fsSL [https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh](https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh))"
Your password will be asked and press "Y" when it asks if you want to change the default shell to zsh.

From now on, you will be required to run the command exec zsh to reset your terminal without needing to close and open a new terminal window.

Our next step is to install direnv, which allows us to set envrc and env files, so we can have environmental variables restricted to specific directories. Run the following code:

Bash
sudo apt-get update; sudo apt-get install direnv
Connecting to GitHub
We'll install the GitHub Command line interface (CLI) to easily pull, push, and manage GitHub repositories from our terminal. Run these commands:

Bash
sudo apt remove -y gitsome # avoid conflict with gitsome if already installed
curl -fsSL [https://cli.github.com/packages/githubcli-archive-keyring.gpg](https://cli.github.com/packages/githubcli-archive-keyring.gpg) | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] [https://cli.github.com/packages](https://cli.github.com/packages) stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install -y gh
Reset your terminal (exec zsh) and check if the gh command works by typing gh --version.

Now we can link our account to the GitHub CLI using SSH keys. Run:

Bash
gh auth login -s 'user:email' -w --git-protocol ssh
Answer "Y" to generate a new SSH key.

Provide a password.

Give it a title.

It will give you a one-time code and a link. Open it in your browser, select your profile, and add the code.

Run gh auth status for final confirmation.

Adding and Editing Dotfiles for Further Configurations
We'll start editing our dotfiles directly. Make sure you're at your Ubuntu user directory by running cd. Open VSCode in this directory by running code ..

Customizing zsh and git
Create or edit the following dotfiles with their respective content:

File called .zprofile

Bash
# Setup the PATH for pyenv binaries and shims
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(/opt/homebrew/bin/brew shellenv 2>/dev/null)"
type -a pyenv > /dev/null && eval "$(pyenv init --path)"
File called .rspec

Plaintext
--color --format documentation
File called .irbrc

Ruby
begin
  require 'rubygems'
  require 'pry'
rescue LoadError
end
if defined?(Pry)
  Pry.start
  exit
end
File called .pryrc

Ruby
# [https://github.com/pry/pry/tree/master/lib/pry](https://github.com/pry/pry/tree/master/lib/pry)
if defined?(Rails)
  short_env_name_options = {
    'development' => 'dev',
    'production' => 'prod'
  }
  app_name = Rails.application.class.module_parent_name.underscore.dasherize
  env_name = short_env_name_options.fetch(Rails.env) { Rails.env }
  description = 'Prompt has to match the rails app name'
else
  current_directory = Dir.pwd.split('/').last.to_s
  description = 'Prompt has to match the current directory name'
end

# [https://github.com/pry/pry/blob/master/lib/pry/prompt.rb](https://github.com/pry/pry/blob/master/lib/pry/prompt.rb)
Pry::Prompt.add(:current_app) do |context, nesting, pry_instance, sep|
  format(
    '[%<in_count>s] %<current_app>s(%<context>s)%<nesting>s%<separator>s',
    in_count: pry_instance.input_ring.count,
    current_app: app_name || current_directory,
    context: env_name || Pry.view_clip(context),
    nesting: (nesting > 0 ? ":#{nesting}" : ''),
    separator: sep
  )
end

prompt = Pry::Prompt[:current_app]
procs = [
  proc { |*args| prompt.wait_proc.call(*args).to_s },
  proc { |*args| prompt.incomplete_proc.call(*args).to_s }
]

Pry.config.prompt = Pry::Prompt.new(
  'custom_app_prompt', description, procs
)
File called .aliases

Bash
# Get External IP / Internet Speed
alias myip="curl [https://ipinfo.io/json](https://ipinfo.io/json)" # or /ip for plain-text ip
alias speedtest="curl -s [https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py](https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py) | python -"

# Quickly serve the current directory as HTTP
alias serve='ruby -run -e httpd . -p 8000' # Or python -m SimpleHTTPServer :)

# NOTE: On Q3 2021, Le Wagon decided to change the Web Dev curriculum default text editor
alias stt="echo 'Launching VS Code instead of Sublime Text... (cf ~/.aliases)' && code ."
File called .gitconfig

Ini, TOML
[color]
  branch = auto
  diff = auto
  interactive = auto
  status = auto
  ui = auto
[color "branch"]
  current = green
  remote = yellow
[core]
  pager = less -FRSX
  editor = code --wait
[alias]
  co = checkout
  st = status -sb
  br = branch
  ci = commit
  fo = fetch origin
  d = !git-no-pager diff
  dt = difftool
  stat = !git-no-pager diff --stat
  # Set remotes/origin/HEAD -> defaultBranch
  remoteSetHead = remote set-head origin -a
  # Get default branch name
  defaultBranch = !git symbolic-ref refs/remotes/origin/HEAD | cut -d'/' -f4
  # Clean merged branches
  sweep = !git branch --merged $(git defaultBranch) | grep -E -v "^* $(git defaultBranch)$" | xargs -r git branch -d && git remote prune origin
  # Pimping out git log
  lg = log --graph --all --pretty=format:'%Cred%h%Creset - %s %Cgreen(%cr) %C(bold blue)<%an>%Creset %C(yellow)%d%Creset'
  # Serve local repo
  serve = !git daemon --reuseaddr --verbose --base-path=. --export-all ./.git
  # Checkout to defaultBranch
  m = !git checkout $(git defaultBranch)
  # Removes a file from the index
  unstage = reset HEAD
[help]
  autocorrect = 1
[push]
  default = simple
[branch "master"]
  mergeoptions = --no-edit
[pull]
  rebase = false
[init]
  defaultBranch = master
Don't close your VSCode window. Copy and run the following code in its entirety in your terminal to install some oh my zsh plugins:

Bash
CURRENT_DIR=$(pwd)
ZSH_PLUGINS_DIR="$HOME/.oh-my-zsh/custom/plugins"
mkdir -p "$ZSH_PLUGINS_DIR"
cd "$ZSH_PLUGINS_DIR" || exit

# zsh-autosuggestions
if [ ! -d "zsh-autosuggestions" ]; then
  echo "----> Installing zsh plugin 'zsh-autosuggestions'..."
  git clone [https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
fi

# zsh-syntax-highlighting
if [ ! -d "zsh-syntax-highlighting" ]; then
  echo "-----> Installing zsh plugin 'zsh-syntax-highlighting'..."
  git clone [https://github.com/zsh-users/zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
fi
cd "$CURRENT_DIR"
Customizing VSCode
Back to your VSCode, navigate through the browser (left menu) vscode-server > data > Machine. Create the following files:

File called settings.json

<details>
<summary>Click to expand settings.json</summary>

JSON
{
  "[python]": {
    "editor.tabSize": 4
  },
  "editor.bracketPairColorization.enabled": true,
  "editor.detectIndentation": false,
  "editor.fontSize": 14,
  "editor.guides.bracketPairs": true,
  "editor.minimap.enabled": false,
  "editor.multiCursorModifier": "ctrlCmd",
  "editor.renderControlCharacters": true,
  "editor.rulers": [
    80,
    120
  ],
  "editor.showFoldingControls": "always",
  "editor.snippetSuggestions": "top",
  "editor.tabSize": 2,
  "emmet.includeLanguages": {
    "erb": "html"
  },
  "emmet.showSuggestionsAsSnippets": true,
  "emmet.triggerExpansionOnTab": true,
  "explorer.confirmDelete": false,
  "files.exclude": {
    "**/.pycache": true,
    "**/.asset-cache": true,
    "**/.bundle": true,
    "**/.ipynb_checkpoints": true,
    "**/.pytest_cache": true,
    "**/.sass-cache": true,
    "**/.svn": true,
    "**/.DS_Store": true,
    "**/.egg-info": true,
    "**/.git": true,
    "build": true,
    "coverage": true,
    "dist": true,
    "log": true,
    "node_modules": true,
    "public/packs": true,
    "tmp": true
  },
  "files.associations": {
    "*.json": "jsonc"
  },
  "files.hotExit": "off",
  "files.insertFinalNewline": true,
  "files.trimFinalNewlines": true,
  "files.trimTrailingWhitespace": true,
  "files.watcherExclude": {
    "**/audits/**": true,
    "**/coverage/**": true,
    "**/log/**": true,
    "**/node_modules/**": true,
    "**/tmp/**": true,
    "**/vendor/**": true
  },
  "git.enabled": false,
  "notebook.diff.ignoreMetadata": true,
  "notebook.lineNumbers": "on",
  "notebook.markup.fontSize": 13,
  "python.defaultInterpreterPath": "~/.pyenv/shims/python",
  "python.formatting.provider": "yapf",
  "python.languageServer": "Pylance",
  "python.linting.enabled": false,
  "python.linting.flake8Enabled": false,
  "python.linting.pylintEnabled": false,
  "python.terminal.activateEnvironment": false,
  "ruby.lint": {
    "rubocop": true
  },
  "search.exclude": {
    "**/.venv": true
  },
  "telemetry.telemetryLevel": "off",
  "window.restoreWindows": "none",
  "window.newWindowDimensions": "maximized",
  "workbench.editor.enablePreview": true,
  "workbench.iconTheme": "vscode-great-icons",
  "workbench.settings.editor": "json",
  "workbench.settings.openDefaultSettings": true,
  "workbench.settings.useSplitJSON": true,
  "workbench.startupEditor": "newUntitledFile",
  "workbench.panel.defaultLocation": "right"
}
</details>

File called keybindings.json

JSON
// Place your key bindings in this file to override the defaults
[
  {
    "key": "ctrl+shift+v",
    "command": "pasteAndIndent.action",
    "when": "editorTextFocus && !editorReadonly"
  },
  {
    "key": "cmd+shift+v",
    "command": "pasteAndIndent.action",
    "when": "editorTextFocus && !editorReadonly"
  }
]
Finally, reset the terminal using exec zsh.

Customizing Git
Set your global GitHub user details. Use an email linked to your account (preferably the noreply.github.com email).

Bash
git config --global user.email "{email}"
git config --global user.name "{full_name}"
Add the ssh-agent to your plugins:

Open .zshrc: code ~/.zshrc.

Add ssh-agent inside the parenthesis of the plugins=(...) line.

Save, then reset the terminal with exec zsh. It will ask for your SSH password.

Connecting your Browser to the Terminal
Find your browser's .exe path (e.g., Chrome). Change backslashes \ to slashes /, and replace C: with /mnt/c.
Example path: /mnt/c/Program Files/Google/Chrome/Application/chrome.exe.

Run the following with your {PATH}:

Bash
echo "export BROWSER=\"{PATH}\"" >> ~/.zshrc
echo "export GH_BROWSER=\"{PATH}\"" >> ~/.zshrc
Reset the terminal (exec zsh).

Pyenv installation (Updated & Corrected)
Pyenv allows us to manage different Python versions.
First, make sure Anaconda is removed. Run conda list. If it says "command not found", you're good.

Clone the Pyenv repo:

Bash
git clone [https://github.com/pyenv/pyenv.git](https://github.com/pyenv/pyenv.git) ~/.pyenv
exec zsh
Install dependencies:

Bash
sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev sqlite3 libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev \
python3-dev
[CRITICAL STEP] Add Pyenv to your Zsh profile:
Copy and paste this whole block at once in your terminal and press Enter:

Bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
Then, force the terminal to read these new settings:

Bash
source ~/.zshrc
Now install python 3.12.7:

Bash
pyenv install 3.12.7
pyenv global 3.12.7
exec zsh
Install python virtual environment:

Bash
git clone [https://github.com/pyenv/pyenv-virtualenv.git](https://github.com/pyenv/pyenv-virtualenv.git) $(pyenv root)/plugins/pyenv-virtualenv
exec zsh
Create a virtual environment named base_venv:

Bash
pyenv virtualenv 3.12.7 base_venv
pyenv global base_venv
Installing All Purpose Packages
Start by upgrading pip:

Bash
pip install --upgrade pip
Create a requirements.txt file and add your packages (Refer to the original PDF for the full 150+ package list, which includes libraries like numpy, pandas, scikit-learn, tensorflow, torch, jupyter, etc.).

Install them by running:

Bash
pip install -r requirements.txt
Jupyter Configurations
Create a folder for jupyter's custom configurations:

Bash
LOCATION=$(jupyter --config-dir)/custom
mkdir -p $LOCATION
code .
Create custom.css inside the custom folder with the following:

CSS
/* Custom CSS for Jupyter Notebook */
details {
    padding: 5px 10px 5px;
    margin-bottom: 1em;
}
body[data-jp-theme-light=true] details {
    background-color: var(--jp-cell-editor-background);
}
summary {
    cursor: pointer;
    display: list-item;
    font-weight: bold;
    background-color: var(--jp-cell-editor-background);
}
details > p {
    margin-top: 5px !important;
    margin-bottom: 0 !important;
}
Generate the config file:

Bash
jupyter notebook --generate-config
sed -i.backup 's/# c.ServerApp.use_redirect_file = True/c.ServerApp.use_redirect_file = False/' ~/.jupyter/jupyter_notebook_config.py
Test your setup by running jupyter notebook. Inside a notebook cell, run:

Python
import sys; sys.version
If you get 3.12.7, you're all set!

Congratulations, your programming setup is complete.
