Remote Development with VS Code
========================================

Using VS Code with Remote-SSH is one of the best ways to develop code on Falcon. Instead of editing files in vim and struggling with limited terminal editors, you get the full power of VS Code on your local machine while your code actually runs on Falcon's supercomputer.

Why Use Remote-SSH?
------------------------------------

Working directly on Falcon using vim or nano has limitations:

- **Limited editing experience**: Terminal editors are powerful but less user-friendly than modern IDEs
- **No built-in debugging**: It's harder to step through code and inspect variables
- **No extensions**: You miss out on language support, linters, formatters, etc.
- **Slow feedback loop**: Making small changes requires saving, quitting the editor, and retesting

With VS Code Remote-SSH, you get:

- **Full VS Code features**: IntelliSense, syntax highlighting, code completion, refactoring tools
- **Integrated terminal**: Run bash commands on Falcon without leaving VS Code
- **Debugging**: Set breakpoints, inspect variables, step through code in Python, JavaScript, etc.
- **Extensions**: Install linters, formatters, language servers, and productivity tools
- **Local editing experience**: Edit files like you're on your local machine, but they're on Falcon
- **Seamless workflow**: Write, test, and debug all in one place

The magic is that VS Code runs a small server on Falcon that communicates with your local VS Code client. You edit locally, but the code executes on Falcon's powerful hardware.

Prerequisites
------------------------------------

Before starting, make sure you have:

- **VS Code installed** on your local machine (https://code.visualstudio.com/)
- **SSH access to Falcon** (completed the :doc:`access` and :doc:`loggingin` guides)
- **SSH config set up** (configured `~/.ssh/config` as shown in :doc:`loggingin`)
- **VPN access** to Cardiff University (required for all Falcon connections)

Step 1: Install VS Code
------------------------------------

If you don't already have VS Code:

1. Go to https://code.visualstudio.com/
2. Download the version for your operating system (Windows, macOS, or Linux)
3. Run the installer and follow the prompts
4. Launch VS Code

Step 2: Install the Remote-SSH Extension
------------------------------------

1. Open VS Code
2. Click the **Extensions icon** on the left sidebar (it looks like four squares)
3. Search for "Remote - SSH" (the official one is by Microsoft)
4. Click **Install**

The extension should appear in your sidebar as a new icon that looks like a remote connection symbol (><).

Step 3: Open a Remote-SSH Connection
------------------------------------

**Quick Method: Using the Command Palette**

1. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS) to open the Command Palette
2. Type "Remote-SSH: Connect to Host..."
3. Select it
4. Choose your host (if you've set up SSH config, you'll see "falcon" in the list)
5. Press Enter
6. VS Code will connect to Falcon and set up the remote server
7. If prompted for a password, enter your Falcon password

**Alternative Method: Using the Bottom-Left Icon**

1. Click the "><" icon in the bottom-left corner of VS Code
2. Select "Connect to Host..."
3. Choose your host from the list
4. Enter your password if prompted

**First Time Connection**

The first time you connect, VS Code will:
- Install a server on Falcon (in `~/.vscode-server`)
- Download necessary dependencies
- This takes a few minutes but only happens once

You'll know you're connected when:
- The "><" icon in the bottom-left shows "SSH: falcon"
- The explorer sidebar shows your Falcon home directory
- The integrated terminal runs commands on Falcon

Step 4: Open a Project Folder
------------------------------------

Once connected to Falcon:

1. Click **File** > **Open Folder**
2. Navigate to your project folder (e.g., `/home/yourusername/projects/myproject`)
3. Click **Select Folder**
4. VS Code will load your project

You can now edit files normally. The files are on Falcon, but you're editing them with VS Code's full feature set.

Using the Integrated Terminal
------------------------------------

One of the most useful features is the integrated terminal, which automatically runs commands on Falcon:

1. Press `` Ctrl+` `` (backtick) or go to **Terminal** > **New Terminal**
2. A terminal opens at the bottom of VS Code
3. Any command you run executes on Falcon
4. You can type bash commands just like SSH-ing in normally

This is perfect for:
- Running your code: `python analysis.py`
- Checking job status: `squeue -u $USER`
- Submitting SLURM jobs: `sbatch myjob.sh`
- Testing quick commands before making changes

**Example Workflow:**

1. Edit your Python script in the editor
2. Save the file (`Ctrl+S`)
3. Open the integrated terminal
4. Run `python -u analysis.py` to test
5. See the output in the terminal
6. Make fixes in the editor
7. Re-run the command

VS Code Extensions for Remote Development
------------------------------------

Once connected via Remote-SSH, you can install extensions that work on Falcon. Some useful ones:

**Python Development**
- **Python** (Microsoft): Full Python support with IntelliSense and debugging
- **Pylance**: Advanced Python language support
- **Ruff**: Fast Python linter and formatter

**Code Quality**
- **ESLint**: JavaScript linting
- **Prettier**: Code formatter
- **Black Formatter**: Python code formatter

**General Productivity**
- **GitLens**: Git history and blame information
- **Git Graph**: Visualize your Git history
- **Thunder Client**: Make HTTP requests (useful for testing APIs)
- **SFTP**: Upload/download files (though less useful with Remote-SSH)

**Installation for Remote Extensions:**

1. Click Extensions in the sidebar
2. Search for the extension
3. Click **Install on SSH: falcon** (or your host)

Some extensions work locally only (they'll say "Local" when you search), while others work remotely (they'll say "Remote").

Useful Keyboard Shortcuts
------------------------------------

**Navigation and Editing**

```
Ctrl+P              Quick file open (start typing filename)
Ctrl+Shift+P        Command palette (search any command)
Ctrl+F              Find in file
Ctrl+H              Find and replace
Ctrl+D              Select next occurrence of word
Ctrl+K Ctrl+F       Format document
```

**Terminal**

```
Ctrl+`              Toggle integrated terminal
Ctrl+Shift+`        New terminal
```

**Remote Connection**

```
Ctrl+Shift+P then "Remote-SSH: Connect to Host"    Start remote session
Ctrl+Shift+P then "Remote-SSH: Close Remote"       Disconnect from remote
```

**Debugging (if using Python extension)**

```
F5                  Start debugging
F9                  Toggle breakpoint
F10                 Step over
F11                 Step into
Shift+F11           Step out
```

Common Workflows
------------------------------------

**Workflow 1: Edit, Test, Debug**

1. Connect to Falcon via Remote-SSH
2. Open your project folder
3. Make changes to your code in the editor
4. Open the integrated terminal (`Ctrl+``)
5. Run your code: `python script.py`
6. See output in the terminal
7. Set breakpoints by clicking line numbers
8. Press F5 to debug with stepping
9. Repeat until fixed

**Workflow 2: Submit SLURM Jobs**

1. Connect to Falcon
2. Edit your job script in VS Code
3. Open the terminal
4. Run `sbatch myjob.sh`
5. Monitor with `squeue -u $USER`
6. Edit and resubmit as needed

**Workflow 3: Interactive Session Development**

1. Connect to Falcon
2. Open terminal
3. Test commands interactively: `python -i script.py`
4. Iterate on code in the editor
5. Re-run with fresh imports

Troubleshooting
------------------------------------

**"Failed to authenticate" or password-related errors:**

- Make sure your VPN is on
- Check that your SSH config is correct (see :doc:`loggingin`)
- Try connecting directly: `ssh falcon` in a local terminal first
- Your Falcon password may have expired—check with Omar

**"Connection timeout":**

- Check your VPN connection
- Verify you can SSH normally: `ssh falcon`
- Try again after a minute (server might be slow)

**"Remote server failed to start":**

- Sometimes the `.vscode-server` folder gets corrupted
- Run this in your local terminal: `ssh falcon 'rm -rf ~/.vscode-server'`
- Reconnect from VS Code—it will reinstall

**Extensions not working on remote:**

- Some extensions only work locally
- Search for the extension and check if it shows "Install on SSH: falcon"
- If it only shows "Install", it's local-only

**Slow performance:**

- This is usually a network issue, not VS Code
- Check your VPN connection
- Try the :doc:`bash` guide to understand system performance
- Close unused editor tabs to reduce overhead

**Can't find Falcon in the host list:**

- Make sure your SSH config is set up (see :doc:`loggingin`)
- You can manually type the host: type "c.sglmn3@falconlogin.cf.ac.uk"
- Check that the SSH config file is at `~/.ssh/config`

Tips for Efficient Remote Development
------------------------------------

1. **Use integrated terminal**: It's faster than alt-tabbing between terminals
2. **Keep workspace files local**: VS Code can sync settings between machines
3. **Use Git for version control**: Clone your repo on Falcon and use the built-in Git tools
4. **Set up a .vscode folder**: Create `.vscode/settings.json` in your project for project-specific settings
5. **Use proper Python environments**: Activate virtual environments in the terminal before running code
6. **Commit frequently**: If something breaks, you can revert

Example Project Setup
------------------------------------

Here's a recommended folder structure for a project on Falcon:

```
~/projects/myproject/
├── .vscode/
│   └── settings.json          # Project-specific VS Code settings
├── .gitignore
├── README.md
├── data/
│   ├── inputs/                # Input files
│   └── outputs/               # Results
├── src/
│   ├── __init__.py
│   ├── analysis.py
│   └── utils.py
├── scripts/
│   └── process.py
├── job_scripts/
│   ├── job1.sh
│   └── job2.sh
└── results/
    └── .gitkeep
```

Then in VS Code:
1. Open the `~/projects/myproject/` folder
2. Edit files in the `src/` folder
3. Use the integrated terminal to run scripts
4. Save results to the `results/` folder
5. Commit to Git

Getting Help
------------------------------------

- **VS Code docs**: https://code.visualstudio.com/docs/remote/remote-ssh
- **Remote-SSH extension guide**: In VS Code, click Extensions, find Remote-SSH, view the README
- **Ask Omar or colleagues**: They likely have their own setup
- **Check the :doc:`loggingin` guide**: If you're having connection issues

With Remote-SSH, you get the best of both worlds: the power of Falcon's supercomputer and the comfort of VS Code on your local machine.