Bash Basics
========================================

Bash is the shell—the command-line interface you use to interact with your computer or remote systems like Falcon. This guide covers the most common and useful bash commands you'll need for scientific computing and file management.

Directory Navigation
------------------------------------

**pwd** — Print Working Directory
Shows the full path of the directory you're currently in.

.. code-block:: bash

   pwd

This is useful when you get lost and need to know where you are.

**cd** — Change Directory
Move to a different directory.

.. code-block:: bash

   cd /path/to/directory      # Navigate to an absolute path
   cd subfolder               # Navigate to a subfolder in current directory
   cd ..                      # Go up one directory level
   cd ../..                   # Go up two levels
   cd ~                       # Go to your home directory
   cd -                       # Go back to the previous directory

**ls** — List Directory Contents
Show what's in a directory. This command has many useful flags:

.. code-block:: bash

   ls                         # List files and folders (basic listing)
   ls -l                      # Long format (shows permissions, size, date, etc.)
   ls -a                      # Show all files (includes hidden files starting with .)
   ls -h                      # Human-readable file sizes (KB, MB, GB)
   ls -lah                    # Combine flags: long, all, human-readable
   ls -lah /path/to/dir       # List contents of a specific directory
   ls -lt                     # Sort by modification time (newest first)
   ls -lS                     # Sort by file size (largest first)
   ls -1                      # List one file per line
   ls *.txt                   # List only files matching a pattern

The permissions shown in ``ls -l`` look like ``-rw-r--r--``. The first character is the file type (``-`` for file, ``d`` for directory), followed by three groups of three characters for owner, group, and others (read/write/execute).

File Operations
------------------------------------

**mkdir** — Make Directory
Create new directories.

.. code-block:: bash

   mkdir foldername           # Create a single directory
   mkdir -p path/to/nested    # Create nested directories (creates parent directories if needed)
   mkdir dir1 dir2 dir3       # Create multiple directories at once

**cp** — Copy
Copy files and directories.

.. code-block:: bash

   cp file1.txt file2.txt     # Copy a file
   cp -r folder1 folder2      # Copy a directory and its contents (recursive)
   cp file.txt /path/to/dest  # Copy to a different directory
   cp *.txt backup/           # Copy all .txt files to a folder

**mv** — Move or Rename
Move files/folders or rename them.

.. code-block:: bash

   mv old_name.txt new_name.txt    # Rename a file
   mv file.txt /path/to/new/location  # Move a file to another directory
   mv -i old.txt new.txt            # Ask before overwriting (interactive)
   mv folder1 folder2                # Rename a folder

**rm** — Remove
Delete files. **Be careful—deletion is permanent!**

.. code-block:: bash

   rm file.txt                # Delete a single file
   rm file1.txt file2.txt     # Delete multiple files
   rm -i file.txt             # Ask before deleting (interactive/safe)
   rm -r folder/              # Delete a folder and all contents (recursive)
   rm -rf folder/             # Force delete without asking (be very careful!)

**touch** — Create an Empty File or Update Timestamp

.. code-block:: bash

   touch newfile.txt          # Create an empty file
   touch existing_file.txt    # Update the modification time without changing content

**cat** — Concatenate and Display
Show file contents or combine files.

.. code-block:: bash

   cat file.txt               # Display a file's contents
   cat file1.txt file2.txt    # Display multiple files
   cat file1.txt > combined.txt  # Combine files into a new file
   cat file.txt | head -20    # Show first 20 lines (more on pipes later)

**head** and **tail** — View Start or End of Files

.. code-block:: bash

   head file.txt              # Show first 10 lines
   head -20 file.txt          # Show first 20 lines
   tail file.txt              # Show last 10 lines
   tail -f file.txt           # Follow the file (useful for monitoring logs)

File Search and Filtering
------------------------------------

**grep** — Search Text
Find lines matching a pattern.

.. code-block:: bash

   grep "search_term" file.txt         # Find lines containing search_term
   grep -n "term" file.txt             # Show line numbers
   grep -i "term" file.txt             # Case-insensitive search
   grep -v "term" file.txt             # Invert match (show lines NOT matching)
   grep "^start" file.txt              # Lines starting with "start"
   grep -r "term" folder/              # Recursive search in folder
   grep "pattern" *.txt                # Search multiple files

**sed** — Stream Editor
Edit text in files (or streams). Common uses:

.. code-block:: bash

   sed 's/old/new/g' file.txt          # Replace all "old" with "new" (s = substitute, g = global)
   sed 's/old/new/' file.txt           # Replace only first occurrence per line
   sed '5d' file.txt                   # Delete line 5
   sed '1,10d' file.txt                # Delete lines 1-10
   sed -n '5,10p' file.txt             # Print only lines 5-10
   sed -i 's/old/new/g' file.txt       # Edit file in-place (saves changes)
   sed 's/old/new/g' file.txt > new.txt # Save to new file instead

**wc** — Word Count
Count lines, words, and characters.

.. code-block:: bash

   wc file.txt                # Show lines, words, characters
   wc -l file.txt             # Count lines only
   wc -w file.txt             # Count words only
   wc -c file.txt             # Count characters only
   wc -l *.txt                # Count lines in all .txt files

**find** — Search for Files
Locate files by name, type, size, etc.

.. code-block:: bash

   find . -name "*.txt"       # Find all .txt files in current directory and subdirectories
   find / -name "file.txt"    # Search entire system (slow!)
   find . -type f -name "*.py"  # Find files (-f) with .py extension
   find . -type d -name "folder"  # Find directories (-d)
   find . -size +10M          # Find files larger than 10MB
   find . -mtime -7           # Find files modified in last 7 days

Text Editors
------------------------------------

On remote systems, you'll often use a terminal text editor. The most common are **vi/vim** and sometimes **emacs** or **nano**. We'll focus on vim since it's nearly universal.

**Opening Files**

.. code-block:: bash

   vim filename.txt           # Open or create a file
   vi filename.txt            # vi (older version of vim)
   nvim filename.txt          # neovim (modern vim variant)

**Understanding Vim Modes**

Vim has two main modes:

- **Normal mode** (command mode): You use keys to navigate and execute commands. This is where you start when you open a file.
- **Insert mode**: You can type text. Press ``i`` to enter this mode, ``Esc`` to exit.

**Essential Vim Commands**

Navigate and View:

.. code-block:: text

   h, j, k, l          Arrow keys: left, down, up, right
   w                   Jump to next word
   b                   Jump to previous word
   e                   Jump to end of word
   0                   Jump to start of line
   $                   Jump to end of line
   gg                  Jump to start of file
   G                   Jump to end of file
   nG                  Jump to line n (e.g., 5G goes to line 5)
   Ctrl+u              Scroll up half page
   Ctrl+d              Scroll down half page
   :n                  Go to line n (e.g., :42)

Editing:

.. code-block:: text

   i                   Enter insert mode at cursor
   I                   Enter insert mode at start of line
   a                   Enter insert mode after cursor
   A                   Enter insert mode at end of line
   o                   Create new line below and enter insert mode
   O                   Create new line above and enter insert mode
   Esc                 Exit insert mode (back to normal mode)
   x                   Delete character at cursor
   dw                  Delete word
   d$                  Delete from cursor to end of line
   dd                  Delete entire line
   5dd                 Delete 5 lines
   D                   Delete from cursor to end of line (same as d$)
   u                   Undo last change
   Ctrl+r              Redo (undo the undo)

Copy and Paste (Yank in Vim terminology):

.. code-block:: text

   yy                  Yank (copy) entire line
   5yy                 Yank 5 lines
   yw                  Yank word
   p                   Paste after cursor
   P                   Paste before cursor

Search and Replace:

.. code-block:: text

   /search_term        Search forward for term
   ?search_term        Search backward for term
   n                   Go to next match
   N                   Go to previous match
   :%s/old/new/g       Replace all "old" with "new" in file
   :s/old/new/g        Replace in current line only
   :5,10s/old/new/g    Replace in lines 5-10 only

Saving and Exiting:

.. code-block:: text

   :w                  Write (save) file
   :q                  Quit (only if no unsaved changes)
   :q!                 Quit without saving (discard changes)
   :wq                 Write and quit
   :x                  Write and quit (alternative to :wq)
   ZZ                  Write and quit (no colon needed)

**A Simple Vim Workflow**

1. Open file: ``vim myfile.txt``
2. Press ``i`` to enter insert mode
3. Type or edit your text
4. Press ``Esc`` to exit insert mode
5. Type ``:wq`` to save and quit
6. Press ``Enter``

**If you get stuck in Vim:**

1. Press ``Esc`` multiple times to ensure you're in normal mode
2. Type ``:q!`` and press ``Enter`` to quit without saving

**Other Editors**

If vim feels overwhelming, here are alternatives:

- **nano**: Simpler but less powerful. Press ``Ctrl+X`` to exit, ``y`` to save.
- **emacs**: More complex but very powerful. Press ``Ctrl+X Ctrl+C`` to exit.
- **nvim** (neovim): Modern version of vim with better defaults. Most commands are the same.

Advanced Bash Concepts
------------------------------------

**Pipes** — Chain Commands Together
The pipe operator ``|`` sends the output of one command as input to another.

.. code-block:: bash

   cat file.txt | grep "search_term"        # Find lines in a file
   cat file.txt | wc -l                     # Count lines in a file
   ps aux | grep python                     # Find running Python processes
   ls -lah | grep "Jan"                     # List files from January

**Redirection** — Save Output to Files

.. code-block:: bash

   command > file.txt                       # Overwrite file with output
   command >> file.txt                      # Append output to file
   command < input.txt                      # Use file as input
   command 2> errors.txt                    # Redirect errors to file
   command &> all.txt                       # Redirect output and errors

**Wildcards** — Match Multiple Files

.. code-block:: bash

   *.txt                                    # All .txt files
   file?.txt                                # file with one character (file1.txt, file2.txt)
   file[0-9].txt                           # file with a digit
   file[abc].txt                           # file followed by a, b, or c

**source** — Run Commands from a File
Execute a shell script or configuration file.

.. code-block:: bash

   source ~/.bashrc                         # Load bash configuration
   . ~/.bashrc                              # Same as source (dot notation)
   source /path/to/script.sh                # Run all commands in a script

This is useful when you change your dotfiles and want to reload them without restarting your terminal.

**chmod** — Change File Permissions
Control who can read, write, and execute files.

.. code-block:: bash

   chmod u+x script.sh                      # Add execute permission for user
   chmod +x script.sh                       # Add execute permission for all
   chmod 755 script.sh                      # rwxr-xr-x (common for scripts)
   chmod 644 file.txt                       # rw-r--r-- (common for text files)

**which** and **whereis** — Find Commands
Locate where a command or program is installed.

.. code-block:: bash

   which python                             # Show path to python executable
   which vim                                # Show path to vim
   whereis python                           # Show multiple locations

**man** — Manual Pages
Read documentation for commands.

.. code-block:: bash

   man ls                                   # Read manual for ls
   man grep                                 # Read manual for grep
   man -k search_term                       # Search manual pages

Press ``q`` to exit a manual page.

**alias** — Create Command Shortcuts
Make shortcuts for long commands (usually in your dotfiles).

.. code-block:: bash

   alias ll='ls -lah'                       # Create shortcut
   alias rm='rm -i'                         # Confirm before deleting (safer)
   alias mypy='python3'                     # Create alternative name

Tips for Efficient Bash Usage
------------------------------------

- **Tab completion**: Press ``Tab`` to auto-complete file names and commands. Press twice to see all options.
- **Command history**: Press ``Up`` and ``Down`` arrows to recall previous commands.
- **Search history**: Press ``Ctrl+R`` and type to search your command history. Press ``Ctrl+R`` again to find next match.
- **Clear screen**: Type ``clear`` or press ``Ctrl+L``
- **Stop a running command**: Press ``Ctrl+C``
- **Suspend a process**: Press ``Ctrl+Z`` (you can resume with ``fg``)
- **Comment**: Lines starting with ``#`` are comments and won't execute

Common Bash Gotchas
------------------------------------

**Spaces matter**: ``cd my folder`` is different from ``cd myfolder``. Use quotes: ``cd "my folder"`` or escapes: ``cd my\ folder``

**Case sensitive**: ``File.txt`` and ``file.txt`` are different files on Unix/Linux.

**Wildcards don't work in quotes**: ``ls "*.txt"`` looks for a file literally named ``*.txt``, not all .txt files. Use ``ls *.txt`` instead.

**The dot is important**: ``./script.sh`` runs a script in the current directory. ``script.sh`` without ``./`` searches your PATH.

**Quotes matter for variables**: ``echo $VAR`` expands the variable. ``echo '$VAR'`` prints ``$VAR`` literally.

Practice
------------------------------------

Try these commands to get comfortable:

.. code-block:: bash

   mkdir test_folder                        # Create a test folder
   cd test_folder                           # Enter it
   touch file1.txt file2.txt               # Create two files
   echo "Hello world" > file1.txt           # Add content
   cat file1.txt                            # View content
   cp file1.txt file3.txt                   # Copy it
   ls -lah                                  # See all files with details
   rm file2.txt                             # Delete one file
   cd ..                                    # Go back up
   rm -r test_folder                        # Delete the test folder

Need Help?
------------------------------------

- Use ``man command`` to read documentation
- Google the error message
- Ask Omar or a colleague
- Check your dotfiles for examples of how others use bash
