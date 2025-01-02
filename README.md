## How to Set Up Project-Specific Shell Histories with `direnv`

This guide will help you set up project-specific shell histories using `direnv`. Follow these steps for each project.

`direnv` is a tool for managing environment variables based on the current directory. You can use it to set `HISTFILE` dynamically and to keep the project-specific command histories and separate system command histories by customizing your shell configuration.

---

## Step-by-Step Instructions

### 1. Install `direnv`
- On Gentoo, install it with:
  ```bash
  sudo emerge -av app-shells/direnv
  ```

### 2. Enable `direnv` for Your Shell
- Add the following line to the end of your `~/.bashrc` file:
  ```bash
  eval "$(direnv hook bash)"
  ```
- Reload the shell to apply the changes:
  ```bash
  source ~/.bashrc
  ```

### 3. Initialize a Project Directory
- Navigate to the folder for your project:
  ```bash
  cd /path/to/your/project
  ```

### 4. Create a `.envrc` File in the Project Directory
- Create a `.envrc` file to set a custom history file for the project:
  ```bash
  echo 'export HISTFILE=$PWD/.bash_history' > .envrc
  ```

### 5. Allow the `.envrc` File
- Allow the `.envrc` file with the following command:
  ```bash
  direnv allow
  ```

### 6. Verify the Setup
- `cd` into the project directory:
  ```bash
  cd /path/to/your/project
  ```
- Check if the `HISTFILE` environment variable is set correctly:
  ```bash
  echo $HISTFILE
  ```
  You should see something like:
  ```plaintext
  /path/to/your/project/.bash_history
  ```

### 7. Test Command History
- Run a few test commands in the project directory (e.g., `echo "test command"`).
- Exit the shell and re-enter the project directory. Use the **Up Arrow** key to confirm your commands are saved in the `.bash_history` file.

### 8. Keep the `.bash_history` File in the Project Directory
- The `.bash_history` file will stay in the project folder and contain only the commands you’ve used in that project.

---

## Optional: Tweak Your Shell for Better History Handling
- To ensure smooth history switching, add the following to your `~/.bashrc` file:
  ```bash
  PROMPT_COMMAND='history -a; history -c; history -r'
  ```
  This ensures the shell writes and reloads the history file when switching directories.
  
1. `history -a` - This command appends any commands from the current shell session to the history file (`HISTFILE`, e.g., `.bash_history`).
+ It ensures that your current session's commands are immediately saved to the history file, so they aren't lost if you switch directories or open a new shell.
2. `history -c` - This command clears the in-memory history of the current shell session.
+ It removes the current session’s command history from memory, preparing to reload it from the history file.
3. `history -r` - This command reads the contents of the history file (HISTFILE) and loads it into the shell's memory.
+ It ensures that the history from the file (potentially updated by other sessions or directories) is available in your current shell.

### Why Use It?
When working with project-specific histories:
  + <b>Without this setup:</b> The shell's in-memory history might conflict with or overwrite the history in the project-specific file. For example, if you switch between projects, you might accidentally bring commands from one project into another.
  + <b>With this setup:</b> Every time the shell prompt appears:
    + The current session's commands are saved to the file (`history -a`).
    + The in-memory history is cleared (`history -c`).
    + The shell re-reads the history file (`history -r`), ensuring it always matches the `HISTFILE` for the current directory.

---

## Setting Up a New Project
For each new project, repeat steps 3–5:
1. Create a `.envrc` file in the project directory.
   ```bash
   echo 'export HISTFILE=$PWD/.bash_history' > .envrc
   ```
2. Allow the `.envrc` file:
   ```bash
   direnv allow
   ```

That’s it! You now have project-specific shell histories.
<br>
<br>

