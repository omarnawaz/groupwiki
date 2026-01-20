Logging Into Falcon for the First Time
========================================

This guide will walk you through connecting to the Falcon supercomputer. We'll cover SSH basics, configuring your connection, setting up your environment, and transferring files.

Prerequisites
------------------------------------

Before you start, make sure you:

- Have completed the registration steps in the :doc:`access` guide
- Have VPN access to Cardiff University
- **Have turned on your VPN** (seriously, this is required for every connection)
- Have a terminal open on your local machine

Understanding SSH
------------------------------------

SSH (Secure Shell) is a way to securely log into a remote computer over the internet. When you SSH into Falcon, you're opening a secure connection to the supercomputer and getting a shell (command prompt) where you can type commands.

The basic SSH command looks like this:

```bash
ssh username@hostname
```

For Falcon, this would be:

```bash
ssh yourfacadeusername@falconlogin.cf.ac.uk
```

When you run this command:

1. SSH asks for your password
2. You type your password (it won't show on screen—this is normal)
3. You get a command prompt on the remote machine
4. You can now type commands that run on Falcon instead of your local computer

Your First Login
------------------------------------

1. **Make sure your VPN is on**—check your system settings to confirm
2. Open your terminal
3. Type the SSH command:

   ```bash
   ssh yourfacadeusername@falconlogin.cf.ac.uk
   ```

4. Press Enter
5. When prompted, enter your Falcon password
6. You should now see a prompt that looks something like `[yourfacadeusername@falconlogin ~]$`

Congratulations! You're now logged into Falcon.

To log out, type:

```bash
exit
```

or press `Ctrl+D`

Simplifying Login with SSH Config
------------------------------------

Typing the full hostname every time is tedious. SSH has a configuration file where you can define shortcuts. You can create a file at `~/.ssh/config` that makes logging in much easier.

Here's an example configuration for Falcon (you can reference Omar's full SSH configuration at `~/.ssh/config`):

```text
Host falcon
  HostName falconlogin.cf.ac.uk
  User yourfacadeusername
  ServerAliveInterval 240
```

With this configuration in place, you can now simply type:

```bash
ssh falcon
```

and you'll be connected to the same system.

The `ServerAliveInterval 240` line tells SSH to send a signal every 240 seconds to keep your connection alive if you're idle—useful for long-running sessions.

You can add multiple hosts to your `~/.ssh/config` file for different systems you connect to. If you want to see more examples, check out Omar's dotfiles repository.

Setting Up Your Environment with Dotfiles
------------------------------------

When you log into a system, several startup files run to configure your environment. These typically include:

- `.bashrc` - Bash configuration file
- `.bash_profile` - Bash login configuration
- `.zshrc` - Zsh configuration file (if using Zsh)

These files can contain:

- **Exports**: Setting environment variables (like `PATH`, `PYTHONPATH`, etc.)
- **Aliases**: Shortcuts for common commands (e.g., `alias ll='ls -lah'`)
- **Functions**: Custom commands you can run
- **Module loads**: Loading necessary software (modules on HPC systems)

Rather than manually editing these files on the remote system, many developers maintain "dotfiles"—a repository of configuration files that can be cloned and installed on any system.

Omar maintains a dotfiles repository at:

https://github.com/omarnawaz/dotfiles

This repository contains configurations for:

- Shell aliases and functions
- Environment variable exports
- Useful utilities for working on HPC systems
- Development tool configurations

You can explore this repository to see useful patterns you might want to adopt. Some common dotfile elements include:

**Aliases** (shortcuts for commands):

```bash
alias ll='ls -lah'
alias gst='git status'
alias gco='git checkout'
```

**Exports** (environment variables):

```bash
export PYTHONPATH="/path/to/your/python:$PYTHONPATH"
export OMP_NUM_THREADS=4
```

**Functions** (custom commands):

```bash
function mkcd() {
  mkdir -p "$1" && cd "$1"
}
```

On Falcon, your startup files will be on the remote system. You can edit them directly on Falcon or clone your own dotfiles repository into your Falcon home directory and source them from your `.bashrc` or `.zshrc`.

Transferring Files with SCP
------------------------------------

Now that you can log in, you'll probably want to transfer files between your local machine and Falcon. The `scp` command (Secure Copy) lets you do this.

**Copying a file FROM your local machine TO Falcon:**

```bash
scp /path/to/local/file falcon:/path/to/remote/destination
```

For example, to copy a script called `analysis.py` to your home directory on Falcon:

```bash
scp ./analysis.py falcon:~/
```

**Copying a file FROM Falcon TO your local machine:**

```bash
scp falcon:/path/to/remote/file /path/to/local/destination
```

For example, to copy results from Falcon to your local machine:

```bash
scp falcon:~/results/output.txt ./
```

**Copying a whole directory:**

Add the `-r` flag (recursive) to copy directories:

```bash
scp -r falcon:~/mydata ./mydata_backup
```

**A few tips:**

- `~` on the remote system refers to your home directory on Falcon
- `./` on the local system refers to the current directory on your local machine
- You can use wildcards: `scp falcon:~/data/*.txt ./` copies all `.txt` files
- If you set up an SSH config entry for `falcon` (as shown above), SCP will automatically use those settings

Workflow Example
------------------------------------

Here's a typical workflow:

1. Write code locally in your favorite editor
2. Copy it to Falcon using SCP:

   ```bash
   scp ./myanalysis.py falcon:~/projects/
   ```

3. Log into Falcon:

   ```bash
   ssh falcon
   ```

4. Run your code on Falcon's compute resources
5. Copy results back to your local machine:

   ```bash
   scp falcon:~/projects/results.txt ./
   ```

6. Log out:

   ```bash
   exit
   ```

Troubleshooting
------------------------------------

**"Connection refused" or "Network unreachable":**
- Check that your VPN is on
- Verify you can access other Cardiff resources

**"Permission denied (publickey,password)":**
- Check that your username is correct in your SSH config
- Verify your Falcon password hasn't expired
- Make sure you're using your Falcon username, not your Cardiff username

**"Connection closed by remote host":**
- Your connection may have timed out
- Simply reconnect with `ssh falcon`

**SCP says "No such file or directory":**
- Double-check your file paths
- Remember that `~` refers to your home directory on the remote system
- Use `pwd` to check your current location

Need Help?
------------------------------------

If you run into issues:

- Check the :doc:`access` guide for account setup issues
- Ask Omar or another group member for help
- Check the `Falcon documentation <https://wiki.arcca.cf.ac.uk/index.php/Falcon>`_
